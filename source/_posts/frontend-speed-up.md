title: frontend-speed-up
date: 2016-03-16 00:37:49
tags:
---
# 从一个请求到服务器的过程看前端优化
---
了解[url 回车后到页面展现发生什么][1]之后,(还有个变态详细的[版本][2])。对整个过程的理解，也就理解了这里的：[yahoo 前端优化法则][3]
整个过程稍微简化程图，可以拿chrome 的timeline 
![此处输入图片的描述][4]
[每个时间点][5]都可以减少,主要的时间点与军规结合理解如下：

 - 首先减少请求：对应军规：减少HTTP请求
 - 不请求：缓存 ,Cache-Control, 用外部JavaScript和CSS
 - 发送量减少:减小Cookie
 - 接收量减少:ETag/304 Not Modified, uglify/min.js 压缩, gzip 压缩
 - 传输更快： cdn、减少DNS查找


  [1]: http://blog.aijc.net/server/2015/11/03/%E4%BB%8E%E8%BE%93%E5%85%A5URL%E5%88%B0%E9%A1%B5%E9%9D%A2%E5%8A%A0%E8%BD%BD%E5%AE%8C%E7%9A%84%E8%BF%87%E7%A8%8B%E4%B8%AD%E9%83%BD%E5%8F%91%E7%94%9F%E4%BA%86%E4%BB%80%E4%B9%88%E4%BA%8B%E6%83%85/
  [2]: http://fex.baidu.com/blog/2014/05/what-happen/
  [3]: http://www.html-js.com/article/Front-end-performance-optimization-YAHOO-34-performance-rule%203032
  [4]: http://7xk67t.com1.z0.glb.clouddn.com/timeline.png
  [5]: https://developers.google.com/web/tools/chrome-devtools/profile/network-performance/resource-loading#view-network-timing-details-for-a-specific-resource
