title: network_monitor
date: 2015-12-30 21:19:21
tags:
---
# 在局域网监控其他机器

------
由于觉得内网机器有异常的流量流入服务器，于是要在局域网监控其他机器，装了wireshark 监控后，发现只能监控发给自己的、自己发出的，多播、广播的，监控不了其他的机器。即使开了promiscuous模式。另外有些网卡/OS不支持混杂模式。于是去看FAQ：
 
[wireshark relative ref][1]:
6. Capturing packets
Q 6.1: When I use Wireshark to capture packets, why do I see only packets to and from my machine, or not see all the traffic I'm expecting to see from or to the machine I'm trying to monitor?

A: This might be because the interface on which you're capturing is plugged into an Ethernet or Token Ring switch; on a switched network, unicast traffic between two ports will not necessarily appear on other ports - only broadcast and multicast traffic will be sent to all ports. 
Note that even if your machine is plugged into a hub, the "hub" may be a switched hub, in which case you're still on a switched network. 
Note also that on the Linksys Web site, they say that their auto-sensing hubs "broadcast the 10Mb packets to the port that operate at 10Mb only and broadcast the 100Mb packets to the ports that operate at 100Mb only", which would indicate that if you sniff on a 10Mb port, you will not see traffic coming sent to a 100Mb port, and vice versa. This problem has also been reported for Netgear dual-speed hubs, and may exist for other "auto-sensing" or "dual-speed" hubs. 
Some switches have the ability to replicate all traffic on all ports to a single port so that you can plug your analyzer into that single port to sniff all traffic. You would have to check the documentation for the switch to see if this is possible and, if so, to see how to do this. See the switch reference page on the Wireshark Wiki for information on some switches. (Note that it's a Wiki, so you can update or fix that information, or add additional information on those switches or information on new switches, yourself.) 
Note also that many firewall/NAT boxes have a switch built into them; this includes many of the "cable/DSL router" boxes. If you have a box of that sort, that has a switch with some number of Ethernet ports into which you plug machines on your network, and another Ethernet port used to connect to a cable or DSL modem, you can, at least, sniff traffic between the machines on your network and the Internet by plugging the Ethernet port on the router going to the modem, the Ethernet port on the modem, and the machine on which you're running Wireshark into a hub (make sure it's not a switching hub, and that, if it's a dual-speed hub, all three of those ports are running at the same speed. 
If your machine is not plugged into a switched network or a dual-speed hub, or it is plugged into a switched network but the port is set up to have all traffic replicated to it, the problem might be that the network interface on which you're capturing doesn't support "promiscuous" mode, or because your OS can't put the interface into promiscuous mode. Normally, network interfaces supply to the host only:

packets sent to one of that host's link-layer addresses;
broadcast packets;
multicast packets sent to a multicast address that the host has configured the interface to accept.
Most network interfaces can also be put in "promiscuous" mode, in which they supply to the host all network packets they see. Wireshark will try to put the interface on which it's capturing into promiscuous mode unless the "Capture packets in promiscuous mode" option is turned off in the "Capture Options" dialog box, and TShark will try to put the interface on which it's capturing into promiscuous mode unless the -p option was specified. However, some network interfaces don't support promiscuous mode, and some OSes might not allow interfaces to be put into promiscuous mode. 
If the interface is not running in promiscuous mode, it won't see any traffic that isn't intended to be seen by your machine. It will see broadcast packets, and multicast packets sent to a multicast MAC address the interface is set up to receive. 
You should ask the vendor of your network interface whether it supports promiscuous mode. If it does, you should ask whoever supplied the driver for the interface (the vendor, or the supplier of the OS you're running on your machine) whether it supports promiscuous mode with that network interface. 
In the case of token ring interfaces, the drivers for some of them, on Windows, may require you to enable promiscuous mode in order to capture in promiscuous mode. See the Wireshark Wiki item on Token Ring capturing for details. 
In the case of wireless LAN interfaces, it appears that, when those interfaces are promiscuously sniffing, they're running in a significantly different mode from the mode that they run in when they're just acting as network interfaces (to the extent that it would be a significant effort for those drivers to support for promiscuously sniffing and acting as regular network interfaces at the same time), so it may be that Windows drivers for those interfaces don't support promiscuous mode.


简单解释：
局域网不可以随便可以监听别人的信息。在router & switch 中，单播与多播的包都是去往特定的机器的，不是你的信息，是不会发到你的机器中。
hub 中才可以监听到。
所以要实现监听，要在上一级网络。

ok。根据我们当前的网络情况，我们有一个5 口（4Wan 1Lan）路由：TL-ER6120,一个24口交换机NetGear GS724T。
交换机接在路由器Lan口。路由器的4Wan口都是用于接拨号的外网网络。Lan口接内网。
看了manual, 交换机有monitor 功能，但不是用来监控网络的。路由器有监控网络流量的功能，但功能太简单，只能区分来源IP，包类别（用于监控客户用什么软件，Audit 功能）。

咨询相关同事后说两种方案：

 - 要做一台中间机，在路由器与交换机之间做转发，同时做监控。 
 - 特点的路由器/交换机可以做复制包功能，将所有包转发到监控端口。

于是结合细看Manual，发现我们使用的路由刚好有个port mirror 功能。
于是设置路由将原来的4wan口1Lan模式转化为3wan口模式2Lan模式，将5th Lan口 mirror 到4th Lan 口做监控。

然后4thLan  接出来一台机器，ok check 1下，可以上网。再开wireshark，得到有5th Lan 口转发过来的所有流量请求了。
后续再接着按需做监控。



 
  [1]: https://www.wireshark.org/faq.html#q6.1
