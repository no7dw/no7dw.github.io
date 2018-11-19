api https://www.bumeng.cn/api.html

重点:
asset account
account->AddBalance
account->RecvAsset

LedgerFrm::Apply
 - transactionFrm->PayFee
 - 会调用 AddBalance 负的

Consensus
GlueManager::SendConsensusMessage
 -  send via peerInstance -> sync -> check -> add to queue

FAQ:
 - 会用到用db sql & leveldb(kv)
 - 请求有服务器，有回调，譬如发行资产可以选异步方式

账户
 - 注册
 - 查询账户信息等
资产操作
 - 发行资产
 - 转移资产
 - 增发资产
 - 账户信息等

架构体系里面有架构图，涉及big data，但src code 没有big data mining (opt)
todo:
 - 根据时序走一遍代码
 - build and run 环境

Q
 - big data mining
 - db
 - compare with bitcoin
