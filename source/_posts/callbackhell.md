title: callbackhell
date: 2015-10-27 23:10:51
tags:
---

## 使用 callback 注意事项  ##  
callback hell 解决：

 - q, bluebird , 
 - async, thenjs 
 - eventproxy 
 - promise, thunk


----------


## 使用 async 解决 callback ，写法上注意事项  ##  

 - long Procedure in async.waterfall
 - use event:

    event_util.emit("cashflow_over_quota", 'create', JSON.stringify(cashflow));


