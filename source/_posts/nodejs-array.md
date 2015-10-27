title: nodejs-array
date: 2015-10-27 23:10:03
tags:
---
## 使用nodejs 数组坑  ##

async 大数组（>5000）, 会报堆栈溢出 Max stack size。
解决：

 -  setImmediate,
 -  --stack-size=32000 , 启动传此参数


