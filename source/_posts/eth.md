# ethereum 的Peer Node 如何进行的信息的交互

流程
1 开启一个注册节点, 让这个注册节点p 在协程里面处理广播消息
2 p根据不同的chan读取广播消息,并进行处理 
消息类型涵盖：
  - StatusMsg:
  - GetBlockHeadersMsg:
  - BlockHeadersMsg:
  - GetBlockBodiesMsg:
  - BlockBodiesMsg:
  - NewBlockHashesMsg:
  - NewBlockMsg:
  - TxMsg:

2.1 queueTxs 等待广播的信息

2.1.1 queueTxs 的消息来源： go pm.txBroadcastLoop()
2.1.2 peer 维护这一个待发送交易数组
2.1.3 已知的交易 在knowTxs

2.2 queueProps 传播新的block
2.3 queueAnns 挖到？创建新的block

//1 peer.go
```
// Register injects a new peer into the working set, or returns an error if the
// peer is already known. If a new peer it registered, its broadcast loop is also
// started.
func (ps *peerSet) Register(p *peer) error {
	ps.lock.Lock()
	defer ps.lock.Unlock()

	if ps.closed {
		return errClosed
	}
	if _, ok := ps.peers[p.id]; ok {
		return errAlreadyRegistered
	}
	ps.peers[p.id] = p
	go p.broadcast()

	return nil
}
```

//2 peer.go
```
// broadcast is a write loop that multiplexes block propagations, announcements
// and transaction broadcasts into the remote peer. The goal is to have an async
// writer that does not lock up node internals.
func (p *peer) broadcast() {
	for {
		select {
		case txs := <-p.queuedTxs:
			if err := p.SendTransactions(txs); err != nil {
				return
			}
			p.Log().Trace("Broadcast transactions", "count", len(txs))

		case prop := <-p.queuedProps:
			if err := p.SendNewBlock(prop.block, prop.td); err != nil {
				return
			}
			p.Log().Trace("Propagated block", "number", prop.block.Number(), "hash", prop.block.Hash(), "td", prop.td)

		case block := <-p.queuedAnns:
			if err := p.SendNewBlockHashes([]common.Hash{block.Hash()}, []uint64{block.NumberU64()}); err != nil {
				return
			}
			p.Log().Trace("Announced block", "number", block.Number(), "hash", block.Hash())

		case <-p.term:
			return
		}
	}
}
```


//2.1 peer.go 调用p2p层进行网络交互
```
// SendTransactions sends transactions to the peer and includes the hashes
// in its transaction hash set for future reference.
func (p *peer) SendTransactions(txs types.Transactions) error {
	for _, tx := range txs {
		p.knownTxs.Add(tx.Hash())
	}
	return p2p.Send(p.rw, TxMsg, txs)
}
```

//2.1 ProtocolManager start 
调度 go pm.txBroadcastLoop() 类似处理BitCoin 的 UTXO
2.1.1 在PeersWithoutTx()  进行判断是否在knowTxs；
2.1.2 不在则准备发送AsyncSendTransactions (txs)  --- 从 ququeTxs 里面读取到 knowTxs ，然后最终通过p2p层发送出去
```
// BroadcastTxs will propagate a batch of transactions to all peers which are not known to
// already have the given transaction.
func (pm *ProtocolManager) BroadcastTxs(txs types.Transactions) {
	var txset = make(map[*peer]types.Transactions)

	// Broadcast transactions to a batch of peers not knowing about it
	for _, tx := range txs {
		peers := pm.peers.PeersWithoutTx(tx.Hash())
		for _, peer := range peers {
			txset[peer] = append(txset[peer], tx)
		}
		log.Trace("Broadcast transaction", "hash", tx.Hash(), "recipients", len(peers))
	}
	// FIXME include this again: peers = peers[:int(math.Sqrt(float64(len(peers))))]
	for peer, txs := range txset {
		peer.AsyncSendTransactions(txs)
	}
}
```

