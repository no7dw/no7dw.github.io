title: network_monitor
date: 2015-12-30 21:19:21
tags:
---
# 网络监控

------
在局域网监控其他机器：
 
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


  [1]: https://www.wireshark.org/faq.html#q6.1
