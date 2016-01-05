title: ant-attack
date: 2016-01-05 22:04:46
tags:
---
## to prevent from attack from as root-chick
## do such things


### not allow ping
 iptables -A INPUT -p icmp --icmp-type 8 -s 0/0 -j DROP

### not allow ssh from exclude ip
 edit /etc/hosts.deny /etc/hosts.allow

### not use 22 as ssh port

## use cdn to prevent server ip explode
