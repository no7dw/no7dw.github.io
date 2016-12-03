title: RPC_Comparation
date: 2016-02-19 11:15:30
tags:
---
# REST SOAP Thrift 对比

[别人的][1]REST SOAP Thrift对比：

                REST        SOAP    Thrift
Extensibility   ☆☆☆☆☆     ☆☆☆      ☆
Neutrality      ☆☆       ☆☆☆☆     ☆☆☆
Independence    ☆        ☆☆☆      ☆☆☆☆
Large Data Handling      ☆        ☆☆☆☆☆   ☆☆☆
Scalability     ☆☆☆☆     ☆☆       ☆☆☆☆☆
Portability     ☆☆☆      ☆        ☆☆☆☆
Simplicity      ☆☆☆      ☆☆       ☆☆☆☆☆
Speed   ☆☆☆     ☆        ☆☆☆☆☆ 
Evolution       ☆☆       ☆        ☆☆☆☆☆


简单总结：

  - 如果你的系统对多语言要有支持、响应速度、并发上有高要求，建议选择Thrift。
  - 如果你的系统多语言要求不高、简单、速度、并发不高要求，无状态要求(HTTP)，REST的方式就够用了。
  - SOAP 的方式综合起来不是太推荐。除了他凸显的场景：严格的交互、有状态、要授权（也可以先授权，再将授权token 附带每次的请求）。

[其它情况对比文章][2]关于websocket, MQTT, SDK等选择


[REST vs SOAP 对比的不错的图文。][3]

[git src][4]


  [1]: http://nordicapis.com/microservice-showdown-rest-vs-soap-vs-apache-thrift-and-why-it-matters/
  [2]: http://www.programmableweb.com/news/rest-losing-its-flair-rest-api-alternatives/analysis/2013/12/19
  [3]: http://nordicapis.com/rest-vs-soap-nordic-apis-infographic-comparison/
  [4]: https://github.com/no7dw/thrift-demo
