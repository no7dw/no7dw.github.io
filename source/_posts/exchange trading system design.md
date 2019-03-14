exchange trading system design

用户 1 --》n 账户 account
账户 1 --》n 订单 order
订单 1 --》n 账单流水ledger

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

rest api 设计
  - version control v1 v2 v3 in url
2018-09-12T07:46:19.435ZGET/api/spot/v3/accounts/btc/ledger?limit=1&from=2&to=4 check <best practices for designing  web api>
  - 

注意所有的数字在传输的时候都是 string，防止数据在加密过程中容易出错，并保留固定位数。（防止 2.00 与 2.000 有差别）

performance metric

错误码

安全

交易撮合

关于时间 UTC String(Thu, 14 Mar 2019 08:06:39 GMT) vs timestamp (1552550799091) vs ISODate (2019-03-14T08:06:39.091Z)
建议 ISODate

 - 优点：比较直观的是ISODate , 通用，便于时区识别 
 - 缺点：排序稍差于 timestamp



ref :

[okex api](https://www.okex.com/docs/zh)

[huobi api](https://github.com/huobiapi/API_Docs/wiki/REST_api_reference)

