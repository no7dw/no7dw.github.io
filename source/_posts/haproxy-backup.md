title: haproxy-backup
date: 2016-03-16 23:41:43
tags:
---
# haproxy backup 功能
 ---
 平常haproxy 作为load balance ，还有一种稍特殊的情况：有个节点A，平时用作唯一的服务，但为了避免单点故障，节点B并不是作为其中一个balance 节点，而是充当A的备份－－－即当A 挂掉时，B顶替A作为唯一的服务存在。
 
###simply config
只需要在server 节点后面加 **backup**

    
    backend www
        redirect scheme https if !{ ssl_fc }
        balance leastconn
        option httpclose
        option forwardfor
        cookie JSESSIONID prefix
        
        server      www host1:7601 weight 100 check inter 1500 rise 2 fall 3 
        server      bck host2:7601 weight 100 check inter 1500 rise 2 fall 3 backup 

效果图是蓝色的条条：
![此处输入图片的描述][1]
见[官网demo][2]


  [1]: http://7xk67t.com1.z0.glb.clouddn.com/backup.png
  [2]: http://demo.haproxy.org/
