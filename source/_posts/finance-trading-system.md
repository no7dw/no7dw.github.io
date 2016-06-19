title: finance trading system
date: 2016-06-19 11:48:31
tags:
---
# finance trading system

---

Basic traind system is about buy and sell. all transactions in money market is base on buy and sell finance instruments.
Basic elements is **price**. For example at stock market : I wanna buy : IBM  at price 20$ for one share. Someone wanna sell IBM at price 20.5$.
Any we have many users bid or offer at diffenent price, the situation may like this.
![stock price bid and offer][1]

In Software system, usually we'll store the bid and offer order info(price and share to bid/sell) in memory, classify by different price.

Another important elements is **time**. For example at stock market : IBM current price is 19.90$ , the next tick is risen to 19.91$, or fallen to 19.89$.
sinace we have data with time ,so visuial data may like this:
![ibm stock price in years][2]

so matching order the elementary part of trading system. 
![order system][3]
as the picture show that ,  we have limit order book (LMT) and market order book (MKT)
to explain in a simple way: 
[limit order][4] is a buy/sell order limited with certain price.
[market order][5] is a buy/sell order without price limited, it's price is at current time.


  [1]: http://www.onestepremoved.com/wp-content/uploads/2013/08/limit-order-book-pretrade-1024x622.png
  [2]: https://media.ycharts.com/charts/dc38386e4b610c3463cec05b08f149db.png
  [3]: https://www.ece.cmu.edu/~ece749/teams-06/team3/images/flow_chart_FTEX.jpg
  [4]: http://www.investopedia.com/university/intro-to-order-types/limit-orders.asp
  [5]: http://www.investopedia.com/university/intro-to-order-types/market-orders.asp
