# 网络拓扑图

![](file:///Users/lijiansheng/Documents/UMCloud/%E9%A1%B9%E7%9B%AE/%E6%B0%91%E7%94%9F%E9%93%B6%E8%A1%8C/%E7%BD%91%E7%BB%9C%E6%8B%93%E6%89%91%E5%9B%BE.png)

在UMCloud OpenStack中有以下网络类型：
*  带外IPMI：用于裸机管理。* Admin/PXE：用于节点发现与操作系统安装。* Public：用于外部网络流量和对外访问。* Management：用于OpenStack API流量、消息队列流量、OpenStack数据库流量、虚拟机和分布式存储之间的流量。* Storage：用于分布式存储副本复制流量。* Private：用于虚拟机之间(私有网络)的流量。

# 硬件接口／vlan对应列表

| 节点 | iDrac  | 网卡  | vlanID | 功能 |
| ------------ | ------------- | ------------ |  ------------ | ------------ |
|Fuel(R730)  | 199.0.50.54  |  网卡1|  1100|  Mirantis OS 部署服务器。| 
| 控制节点（网络） （ R730 ）1|199.0.50.55   |网卡1  | 1100  | Admin/PXE |
|  |   |  网卡2| 1124| Public  |
|  |   | 网卡3 |247 |  Management |
|  |   | 网卡4 |255 | Storage  |
|  |   | 网卡5 | 252| Private  |
| 控制节点（网络） （ R730 ）2| 199.0.50.81  |网卡1|  1100|  Mirantis OS 部署服务器。| 
|  |   |  网卡2| 1124| Public  |
|  |   | 网卡3 |247 |  Management |
|  |   | 网卡4 |255 | Storage  |
|  |   | 网卡5 | 252| Private  |
| 控制节点（网络） （ R730 ）3| 199.0.50.82  |  网卡1|  1100|  Mirantis OS 部署服务器。| 
|  |   |  网卡2| 1124| Public  |
|  |   | 网卡3 |247 |  Management |
|  |   | 网卡4 |255 | Storage  |
|  |   | 网卡5 | 252| Private  |
| LMA（监控）（R730） |  199.0.50.90 | 网卡1|  1100|  Mirantis OS 部署服务器。| 
|  |   |  网卡2| 1124| Public  |
|  |   | 网卡3 |247 |  Management |
|  |   | 网卡4 |255 | Storage  |
|  |   | 网卡5 | 252| Private  |
|计算节点＋存储节点 1  |  199.0.50.105 |  网卡1|  1100|  Mirantis OS 部署服务器。| 
|  |   |  网卡2| 1124| Public  |
|  |   | 网卡3 |247 |  Management |
|  |   | 网卡4 |255 | Storage  |
|  |   | 网卡5 | 252| Private  |
|计算节点＋存储节点 2  |  199.0.50.51 |  网卡1|  1100|  Mirantis OS 部署服务器。| 
|  |   |  网卡2| 1124| Public  |
|  |   | 网卡3 |247 |  Management |
|  |   | 网卡4 |255 | Storage  |
|  |   | 网卡5 | 252| Private  |
|计算节点＋存储节点 3  |  199.0.50.52 |  网卡1|  1100|  Mirantis OS 部署服务器。| 
|  |   |  网卡2| 1124| Public  |
|  |   | 网卡3 |247 |  Management |
|  |   | 网卡4 |255 | Storage  |
|  |   | 网卡5 | 252| Private  |
|计算节点＋存储节点 4  |  199.0.50.60 |  网卡1|  1100|  Mirantis OS 部署服务器。| 
|  |   |  网卡2| 1124| Public  |
|  |   | 网卡3 |247 |  Management |
|  |   | 网卡4 |255 | Storage  |
|  |   | 网卡5 | 252| Private  |
|计算节点＋存储节点 5  | 199.0.50.79  |  网卡1|  1100|  Mirantis OS 部署服务器。| 
|  |   |  网卡2| 1124| Public  |
|  |   | 网卡3 |247 |  Management |
|  |   | 网卡4 |255 | Storage  |
|  |   | 网卡5 | 252| Private  |
|计算节点＋存储节点 6  |  199.0.50.86 |  网卡1|  1100|  Mirantis OS 部署服务器。| 
|  |   |  网卡2| 1124| Public  |
|  |   | 网卡3 |247 |  Management |
|  |   | 网卡4 |255 | Storage  |
|  |   | 网卡5 | 252| Private  |
|计算节点＋存储节点 7  | 199.0.50.90  |  网卡1|  1100|  Mirantis OS 部署服务器。| 
|  |   |  网卡2| 1124| Public  |
|  |   | 网卡3 |247 |  Management |
|  |   | 网卡4 |255 | Storage  |
|  |   | 网卡5 | 252| Private  |

注意：**若有连接的网卡而未在上述表中体现的，则在实际操作中会以 Bonding 的形式来进行测试。**

# 网络vlan及ip规划建议
| 网络类型 | 网络IP段 | VLAN |
| ------------ | ------------- | ------------ |
| iDrac(带外IPMI) |199.0.50.｛51，52，54，55，60，79，81，82，86，87，90，105｝   |  |
| Admin/PXE | 10.53.0.0/24  | 1100，untagged |
| Public及Floating IP | 10.53.24.0/24  |  1124|
|management  | 197.1.47.0/24  |  247|
| storage |  197.1.55.0/24 | 255 |
| private |  197.1.52.0/24 | 252 |

注：**IP段和VLAN均借鉴了客户给的例子。