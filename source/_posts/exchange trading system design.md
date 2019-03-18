### exchange trading system design 交易所系统设计

#### model关系

用户 1 --》n 账户 account
账户 1 --》n 订单 order
订单 1 --》n 账单流水ledger

#### model设计关键

订单order: 
type 类型 :
  - market order
  - limit order
方向 side:
  - buy 
  - sell
ID:
  - 订单id order_id
  - 用户设置的订单ID（系统foreign key to join）client_id
状态 status：
 - submitted（始态）
 - open
 - filled（终态）
 - partial-filled
 - canceled（终态）
 - partial-canceled（终态）

账单流水ledger：
 - balance 余额
 - order_id 关联foreign key
 - amount 变动的量
 - currency 币种(数字货币&外汇比较重要)
 - type 流水来源/类型
    - trade 交易
    - transfer 转账
    - fee 手续费
    - rebate 返佣

账户
  - ID 账户ID
  - balance  余额
  - hold 冻结
  - available  可用余额
  - currency 币种
  - type/name  用于区分用途

钱包账户-- 可用理解成总账户,与对外的流水入口。譬如充值、提现关联.
参考 okex 账户体系
 ![](https://support.okex.com/hc/article_attachments/360002553091/mceclip2.png)



每次的流水都记录当前的balance

holding:持仓，指数量(股数)，跟market price不挂钩

portfolio

product & instrument

#### rest api 设计
  - version control v1 v2 v3 in url
2018-09-12T07:46:19.435ZGET/api/spot/v3/accounts/btc/ledger?limit=1&from=2&to=4 check <best practices for designing  web api>
  - 

注意所有的数字在传输的时候都是 string，防止数据在加密过程中容易出错，并保留固定位数。（防止 2.00 与 2.000 有差别）

#### performance metric
  - return
  - exposure
  - unrealized return

#### 错误码

#### 安全

#### 交易撮合

关于时间 UTC String(Thu, 14 Mar 2019 08:06:39 GMT) vs timestamp (1552550799091) vs ISODate (2019-03-14T08:06:39.091Z)
建议 ISODate

 - 优点：比较直观的是ISODate , 通用，便于时区识别 
 - 缺点：排序稍差于 timestamp

#### 币币交易

#### 期货/合约交易


### 数据

#### 数据存储：
  - 区分热数据、冷数据
    - 热数据：最近的、要实时快的
    - 冷数据：稍久的，允许稍长一点的时延

#### kline 的数据存储
  - 按频率区分开表格存 
  - 不用筛选，快速

#### 数据真的太多了怎么办
  - 某日清算正确后，可以将balance 等关键信息汇总至某日，作为snapshot。以后基于这个数据去计算
  - 某日清算正确前的数据，把数据存到冷存储，如磁带机(多份)


容量估计：
  - 带宽
  - 数据
  - 系统压力

关键子系统概述:
 - 行情系统
 - 交易系统
 - 风控系统
 - 营销系统
 
量化策略系统
 - 行情系统
 - 交易系统
 - 风控系统
 - 策略系统



ref :

[okex api](https://www.okex.com/docs/zh)

[huobi api](https://github.com/huobiapi/API_Docs/wiki/REST_api_reference)

