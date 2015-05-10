---                                                                               
layout: post
title: 网络虚拟化核心技术
categories:
- Virtualization
tags:
- kvm
- openstack
---

## 虚拟化&OpenStack

** 简要描述虚拟化的原理，虚拟化的几种类型**

+ 全虚拟化 -> 完全支持所有指令
+ 类虚拟化 -> 支持部分指令，部分需要修改系统内核

全虚拟化实现：硬件辅助，软件辅助（二进制指令动态翻译）
实现模式：hypervisor，宿主，混合

**了解虚拟化平台&特点**

+ vsphere
+ XEN/xenserver
+ KVM
+ hyper-v
+ docker/lxc

**在虚拟化平台进行管理&如何集中资源**

+ OpenStack
+ CloudStack

**OpenStack的组成和结构&设计原理**

+ 核心    
计算 存储 网络
+ 以服务组件模式，每个服务组件实例通过消息队列通信

**组件的的核心流程和结构**

计算、存储、网络

## Python & 操作系统、shell编程

+ Python常用库
+ 操作系统概念、KVM与Linux
i.   KVM & QEMU
ii.  KVM & Cgroup
iii. OpenStack、libvirt & KVM
iv.  网络 & KVM, bridge,ovs,namespace
v.   网络与Vlan
vi.  iptables,ebtables,openstack如何应用iptables
vii. iptables调试

## OpenStack编程