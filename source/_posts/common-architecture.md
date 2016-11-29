title: common-architecture
date: 2016-11-28 21:14:24
tags:
---

### most easy
Load balancer (nginx/haproxy) + x Node + DB
can support according around 1k rps (x=1) for helloworld web without DB query
on ECS(2CPU 1G Mem)

### most common seen
Load balancer (nginx/haproxy) + x Node + redis + DB
can support rps : unknown

### most common seen for high speed demand 
Load balancer (nginx/haproxy) + varnish + x Node + redis + DB

### most stable & common seen demand
LVS/Keepalived + Load balancer (nginx/haproxy) + varnish + x Node + redis + DB

### advance db config
Load balancer (nginx/haproxy) + varnish + x Node + redis + DB(master/slave)

### stable and advance 
LVS/Keepalived + Load balancer (nginx/haproxy) + varnish + x Node + redis + DB

### advance 
DNS Load balancer + LVS/Keepalived + Load balancer (nginx/haproxy) + varnish + x Node + redis + DB

### DB is busy: depart DB according to business
DB(order) + DB(user) + DB(other)
DNS Load balancer + LVS/Keepalived + Load balancer (nginx/haproxy) + varnish + x Node + redis + DB(order) + DB(user) + DB(other)

ref:
[大型网站架构系列：电商网站架构案例](http://blog.jobbole.com/97951/)
[大型网站架构演变和知识体系](http://www.blogjava.net/BlueDavy/archive/2008/09/03/226749.html)
[大型网站架构演化历程](http://www.hollischuang.com/archives/728)

