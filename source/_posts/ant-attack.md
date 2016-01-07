title: ant-attack
date: 2016-01-05 22:04:46
tags:
---
## to prevent from attack from as zoombie computer, do such things:

phenomemon: network outgoing is extremely huge ,comparing to usual:
![此处输入图片的描述][1]
and often , your VPS provider will have to disable your network.

### not allow ping
 iptables -A INPUT -p icmp --icmp-type 8 -s 0/0 -j DROP

### not allow ssh from exclude ip
 edit /etc/hosts.deny 
 

    sshd:All

 edit /etc/hosts.allow 
 

    sshd:your_allow_ip

### not use 22 as ssh port
 edit /etc/ssh/sshd_config

    Port xxxx


### use cdn to prevent server ip explode
ref to: [cdn your website network][2]


  [1]: http://7xk67t.com1.z0.glb.clouddn.com/outgoing-is-huge.png
  [2]: http://www.deng.io/2015/12/31/cdn-setting/
