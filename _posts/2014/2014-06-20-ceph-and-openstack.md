---
layout: post
title: Ceph and OpenStack
categories: 
- OpenStack 
tags:
- ceph
- openstack
---


# 引言

关于OpenStack, 作为一种成熟开源的云计算技术已经日益受到关注,在这里主要是想根据自己对OpenStack和Ceph的理解和实践写一点总结和感悟,怕自己忘了.

## Openstack存储技术

Openstack作为一个IaaS系统，涉及到存储的部分主要是块存储服务模块、对象存储服务模块、镜像管理模块和计算服务模块,分别对应为其中的Cinder、Swift、Glance和Nova四个项目

## Reference

[将Ceph存储集群集成到OpenStack云中](http://www.ibm.com/developerworks/cn/cloud/library/cl-openstackceph/)    
[Ceph与OpenStack整合文档](http://blog.csdn.net/epugv/article/details/16889135)   
[华为章宇：如何学习开源项目及Ceph的浅析](http://www.csdn.net/article/2014-04-10/2819247-how-to-learn-opensouce-project-&-ceph/2)
