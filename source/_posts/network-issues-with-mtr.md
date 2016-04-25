title: network-issues-with-mtr
date: 2016-04-26 01:01:38
tags:
---
# 怀疑网络丢包


---

 排查第三方服务器到我们服务器的网络问题时，用了mtr 命令。 mtr发现中间有几行lost很高，误以为中间丢包很严重。
 实际mtr是traceroute 的改良版，并且是ping & traceroute 的混合体。
 但mtr 中途的lost 高，后续的lost 回复0% 时，仅仅代表转发的路由不回icmp 包，但正常处理了转发请求。
 上面说是ping & traceroute的混合体，如何利用mtr 看丢包呢？直接看最后到达目的地IP的lost percentage。如果是0%～1%算正常。图中最后一行。
 ![此处输入图片的描述][1]
 
ref:[diagnosing-network-issues-with-mtr][2]


  [1]: http://7xk67t.com1.z0.glb.clouddn.com/mtr.png
  [2]: https://www.linode.com/docs/networking/diagnostics/diagnosing-network-issues-with-mtr/
