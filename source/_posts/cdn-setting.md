title: cdn-setting
date: 2015-12-31 01:29:25
tags:
---
# cdn 你的网站

------
购买一个域名(此为deng.io)，以namecheap 为例子，
购买一个机器托管的地方，以linode 为例子
选一个cdn服务商，这里选了incapsula为例,

![此处输入图片的描述][1]
选一个dns服务商，这里选了dnspod为例
![此处输入图片的描述][2]

待设置生效后，ping 你的域名，你会发现，不会直接ping 得到你的机器托管的地点(linode的直接ip)，而是incapsula 提供的cdn 的ip。而你登陆到不同地区，有可能ping到另外一个ip －－－ cdn网络中的另外的ip。－－－当然，此例中incapsula免费版 只提供了2个ip。

ping 效果

    dengwei@dengweis-MacBook-Pro:~$ ping www.deng.io
    PING hrvni.x.incapdns.net (149.126.77.43): 56 data bytes
    64 bytes from 149.126.77.43: icmp_seq=0 ttl=53 time=208.264 ms

deploy 好后，你的网站加多了一个小logo。
![此处输入图片的描述][3]


  [1]: http://7xk67t.com1.z0.glb.clouddn.com/incapsula%20setting.png
  [2]: http://7xk67t.com1.z0.glb.clouddn.com/dnspod%20setting.png
  [3]: http://7xk67t.com1.z0.glb.clouddn.com/incapsula.png
