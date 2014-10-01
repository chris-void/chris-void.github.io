---
layout: post
title: Deep in OpenStack-Cinder
categories:
- Vinzor
tags:
- openStack
- cinder
---

[Notes for Cinder](https://github.com/chris-void/OpenStack-Understand-Guide/tree/master/cinder-understand)    
    ^    
完整版请戳    


Outline
---
1.关于Cinder的介绍   
2.关于snapshot,image   
3.Cinder代码研究(backup, scheduler, volume)    

## 关于Cinder的介绍

Cinder的前身是nova-volume, Openstack中的实例是不能持久化的,实现持久化的方法就是使用cinder/bin/cinder-volume. 挂载volume之后在volume中实现持久化.

Cinder的服务主要有cinder-volume，cinder-backup，cinder-scheduler，还有一个api服务，并且Cinder提供了一个控制台管理工具，提供命令行操作。cinder-backup提供cinder中的volume的备份管理功能，现在的实现是用swift作为存储后台，按照以后的发展也许用Ceph代替能提高性能, cinder-volume就是实现实际的块存储管理功能，cinder-scheduler实现调度功能.我认为理解cinder的难点在于要对于分布式存储，ISCSI协议有一定知识.

Cinder对块数据实现了多种的存储管理方式。主要有LVM，nfs，ISCSI. 这些存储方式都在cinder/volume/drivers下,要实现特定的存储方法只需要继承VolumeDriver基类或者类似iSCSIDriver子类.

在nova的源代码libvirt目录下有一个volume.py实现对应cinder的，实现对于实际运行volume相关操作。针对cinder的不同存储类型，对应的有不同的Volume Driver类型，如LibvirtVolumeDriver，LibvirtNetVolumeDriver，LibvirtISCSIVolumeDriver，LibvirtNFSVolumeDriver等。这些类都继承于LibvirtBaseVolumeDriver这个基类，主要实现的功能其实就是构造libvirt 中attachDeviceFlags 函数需要的xml格式参数（挂载卷时attach_volume），或者构造实例xml时添加device，和实现一些功能的命令执行如LibvirtISCSIVolumeDriver中一些iscsi命令。

## Snapshot & Image



## Cinder代码研究

Api.py：通常处理与本组件有关的请求。

/driver：通常是继承本部分的driver.py的基类开发出来的适应不同情况的具体使用方法。

+ backup    
卷备份（create、restore、delete） driver（ceph swift tcm）

+ brick    
块设备（还在研究中）

+ compute    
利用novaClient实现卷的快照的建立 删除

+ db    
存储键值&当前运行的服务的db的内容

+ image    
应用glance作为后端镜像服务

+ keymgr    
密钥管理（对称加密）

+ schduler    
调度器（filter weigh）

+ transfer    
卷在用户之间的转换

+ volume    
50%代码在这里，主要是卷存储的相关知识

**个人研究感觉比较复杂一点的就是backup，schduler，volume三个部分比较复杂一些，其中brick是为H版所专门包含的，以后会脱离Cinder挪到oslo。Schduler模块其实就是衡量如何选择主机的，主要功能就是filter过滤掉不适合做主机的机子，weigh计算权重得到最适合做主机的机子。Volume应该是Cinder的核心，所有和卷存储有关的内容都包含在这里**




/cinder/volume/api.py：处理卷相关的所有请求；

    class API(base.Base):卷的管理操作接口API；
        def list_availability_zones(self):描述已知可用的zone；
        def create/delete/update实现卷的建立/ 删除/ 设置给定的属性&&更新；
        def get(self, context, volume_id):根据volume_id获取相应的volume；
        def get_all(self, context, marker=None, limit=None,  sort_key='created_at', sort_dir='desc', filters={}):获取所有卷的信息；
        def get_snapshot(self, context, snapshot_id):获取指定卷的快照；
        def get_volume(self, context, volume_id):根据volume_id获取volume；
        def get_all_snapshots(self, context, search_opts=None):获取属于指定上下文环境中用户的所有的卷的快照；
        def reserve_volume(self, context, volume):卷信息的预留保存；
        def attach/detach: 卷的挂载/卸载；
        def initialize_connection(self, context, volume, connector):初始化卷的连接操作；
        def terminate_connection(self, context, volume, connector, force=False):通过连接器从主机清理连接；
        def accept_transfer(self, context, volume, new_user, new_project):实现存储器上卷的所有权的转换；指定了要转换所有权的卷volume、新的用户new_user和新的对象new_project；

        def create_snapshot(self, context, volume, name, description,metadata=None):调用（_create_snapshot）实现建立并导出快照；
        def create_snapshot_force(self, context, volume, name, description,metadata=None):实现建立并导出快照；
        def delete_snapshot(self, context, snapshot, force=False):实现删除快照；
        def update_snapshot(self, context, snapshot, fields):为一个快照设置给定属性并进行更新；

        def get_volume_metadata(self, context, volume):获取卷相关联的所有元数据；
        def delete_volume_metadata(self, context, volume, key):删除卷的元数据序列；
        def _check_metadata_properties(self, context, metadata=None):检测卷元数据的属性；
        def update_volume_metadata(self, context, volume, metadata,delete=False):更新或建立卷的元数据；
        def get_volume_metadata_value(self, volume, key):获取卷的元数据中key对应的值；

        def get_volume_admin_metadata(self, context, volume):管理员用户获取指定volume_id的卷的元数据信息；
        def delete_volume_admin_metadata(self, context, volume, key):删除给定卷的元数据项；
        def update_volume_admin_metadata(self, context, volume, metadata,delete=False):更新或建立卷的管理元数据；

        def get_snapshot_metadata(self, context, snapshot):获取一个快照的所有相关联的元数据；
        def delete_snapshot_metadata(self, context, snapshot, key):在数据库中删除给定快照的元数据序列；
        def update_snapshot_metadata(self, context, snapshot, metadata,delete=False):如果数据库中指定快照的元数据存在，则进行更新，如果快照的元数据不存在，则建立相应的条目信息；

        def get_volume_image_metadata(self, context, volume):获取指定卷的glance中的元数据；
        def_check_volume_availability(self, context, volume, force):检测卷是可用的；
        def copy_volume_to_image(self, context, volume, metadata, force):为指定的卷建立一个新的镜像，并实现上传指定的卷到Glance；

        def extend(self, context, volume, new_size):调用存储后端的extend_volume方法，实现卷大小的扩展操作；
        def migrate_volume(self, context, volume, host, force_host_copy):调用存储后端的migrate_volume_to_host方法，实现迁移卷到指定的主机；
        def migrate_volume_completion(self, context, volume, new_volume, error):调用存储后端的migrate_volume_completion方法，实现卷的迁移的完成操作；


/cinder/volume/driver.py：卷的驱动类；

	class VolumeDriver(object):执行和存储卷相关的命令；
        def get_version(self):获取驱动器当前的版本信息；
        def create_volume(self, volume):卷的建立；
        def create_volume_from_snapshot(self, volume, snapshot):根据快照建立卷；
        defcreate_cloned_volume(self, volume, src_vref):创建指定卷的克隆；
        def delete_volume(self, volume):卷的删除；

        def create_snapshot(self, snapshot):快照的建立；
        def delete_snapshot(self, snapshot):快照的删除；

           def create_export(self, context, volume):创建target，并将指定的逻辑卷加入LUN；
        def remove_export(self, context, volume):删除卷的导出；

        def initialize_connection(self, volume, connector):允许连接到连接器并返回连接信息；
        def terminate_connection(self, volume, connector, **kwargs):断开从连接器的连接；

        def attach_volume(self, context, volume, instance_uuid, host_name,mountpoint):附加到实例或主机的卷的回调方法；
        def detach_volume(self, context, volume):卷的卸载的回调方法；
        def get_volume_stats(self, refresh=False):返回卷的服务的当前状态；

        def do_setup(self, context):初始化volume driver的操作；
        def validate_connector(self, connector):如果连接器不包含驱动所需的所有数据，则结果为fail；
        def _copy_volume_data_cleanup(self, context, volume, properties,attach_info, remote, force=False):实现从主机断开卷的连接，并清理连接的数据信息；
        def copy_volume_data(self, context, src_vol, dest_vol, remote=None):从src_vol到dest_vol复制数据；
        def copy_image_to_volume(self, context, volume, image_service,image_id):从image_service获取镜像数据并写入卷；
        def copy_volume_to_image(self, context, volume, image_service,image_meta):拷贝卷到指定的镜像；
        def _attach_volume(self, context, volume, properties, remote=False):实现附加一个卷；
        def _detach_volume(self, attach_info):从主机断开卷的连接；
        def clone_image(self, volume, image_location, image_id):从现有的镜像有效的建立一个卷；
        def backup_volume(self, context, backup, backup_service):为一个已存在的卷创建新的备份；
        def restore_backup(self, context, backup, volume, backup_service):恢复一个备份到一个新的或者是存在的卷；
        def clear_download(self, context, volume):中断一个镜像的拷贝之后清理缓存信息；
        def migrate_volume(self, context, volume, host):迁移卷到指定的主机；
    class ISCSIDriver(VolumeDriver):执行与ISCSI卷相关的命令；执行支持ISCSI协议的存储卷相关的命令；
    class ISERDriver(ISCSIDriver):执行与ISER卷相关的命令；
    class FibreChannelDriver(VolumeDriver):执行Fibre Channel volumes的相关命令；

/cinder/volume/manager.py：卷的管理（建立、挂载、卸载以及持久性存储）；
    
	class VolumeManager(manager.SchedulerDependentManager):管理可连接的块存储设备；
        def create_volume(self, context, volume_id, request_spec=None,filter_properties=None, allow_reschedule=True, snapshot_id=None, image_id=None,source_volid=None):建立并导出卷；
        def delete_volume(self, context, volume_id):删除卷的操作；
        def create_snapshot(self, context, volume_id, snapshot_id):调用存储后端的create_snapshot方法，实现调用实现建立并导出快照；
        def delete_snapshot(self, context, snapshot_id):调用存储后端的delete_snapshot方法，实现删除快照；
        def attach_volume(self, context, volume_id, instance_uuid, host_name,mountpoint, mode):调用具体存储后端的attach_volume方法，实现卷的附加操作，并更新数据库表明卷已经附加；
        def detach_volume(self, context, volume_id):调用存储后端的detach_volume方法，实现卷的卸载操作，并更新数据库来表明卷已经卸载；
        def copy_volume_to_image(self, context, volume_id, image_meta):调用存储后端的copy_volume_to_image方法，实现上传指定的卷到Glance；
        def initialize_connection(self, context, volume_id, connector):调用存储后端的initialize_connection方法，实现初始化卷的连接操作；
        def terminate_connection(self, context, volume_id, connector,force=False):调用存储后端的terminate_connection方法，实现通过连接器从主机清理连接；
        def accept_transfer(self, context, volume_id, new_user, new_project):调用存储后端的accept_transfer方法，实现存储器上卷的所有权的转换；指定了要转换所有权的卷volume、新的用户new_user和新的对象new_project；
        def _migrate_volume_generic(self, ctxt, volume, host):普通的卷的迁移方法；
        def migrate_volume_completion(self, ctxt, volume_id, new_volume_id,error=False):卷的迁移结束之后的操作；
        def migrate_volume(self, ctxt, volume_id, host, force_host_copy=False):迁移卷到指定的主机；
        def _report_driver_status(self, context):获取卷的状态信息；
        def publish_service_capabilities(self, context):收集驱动的状态和capabilities信息，并进行信息的发布；
        def _notify_about_volume_usage(self, context, volume, event_suffix,extra_usage_info=None):获取指定卷的使用率信息，并进行通知操作；
        def _notify_about_snapshot_usage(self, context, snapshot, event_suffix,extra_usage_info=None):从卷的快照获取卷的使用率信息，并进行通知操作；
        def extend_volume(self, context, volume_id, new_size):卷的扩展；调用存储后端的extend_volume方法，实现卷大小的扩展操作；



/cinder/volume/qos_specs.py：QoS功能的实现；

**QoS（Quality of Service）服务质量，是网络的一种安全机制；是用来解决网络延迟和阻塞等问题的一种技术；在正常情况下，如果网络只用于特定的无时间限制的应用系统，并不需要QoS；比如Web应用，或E-mail设置等；但是对关键应用和多媒体应用就十分必要；当网络过载或拥塞时，QoS能确保重要业务量不受延迟或丢弃，同时保证网络的高效运行；**
    
	def create(context, name,specs=None):在数据库中建立一个qos_specs；
    def update(context, qos_specs_id,specs):更新数据库中QOS功能的数据信息
											；
    def delete(context, qos_specs_id,force=False):标志QOS功能为删除标识；
    def delete_keys(context,qos_specs_id, keys):设置指定的目标QOS功能的标识为delete标识；
    def get_associations(context,specs_id):根据给定的qos_specs的id值获取所有相关的卷的类型信息；
    def associate_qos_with_type(context, specs_id, type_id):根据给定的卷的类型解除相关的qos_specs；
    def disassociate_qos_specs(context,specs_id, type_id):解除卷类型从指定的qos_specs；
    def disassociate_all(context,specs_id):从所有的实体中消除与specs_id相关联的qos_specs；
    def get_all_specs(context,inactive=False, search_opts={}):获取所有符合条件的qos_specs；
    def get_qos_specs(ctxt, id):根据给定的id值获取单个的QOS功能；
	def get_qos_specs_by_name(context,name):根据给定的名称获取单个QOS功能的相关信息；


/cinder/volume/utils.py：卷相关的实用工具和方法；

/cinder/volume/volume_types.py：内置卷的类型属性相关方法；
   
	def create(context, name,extra_specs={}):在数据库中建立一个新的卷的类型；
    def destroy(context, id):在数据库中删除卷的类型信息；
    def get_all_types(context,inactive=0, search_opts={}):获取所有数据库中没有删除的卷的类型；
    def get_volume_type(ctxt, id):根据给定id来检索获取单个的卷的类型；
    def get_volume_type_by_name(context, name):根据名称获取单个卷的类型；
    def get_default_volume_type():获取默认的卷的类型；
    def is_encrypted(context,volume_type_id):验证卷的类型是否是加密的；
    def get_volume_type_qos_specs(volume_type_id):根据给定的卷类型获取所有QOS功能相关的信息；

/cinder/volume/flows/base.py：flow的基类实现；
		def _make_task_name(cls,addons=None):获取任务类的名称；
    class CinderTask(task.Task):所有cinder任务的基类；
    class InjectTask(CinderTask):这个类实现了注入字典信息到flow中；

/cinder/volume/flows/utils.py：flow相关的一些实用工具；




/cinder/volume/drivers/emc----emc卷存储驱动；    
/cinder/volume/drivers/hds----hus卷存储驱动；    
/cinder/volume/drivers/huawei----huawei卷存储驱动；   
/cinder/volume/drivers/netapp----NetApp卷存储驱动；   
/cinder/volume/drivers/nexenta----Nexenta卷存储驱动；  
/cinder/volume/drivers/san----San卷存储驱动；   
/cinder/volume/drivers/vmware----VMware卷存储驱动；   
/cinder/volume/drivers/windows----Windows卷存储驱动；   
/cinder/volume/drivers/xenapi----XenApi卷存储驱动；   
/cinder/volume/drivers/coraid.py----Coraid卷存储驱动；   
/cinder/volume/drivers/eqlx.py----DELL卷存储驱动；   
/cinder/volume/drivers/gpfs.py----GPFS卷存储驱动；   
/cinder/volume/drivers/nfs.py----NFS卷存储驱动；    
/cinder/volume/drivers/rbd.py----RADOS块设备驱动；   
/cinder/volume/drivers/scality.py----ScalitySOFS卷驱动；    
/cinder/volume/drivers/solidfire.py----SolidFire卷驱动；    
/cinder/volume/drivers/storwize_svc.py----IBMStorwize和SVC卷驱动；   
/cinder/volume/drivers/xiv_ds8k.py----IBMXIV和DS8K存储系统卷驱动；   
/cinder/volume/drivers/zadara.py----VPSA卷驱动；   




### /bin/cinder-volume





## Reference：

---

[OpenStack源码分析之cinder-volume服务](http://blog.csdn.net/ddl007/article/details/8686445)

[OpenStack Cinder服务启动过程中的资源加载和扩展源码解析之一](http://blog.csdn.net/gaoxingnengjisuan/article/details/21722111)

[OpenStack Cinder源码分析之一](http://blog.csdn.net/gaoxingnengjisuan/article/details/17722991)

[OpenStack G版Cinder源码分析](http://www.linuxidc.com/Linux/2013-05/84492.htm)

[Openstack cinder命令学习笔记（一）](http://www.cnblogs.com/youjianxiaoxiang/archive/2012/12/18/2823511.html)

---

[CSDN——OpenStack源码分析](http://blog.csdn.net/column/details/liudong.html?page=1)


---

[ Openstack之Cinder服务初探](http://blog.csdn.net/luo_brian/article/details/8592692)

[Cinder](https://wiki.openstack.org/wiki/Cinder)

[Redhat 6.3 openstack的cinder安装配置](http://mars914.iteye.com/blog/1830941)

[Openstack cinder命令学习笔记（一）](http://www.cnblogs.com/youjianxiaoxiang/archive/2012/12/18/2823511.html)

[OpenStack安装部署管理中常见问题解决方法（OpenStack-Lite-FAQ）](http://www.open-open.com/lib/view/open1342256392776.html)

[OpenStack G版Cinder源码分析](http://www.linuxidc.com/Linux/2013-05/84492.htm)


[OpenStack Cinder源码分析之一](http://blog.csdn.net/gaoxingnengjisuan/article/details/17722991)

[OpenStack源码分析之cinder-api服务起步](http://www.reader8.cn/jiaocheng/20130316/1817391.html)


[OpenStack安装部署管理中常见问题解决方法（OpenStack-Lite-FAQ）](http://www.open-open.com/lib/view/open1342256392776.html)




[pycharm key](http://www.geek521.com/?p=1951)
