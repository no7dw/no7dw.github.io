title: haprxoy
date: 2016-04-29 23:33:16
tags:
---
# Haproxy configuration and Log details

---

HAProxy is a free, very fast and reliable solution offering high availability, load balancing, and proxying for TCP and HTTP-based applications

high availability
load balancing
proxying

download from this
tar zxvf haproxy-1.x.x.tar.gz
cd haproxy-1.x.x
make TARGET=xxx (xxx需要指定OS, linux26 注意配置use_xx 变量 如：ssl)
sudo make install
copy script to /etc/init.d/haproxy
启动不成功：Edit /etc/default/haproxy and make sure it has a line that says ENABLED=1 in it.
The default is ENABLED=0. This is done because haproxy has no sane default configuration, so you need to first configure it, then enable it.

