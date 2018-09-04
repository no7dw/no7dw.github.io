api https://www.bumeng.cn/api.html
重点:
asset account 
account->AddBalance
account->RecvAsset

LedgerFrm::Apply
    transactionFrm->PayFee
    会调用 AddBalance 负的

Consensus
GlueManager::SendConsensusMessage
    send via peerInstance -> sync -> check -> add to queue
