SDN中Switch和controller

在SDN中很重要的两个实体是Switch跟Controller。Controller在网络中相当于上帝，可以知道网络中所有的消息，可以给交换机下发指令。Switch就是一个实现Controller指令的实体，只不过这个交换机跟传统的交换机不一样，他的转发规则由流表指定，而流表由控制器发送。

## switch组成与传统交换机的差异

switch组成

switch由一个Secure Channel和一个flow table组成，of1.3之后table变成多级流表，有256级。而of1.0中table只在table0中。

Secure Channel是与控制器通信的模块，switch和controller之间的连接时通过socket连接实现。
Flow table里面存放这数据的转发规则，是switch的交换转发模块。数据进入switch之后，在table中寻找对应的flow进行匹配，并执行相应的action，若无匹配的flow则产生packet_in（后面有讲）
of中sw与传统交换机的差异

匹配层次高达4层，可以匹配到端口，而传统交换机只是2层的设备。
运行of协议，实现许多路由器的功能，比如组播。


#### controller组成

控制器有许多种，不同的语言，如python写的pox,ryu，如java写的floodlight等等。从功能层面controller分为以下几个模块：

底层通信模块：OpenFlow中目前controller与switch之间使用的是socket连接，所以控制器底层的通信是socket。
OpenFlow协议。socket收到的数据的处理规则需按照OpenFlow协议去处理。
上层应用：根据OpenFlow协议处理后的数据，开发上层应用，比如pox中就l2_learning,l3_learning等应用。更多的应用需要用户自己去开发。


---
e.g
OFPT_HELLO

创建socket之后，sw跟controller会彼此发送hello数据包。

目的：协议协商。
内容：本方支持的最高版本的协议
成果：使用双方都支持的最低版本协议。
成功：建立连接
失败：OFPT_ERROR (TYPE:OFPT_HELLO_FAILED,CODE =0),终止连接。


ofp_action_type = { 0: "OFPAT_OUTPUT",
                    1: "OFPAT_SET_VLAN_VID",
                    2: "OFPAT_SET_VLAN_PCP",
                    3: "OFPAT_STRIP_VLAN",
                    4: "OFPAT_SET_DL_SRC",
                    5: "OFPAT_SET_DL_DST",
                    6: "OFPAT_SET_NW_SRC",
                    7: "OFPAT_SET_NW_DST",
                    8: "OFPAT_SET_NW_TOS",
                    9: "OFPAT_SET_TP_SRC",
                    10: "OFPAT_SET_TP_DST",
                    11: "OFPAT_ENQUEUE"
                    }


---
OF 1.3

OpenFlow table

OpenFlow table分为255级，最低级为0.

一条flow entry的结构为：

Match Fields	Priority	Counters	Instructions	Timeouts	Cookie
此处有一个区别于1.0版本的instruction, 用于修改动作及或者处理流程，作用于各级流表之间。匹配过程可查看协议。



Multicle Controller

在1.3版本的OF中支持多控制器。但是这个多控制器只是相对于交换机而已。因为OF协议只是定义了交换机和控制器之间的通信过程。具体的控制器协同工作内容不是OF的范围。也许我可以开发一个这样的协议哦！

在多控制器内容中，相对于交换机而言，控制器可以有3中身份：equal,master和slave。控制器可以在任何时候改变角色。

Equal

Equal表明这个控制器并没有什么特殊之处，他和其他同样为equal的控制器是同等级的。比如一个交换机连接了3个控制器，且这三个控制器都是equal属性，那么这三个控制器对于交换机而言是等价的。equal类型控制器相当于一个独立的，具有完全权限的控制器。

Master and Slave

Master

Master角色具有和Equal一样的完全权限。

当多控制器中某一个控制器申请成为Master时，其他控制器将成为Slave角色。

Slave

作为Slave角色的控制器对交换机仅有可读权限,不能接受异步消息

（除去port_status以外的其他异步等消息）

不能向交换机发送写消息（ofp_flow_mod等），若交换机收到slave控制器发送的写消息，将产生ERROR。


---
RYU

介绍ryu/ryu目录下的主要目录内容。

base

base中有一个非常重要的文件：app_manager.py.其作用是RYU 应用的管理中心。用于加载RYU应用程序，接受从APP发送过来的信息，同时也完成消息的路由。

其主要的函数有app注册、注销、查找、并定义了RYUAPP基类，定义了RYUAPP的基本属性。包含name,threads,events、event_handlers和observers等成员，以及对应的许多基本函数。如：start(),stop()等。

这个文件中还定义了AppManager基类,用于管理APP。定义了加载APP等函数。不过如果仅仅是开发APP的话，这个类可以不必关心。

controller

controller文件夹中许多非常重要的文件，如events.py,ofp_handler.py,controller.py等。其中controller.py中定义了OpenFlowController基类。用于定义OpenFlow的控制器，用于处理交换机和控制器的连接等事件，同时还可以产生事件和路由事件。其事件系统的定义，可以查看events.py和ofp_events.py。

在ofp_handler.py中定义了基本的handler(应该怎么称呼呢？句柄？处理函数？)，完成了基本的如：握手，错误信息处理和keep alive 等功能。更多的如packet_in_handler应该在app中定义。

在dpset.py文件中，定义了交换机端的一些消息，如端口状态信息等，用于描述和操作交换机。如添加端口，删除端口等操作。

其他的文件不再赘述。

lib

lib中定义了我们需要使用到的基本的数据结构，如dpid,mac和ip等数据结构。在lib/packet目录下，还定义了许多网络协议，如ICMP,DHCP，MPLS和IGMP等协议内容。而每一个数据包的类中都有parser和serialize两个函数。用于解析和序列化数据包。

lib目录下，还有ovs,netconf目录，对应的目录下有一些定义好的数据类型，不再赘述。

ofproto

在这个目录下，基本分为两类文件，一类是协议的数据结构定义，另一类是协议解析，也即数据包处理函数文件。如ofproto_v1_0.py是1.0版本的OpenFlow协议数据结构的定义，而ofproto_v1_0_parser.py则定义了1.0版本的协议编码和解码。具体内容不赘述，实现功能与协议相同。

topology

包含了switches.py等文件，基本定义了一套交换机的数据结构。event.py定义了交换上的事件。dumper.py定义了获取网络拓扑的内容。最后api.py向上提供了一套调用topology目录中定义函数的接口。

contrib

这个文件夹主要存放的是开源社区贡献者的代码。我没看过。

cmd

定义了RYU的命令系统，具体不赘述。

services

完成了BGP和vrrp的实现。具体我还没有使用这个模块。

tests

tests目录下存放了单元测试以及整合测试的代码，有兴趣的读者可以自行研究。
