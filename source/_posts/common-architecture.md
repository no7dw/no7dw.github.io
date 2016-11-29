title: common-architecture
date: 2016-11-28 21:14:24
tags:
---

### 1 most easy
最基础、部署最快的web 应用架构
Load balancer (nginx/haproxy) + x Node + DB
can support according around 1k rps (x=1) for hello world web without DB query
on ECS(2Core 1G Mem)
![level0][1]

### 2 most common seen
DB有压力后，分担db查询的压力
Load balancer (nginx/haproxy) + x Node + redis + DB


### 3 most common seen for high speed demand 
web 请求上来后，前端静态文件是需要缓存的，某些高频延时稍高接口返回
Load balancer (nginx/haproxy) + varnish + x Node + redis + DB
![此处输入图片的描述][2]

### 4 more stable & common seen demand
单体的大节点需要拆分成小的业务节点
LVS/Keepalived + Load balancer (nginx/haproxy) + varnish + x Node + redis + DB
![此处输入图片的描述][3]

### 5 advance db config
db要求高稳定性，需要搭建db集群
Load balancer (nginx/haproxy) + varnish + x Node + redis + DB(master/slave)


### stable and advance 
防止入口机器挂掉，使用keepalived，相当于复制了一份入口的backup
LVS/Keepalived + Load balancer (nginx/haproxy) + varnish + x Node + redis + DB
![此处输入图片的描述][4]

### 6 dns advance 
单台机器作为入口已经支撑不起流量，需要dns 按区域或者按运营商分担入口压力
DNS Load balancer + LVS/Keepalived + Load balancer (nginx/haproxy) + varnish + x Node + redis + DB

### 7 DB is busy: depart DB according to business
db 繁忙，需要拆分单个db 到多个业务db
DNS Load balancer + LVS/Keepalived + Load balancer (nginx/haproxy) + varnish + x Node + redis + DB集群(order) + DB集群(user) + DB集群(other)

db压力减少，解决：

 - 使用集群
 - 拆分业务
 - 拆分实时要求、离线要求
 - 读写分离
 - 使用Hadoop 等技术提高数据的处理能力
 - 利用NoSQL减少跨表的查询

整体总结：

 - 大部分稳定性问题都是通过冗余在解决单点问题
 - 通过前置缓存来分担应用、db的压力
 - 通过分担机器压力解决单台机器的压力
 

our current status:
![current architeture][5] 
根据业务量情况，支撑目前是大约压力测试结果：

 - 300 RPS （涉及db）
 - 3K同时在线
机器4台ECS(4Core 8G) 比较稳定，<20% cpu 占用。
线下hadoop集群的离线处理未添加在图中。

整个过程是不断演变的，当前状态还没有完。

### 版本发布：
多台机器的时候，逐个发布已经比较麻烦了，解决：

 - jenkins + 脚本，集成一键发布
 - jenkins + docker


### 利用缓存改善网站性能
web 永远是“缓存为王”
ELK 的统计数据显示请求，70%的request 都是请求静态资源，解决参考 [static file caching][6]




ref:
[大型网站架构系列：电商网站架构案例](http://blog.jobbole.com/97951/)
[大型网站架构演变和知识体系](http://www.blogjava.net/BlueDavy/archive/2008/09/03/226749.html)
[大型网站架构演化历程](http://www.hollischuang.com/archives/728)


  [1]: http://7xk67t.com1.z0.glb.clouddn.com/architecture-0.jpeg
  [2]: http://7xk67t.com1.z0.glb.clouddn.com/architecture-1.jpeg
  [3]: http://7xk67t.com1.z0.glb.clouddn.com/architecture-2.jpeg
  [4]: http://7xk67t.com1.z0.glb.clouddn.com/architecture-3.jpeg
  [5]: http://7xk67t.com1.z0.glb.clouddn.com/architecture.jpeg
  [6]: http://www.deng.io/2016/11/14/static-file-cache/