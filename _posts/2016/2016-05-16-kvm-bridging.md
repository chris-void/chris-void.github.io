---
layout: post
title: Linux bridging and KVM
categories:
- linux
- network
tags:
- note
---

## Linux Networking

**RFC 1918**

* Private Address Space

The Internet Assigned Numbers Authority (IANA) has reserved the
following three blocks of the IP address space for private internets:

    10.0.0.0 - 10.255.255.255 (10/8 prefix)
    172.16.0.0 - 172.31.255.255 (172.16/12 prefix)
    192.168.0.0 - 192.168.255.255 (192.168/16 prefix)

*Note:* 192.168.x.x is for smaller networks, 10.x.x.x is for larger networks.


## Linux Bridging

**Bridging**

    +-------+
    | eth0  |
    +---+---+
        |
   +----+----+
   |   br0   |
   +----+----+
        |
    +---+----+
    |  eth1  |
    +--------+

**VLAN**

   +---------+
   | net dev |  vlan parent net dev
   +----+----+
        |
    +---+---+
    | vlan  |    vlan100
    +-------+

**TAP**

   +------+
   | tap0 |     tap dev
   +--+---+
      |                     kernel space
------+---------------------------------------------
      |                      user space
 +----+-----+
 | char dev |     linux tap char dev
 +----------+







## Linux bridging

* OpenVPN bridge. Can't access machines on local network

ANS: Bridging is using MAC address table that it learned, a virtual machine serves as the openvpn server may results in "no learn new mac address" issue. Thus, the arp request will show on the ovpn server but not reply on eth0 network.

### OpenVPN stuff

                      +-----------------------+
                      |     OpenVPN Host      |
                      |                       |
                      |    +-------------+    |
                      |    |     br0     |    |
                      |    +-+---------+-+    |
                      |      |         |      |
+---------------+     |  +---+--+   +--+---+  |
| GpENI  switch +-----+--+ eth0 |   | tap0 |  |
+---------------+     |  +------+   +------+  |
                      |                       |
+-----------------+   |  +------+             |
| public internet +---+--+ eth1 |             |
+-----------------+   |  +------+             |
                      |                       |
                      +-----------------------+



[IBM guide on linux networking](https://www.ibm.com/developerworks/cn/linux/1310_xiawc_networkdevice/)   

## Linux Kernel Virtual Machine


[Understanding Linux Bridging](http://networkengineering.stackexchange.com/questions/29014/understanding-linux-bridging)


[2](http://tjlxy.lofter.com/post/335f69_10a48df)   
[3](http://suntus.github.io/2014/10/02/%E5%9C%A8ubuntu%E4%B8%AD%E6%90%AD%E5%BB%BA%E7%BD%91%E6%A1%A5%E2%80%94%E2%80%94%E8%99%9A%E6%8B%9F%E6%9C%BA%E7%9B%B8%E5%85%B3/)   
[4](http://kuring.me/post/linux_bridge)   
[5](http://blog.csdn.net/zhaihaifei/article/details/38581247)   
[6](http://vinllen.com/linux-bridgeshe-ji-yu-shi-xian/)   
[7](http://rock3.info/blog/2013/11/05/bridge-in-linux-kernel-stp/)   
[8](https://www.zybuluo.com/zwei/note/350311)   
[9](http://www.aboutyun.com/blog-61-1014.html)   

## reference
[Getting To Know the KVM](http://prefetch.net/blog/index.php/2009/06/19/getting-to-know-the-linux-kernel-virtualization-machine-kvm-presentation-slides/)   
[Kernel Korner - Linux as an Ethernet Bridge](http://www.linuxjournal.com/article/8172)   
