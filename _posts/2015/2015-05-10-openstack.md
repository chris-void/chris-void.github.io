---
layout: post
title: OpenStack 
categories: 
- OpenStack 
tags:
- openstack
---


# 引言

关于OpenStack, 作为一种成熟开源的云计算技术已经日益受到关注,在这里主要是想根据自己对OpenStack和Ceph的理解和实践写一点总结和感悟,怕自己忘了.

OpenStack是由Rackspace与NASA于2010年7月共同推出的云计算开源项目，目的是提供大规模云操作系统，支持类似AWS功能的IaaS平台。目前已经成为仅次于Linux的最大的开源社区，其会员覆盖几乎所有主流的IT供应商。OpenStack广泛在互联网公司和传统企业间部署，并因经诞生了许多创业公司。OpenStack拥有非常好的架构，这体现在所有功能全部模块和API化，模块之间松耦合。( [项目详情](https://code.csdn.net/openkb/p-OpenStack) ）     

[项目主页](http://www.openstack.org/)    

[代码托管地址](https://github.com/openstack/openstack)   

### 关于OpenStack

自己开始研究和学习OpenStack也差不多有半年多了,开始的时候我还是三年级的小菜鸟,对于Linux的认识就仅限于用来写写课程作业,平时上上网.在实验室的锻炼(感谢师兄们和老板)自己对于Linux的认识和使用都有长足的进步,不敢说是什么高手,但是入门加上经常排错也可以说经验比较丰富了,遇到问题也可以思考加上实践尝试去解决了.OpenStack作为一种提供计算机虚拟化服务的云计算技术,最适合进行部署的环境当然是Linux,在网络部分Linux相较与windows server有巨大的优势. OpenStack宣称支持CentOS,Ubuntu和Fedora,不过我一般还是用Ubuntu的Desktop和server, 为什么呢?第一是ubuntu的安装比较省心,基本的软件包都有,平时自己的工作站去折腾还好,真要你对四五个环境一个一个折腾就算是写自动化脚本也受不了;第二是因为在对于OpenStack的支持上Ubuntu要强于其他的系统,估计是bug比较少吧.

好了,废话说完,第一个问题来了,OpenStack是怎么一回事呢?

就我个人的理解,OpenStack的每个组件都是独立的,组件之间通过nova-api,rabbitmq,mysql等等来进行合作,实现






##推荐相关文档：

[如何学习OpenStack，如何成为OpenStack工程师？](http://blog.csdn.net/z_lstone/article/details/14127227)   
[Openstack能走多远——Openstack、VMware浅析](http://blog.csdn.net/u012620688/article/details/13743517)   
[【OpenStack】Openstack之Cinder服务初探](http://blog.csdn.net/lynn_kong/article/details/8659145)    
[【OpenStack】在OpenStack上搭建OpenStack UT环境](http://blog.csdn.net/lynn_kong/article/details/9665027)   
[OpenStack学习笔记之--OpenStack Nova 架构](http://blog.csdn.net/xiangmin2587/article/details/7737778)    

##推荐下载资源：

[OpenStack快速进阶](http://download.csdn.net/detail/bilyyang/5810571)    
[OpenStack运维指南](http://download.csdn.net/detail/adela_09/5130471)     
[Openstack基础讲解](http://download.csdn.net/detail/necessary8/4474697)   
[openstack 安装以及配置教程超详细](http://download.csdn.net/detail/zhenxi537/4427341)    
[OpenStack云计算平台管理教程下载 OpenStack入门教程](http://download.csdn.net/detail/u010973404/6580117)    

### Devstack

```
git clone https://github.com/openstack-dev/devstack.git -b stable/icehouse
```


---
#资源汇总

[OpenStack安装部署管理中常见问题解决方法（OpenStack-Lite-FAQ）](http://blog.csdn.net/hilyoo/article/details/7746634)     
[OpenStack云平台的网络模式及其工作机制](http://blog.csdn.net/hilyoo/article/details/7721401)     
[OpenStack Hacker养成指南](https://www.ustack.com/blog/openstack_hacker/)      
[]()    
[]()    
[]()    
[]()    
[]()    
[]()    
[]()    
[]()    
[]()    
[]()    
[]()    


---

## Openstack存储技术

Openstack作为一个IaaS系统，涉及到存储的部分主要是块存储服务模块、对象存储服务模块、镜像管理模块和计算服务模块,分别对应为其中的Cinder、Swift、Glance和Nova四个项目

## Reference

部分资源来自CSDN知识库：[Link](http://code.csdn.net/news/2821726)    

[将Ceph存储集群集成到OpenStack云中](http://www.ibm.com/developerworks/cn/cloud/library/cl-openstackceph/)    
[Ceph与OpenStack整合文档](http://blog.csdn.net/epugv/article/details/16889135)   
[华为章宇：如何学习开源项目及Ceph的浅析](http://www.csdn.net/article/2014-04-10/2819247-how-to-learn-opensouce-project-&-ceph/2)
