
## 网络拓扑设计


逻辑图如下：

![](file:////Users/lijiansheng/developing/work_for_umcloud/project/ump/网络拓扑图.png)

在UMCloud OpenStack中有以下网络类型：
*  带外IPMI：用于裸机管理。* Admin/PXE：用于节点发现与操作系统安装。* Public：用于外部网络流量和对外访问。* Management：用于OpenStack API流量、消息队列流量、OpenStack数据库流量、虚拟机和分布式存储之间的流量。* Storage：用于分布式存储副本复制流量。* Private：用于虚拟机之间(私有网络)的流量。


## VLAN 及IP 段划分

| 网络类型 | 网络IP段 | VLAN |
| ------------ | ------------- | ------------ |
| iDrac(带外IPMI) |任意（如192.168.1.0/24）   |  |
| Admin/PXE | 10.1.0.0/24  | 100，untagged |
| Public及Floating IP | 197.1.0.0/24  |  200|
|management  | 172.16.10.0/24  |  201|
| storage |  192.168.202.0/24 | 202 |
| private |  192.168.203.0/24 | 203 |



## 本次测试的硬件（X86服务器）配置参数表

| 设备型号| 序列号 | 配置描述 | 角色 |
| ------------ | ------------- | ------------ | ------------ |
|Dell R720 |3SZHF3X   |1*E5-2603 4C 1.8GHz/32GB/2*300GB | Fuel |
|Dell R720 |B7GFS02   |2*E5-2650 8C 2.6GHz/32GB/4*300GB | 控制节点 |
|Dell R720 |C7GFS02   |2*E5-2650 8C 2.6GHz/32GB/4*300GB |计算节点（Ironic）  |
|Dell R620 |JWRG5X1   |2*E5-2660 8C 2.2GHz/16GB/2*300GB | 控制节点 |
|Dell R620 |6WRG5X1   |2*E5-2660 8C 2.2GHz/16GB/2*300GB | 控制节点 |
|Dell R730 |2PL8DB2   |2*E5-2630v3 8C 2.4GHz/128GB/8*300GB |计算／存储节点  |
|Dell R730 |3NWGDB2   |2*E5-2630v3 8C 2.4GHz/128GB/8*300GB |计算／存储节点  |
|IBM 3650M4 |06NGKD4   |2*E5-2660 8C 2.2GHz/128GB/8*300GB | 计算／存储节点 |
|浪潮5280M4 |   |2*E5-2630v3 8C 2.4GHz/128GB/16*300GB | 待购 |
|浪潮5280M4 |   |2*E5-2630v3 8C 2.4GHz/128GB/16*300GB | 待购 |

交换机 2台。

## 本次涉及到的Mirantis 软件部分


