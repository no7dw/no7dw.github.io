title: finance trading system
date: 2016-06-19 11:48:31
tags:
---
# finance trading system

---


Basic trading system is about buy and sell. all transactions in money market is base on buy and sell finance instruments.

#### price

Basic elements is **price**. For example at stock market : I wanna buy : IBM  at price 20$ for one share. Someone wanna sell IBM at price 20.5$.
Any we have many users bid or offer at diffenent price, the situation may like this.
![stock price bid and offer][1]

In Software system, usually we'll store the bid and offer order info(price and share to bid/sell) in memory, classify by different price.

#### time

Another important elements is **time**. For example at stock market : IBM current price is 19.90$ , the next tick is risen to 19.91$, or fallen to 19.89$.
sinace we have data with time ,so visuial data may like this:
![ibm stock price in years][2]

#### order 
so matching order the elementary part of trading system. 
![order system][3]
as the picture show that ,  we have limit order book (LMT) and market order book (MKT)
to explain in a simple way: 
[limit order][4] is a buy/sell order limited with certain price.
[market order][5] is a buy/sell order without price limited, it's price is at current time.

### advance part: executing order
as we see in fig #1 , we place the buy/sell order(with how many shares client wanna perform ) according to different price.
When we match those order at some curtain price, the next task for system is executing those price matched order.
Let's going a little deeper with an example. 
Orders: 

 - 1 client A buy IBM at 20$ with 10 shares
 - 2 client B sell IBM at 20$ with 5 shares
 - 3 client C sell IBM at 20$ with 2 shares

so when complete executing these 3 orders, the buy order is complete 7 shares ,and 3 shares left.

But during we executing order,  client A  may cancel the order. This may cause order#2 complete, while order#3 is not.

check for more about [security trading system at shenzhen][6]
[trading rule of security trading system at shenzhen][7]
[order matching principles][8]


  [1]: http://www.onestepremoved.com/wp-content/uploads/2013/08/limit-order-book-pretrade-1024x622.png
  [2]: https://media.ycharts.com/charts/dc38386e4b610c3463cec05b08f149db.png
  [3]: https://www.ece.cmu.edu/~ece749/teams-06/team3/images/flow_chart_FTEX.jpg
  [4]: http://www.investopedia.com/university/intro-to-order-types/limit-orders.asp
  [5]: http://www.investopedia.com/university/intro-to-order-types/market-orders.asp
  [6]: http://www.szse.cn/
  [7]: http://www.szse.cn/UpFiles/largepdf/20160429091100.pdf
  [8]: http://www.eurexchange.com/exchange-en/trading/market-model/matching-principles
