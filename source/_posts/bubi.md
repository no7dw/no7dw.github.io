title: bubi 布萌区块链数字资产网络 分析 
date: 2018-08-15 12:33:10

[api readme]( https://www.bumeng.cn/api.html)

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


FAQ:
会用到用db sql & leveldb(kv)
请求有服务器，有回调，譬如发行资产可以选异步方式
![img](https://github.com/bubichain/bubichain/blob/master/doc/tx_flow.png)
    以api 页面的获取查询的账户为例
    getAccount:
    web-server ->  router-> getaccount -> ledgerManager::GetAccountEntry(address, acc) -> Storage::->db->Get()
账户
  - 注册
  - 查询账户信息等
资产操作
  - 发行资产
  - 转移资产
  - 增发资产
  - 账户信息等

架构体系里面有架构图，但src code 没有big data mining (opt)
todo:
根据时序走一遍代码
build and run 环境

Q:
  - big data mining
  - db
  - compare with bitcoin
