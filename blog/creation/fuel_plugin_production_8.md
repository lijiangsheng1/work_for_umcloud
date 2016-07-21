# Fuel 插件盘点

摘要：

OpenStack 为了社区、为了开源，走的是情怀和理想路线。所以不会对任何的商业产品有所妥协。所以OpenStack和商业产品的整合在社区是蛮困难的一件事情。幸亏有Fuel，对于商业软件的支持作了改进，并以插件的形式提供给合作伙伴。本文尝试将目前8.0所支持并有实际案例的插件按照功能类别作一介绍。

_______

## 在归类之前需要解释的一点事情

在 [OpenStack 社区项目导航](http://www.openstack.org/software/project-navigator/)中，我们知道核心的子项目有：

* Nova 
* Neutron
* Swift
* Cinder
* Keystone
* Glance

而可选的子项目有：

* Horizon
* Ceilometer
* Heat
* Trove
* Sahara
* Ironic
* Zaqar
* Manila
* Designate
* Barbican
* Magnum
* Murano
* Congress

具体这些项目的代号，我就不作过多的解释了，还有很多没有进入OpenStack社区的项目。而Fuel或者说是 Mirantis 也有自己的理解。那么作为 Mirantis OpenStack 所支持的核心项目有：

* Nova
* Neutron
* Cinder
* Keystone
* Glance
* Horizon
* **Ceph**

可选的有：

* Heat
* Sahara
* Ironic
* Murano

以及这次我们要作如下介绍的插件。

## 计费

### Talligent Openbook Fuel 插件

此插件扩展了 Mirantis OpenStack 的功能，当然是通过 OpenBook 来实现的，为Mirantis OpenStack 带来了自助服务、收费／退款等功能。OpenBook 是一款云平台管理和报告的软件，它可以为云服务提供计划任务、掌控和预测需求。Talligent 为管理员和租户带来了实时的可视化的资源消耗情况，以及根据预算和需求来扩展和压缩的能力。

对于服务供应商来说：Talligent 的 OpenBook 是将 OpenStack 服务售出的最快的途径。对于企业来讲：没有部门的对于云服务的可视化，是非常难以预测有多少新的资源需要被增加、或者是何时。

计费是颇为复杂的，设计到某些细节的商品，折扣、市场活动等等。需要花费一番功夫的。

## 计算

### Citrix XenServer 

Xen 我们都知道是开源界非常有名的 Hypervisor，公有云的王者 AWS、以及OpenStack 的发起者 RackSpace都是使用的Xen作为其默认的 Hypervisor的。而 XenServer 是类似于 VMWare vsphere、oVirt这样的虚拟化集中管理工具。我们知道OpenStack默认是支持Xen Hypervior的，但是Mirantis 并不建议使用，而是选择交给最专业的合作伙伴来做这件事情。

XenServer 背后有个公司叫做思杰，玩开源项目失败的典型案例的那家公司。它提供Xen Hypervisor 商业上的支持。本插件就是这么来的，为Fuel在安装 Mirantis OpenStack 的时候提供XenServer的选项。

## 数据库即服务

### Tesora 数据库即服务平台

此插件所提供的功能是将 Tesora DBaaS 的组件基于Fuel自动的部署到 Mirantis OpenStack的环境当中，插件还扩展了Fuel web 界面，可以让管理员去配置一些必要的参数。

在Fuel环境下，将 Tesora DBaaS 会作为单独一个节点来处理的，会将Tesora 的一些服务（API、任务管理、调度）跑在这上面。

我们知道 Tesora 是 OpenStack 数据库即服务项目——Trove 的主要贡献者，它本身提供商业化的解决方案，可以参考下其[官方网站](http://www.tesora.com/database/)上认证过的数据库（关系型、非关系型等），比如国内比较受关注的：Oracle 11g和12C，DB2。

个人觉得可以和Ironic项目作一整合来提供解决方案。

## 消息队列

### Detach RabbitMQ Fuel 插件

Detach RabbitMQ 的Fuel插件，是在OpenStack环境中，将RabbitMQ部署在专用的机器上，在较大的环境中 RabbitMQ 会消耗大量的CPU时间，此插件能够将 RabbitMQ 从运行诸如数据库或其它OpenStack服务的控制器中分离出来。

关于RabbitMQ，消息队列实现的一种，最为常见的、使用颇为广泛的一种实现。是AMQP的完整实现。更多内容，请参考[Alexis的演讲稿](http://www.rabbitmq.com/resources/google-tech-talk-final/alexis-google-rabbitmq-talk.pdf)。

## 网络、NFV、NFVI、SDN

### 6WIND Virtual Accelerator Fuel 插件

虚拟加速器（Virtual Accelerator）是 6Wind 的产品之一，为虚拟网络基础设施提供网络包加速处理。

虚拟加速器运行在 hypervisor内部，通过从网络栈中卸载虚拟交换而达到无性能瓶颈的目的。从而大大地降低了包处理必要的 CPU 资源，所以对于处理大规模的网络流量的所在服务器的 CPU 核心不在提出更多的要求。

在简单的虚拟交换之外（使用 OVS 或 Linux 网桥），虚拟加速器还支持更多的网络协议，进而提供完整的虚拟网络基础设施。

虚拟加速器完全的和 Linux 做了集成，所以Linux现有的应用使用加速也毋需更改。

支持以下三种类型的：

* 基本：简单虚拟交换和IP转发
* L3：基本＋诸如vrf、过滤、网络地址转换等高级的3层特性
* ipsec：L3 ＋ ipsec

Fuel 的插件，则提供在部署 Mirantis OpenStack 的同时，安装和配置虚拟加速器到用户的环境中。注意：在Fuel安装虚拟加速器的过程中，会到6wind的官方去下载软件包，所以要确保Fuel能够访问互联网。

关于此虚拟加速的更多内容，请参考[官方站点](http://www.6wind.com/products/6wind-virtual-accelerator/)。

### Cisco ACI/GBP Fuel 插件

什么是思科 ACI／GBP？ 

思科的应用程序规则基础设施控制器，基于开放的 API 而设计，能够集成到云计算平台中，比如OpenStack。

这个插件就是将思科 ACI/GBP 作为 Mirantis OpenStack 的后端，而且还可以在 Fuel 的图形界面做一些能满足基本需求的配置。

具体的用户指南，请访问[Fuel插件文档](http://plugins.mirantis.com/docs/a/c/aci_opflex/aci_opflex-7.0-7.0.7-1.pdf)。

更多内容，请参考[思科 OpFlex 架构概览](http://www.cisco.com/c/en/us/td/docs/switches/datacenter/aci/apic/sw/1-x/openstack/b_ACI_with_OpenStack_OpFlex_Architectural_Overview.pdf)。

### Juniper Contrail Networking Fuel 插件

要摸清楚插件之前，先要搞明白 Contrail 是什么？

先到[OpenContrail](http://www.opencontrail.org/)这个网站去阅读一些内容。注意，这里是 Juniper Contrail 不是 OpenContrail， 号称是开源的软件定义网络解决方案，可以自动化的、灵活的编排、创建高扩展性的虚拟网络，具有下面特性：

* API 调用（RESTful 或者是 python的类调用）
* 分析引擎：流量报告、状态等
* OSI 模型2～4层的管理
* 服务链架构：可以透明的通过服务实例来过流量，诸如IDS、防火墙、和DPI。
* 虚拟网络访问规则控制

此插件是用来为 Mirantis OpenStack 使用 [Juniper Contrail ](http://www.juniper.net/us/en/products-services/sdn/contrail/contrail-networking/)进行配置和部署，默认的配置只是将Contrail相关组件安装好，若要支持高级特性如DPDK、ToR、SR-IOV等，则需要特别的配置，需要理解Contrail本身的一些知识，如VN，vRouter等。


### Mellanox Fuel 插件

Mellanox 是知名的网卡制造商，针对云计算其有两款网卡：[ConnectX-3](http://www.mellanox.com/page/products_dyn?product_family=119&mtag=connectx_3_vpi)和[ConnectX-4](http://www.mellanox.com/page/products_dyn?product_family=201&)，基于虚拟内部互联协议（VPI），支持EDR 十万兆 InfiniBand和 十万兆以太网的互联。提供高性能的、健壮的网络解决方案。

此插件允许用户在Fuel的界面下配置Mellanox的网卡。设置一些满足基本需求的参数。

在介绍下 Mellanox的NEO，这是Mellanox的一款针对横向扩展的计算网络开发的管理平台。运维人员基于此平台可以部署、监控和操作现代数据中心。即SDN 控制器，更多此内容，请参考：[官方介绍](http://www.mellanox.com/page/products_dyn?product_family=220&mtag=mellanox_neo)。

### MidoNet Fuel 插件

MidoNet 是一款针对基础设施即服务（Infrastructure as a service）的网络虚拟化软件，基于Apache许可证发布，开源、企业级的overlay 虚拟网络。更多关于MidoNet的内容，请移步：[MidoNet网站](https://www.midonet.org/)。

此插件是Fuel 能够安装和配置MidoNet到 Mirantis OpenStack环境，不过对于MidoNet本身来讲，有两种选择模式，一种是完全开源的软件，直接下载安装即可；另外一种就是Midokura 企业级的MidoNet，但是这个是由密码保护的仓库，必须从Midokura拿到相应的证书方可下载和使用。

MidoNet 在业界的口碑算是后起之秀，而且社区也标榜是专门针对OpenStack的网络解决方案开源的伙伴。拭目以待吧！


### FWaaS Fuel 插件

防火墙即服务（FWaaS）是Neutron的一个插件，它能够为OpenStack提供了网络管理的功能。FWaaS 使用 iptables来设置防火墙的规则，若是和安全组来作比较的话，安全组是在实例这层来进行的操作，而FWaaS是在路由这一层进行的操作。

在Fuel安装完成FWaaS之后，剩下的操作就是在OpenStack Horizon下即可操作完成。唯一的需要注意的问题就是和安全组的重叠部分。仔细理解了也没有多大问题。

### VPNaaS Fuel 插件

先来介绍些概念：

| 软件名称 | 描述 | 
| ------------ | ------------- | 
| VPNaaS  |  VPN 即服务，Neutron 的一个扩展，通过互联网连接两个私有网络。   |
|  IPSec       |   通过为每个IP包进行验证和加密来让因特网协议来作安全的通信。  |
|  OpenSwan       |  Linux 下的IPSec实现，它支持几乎所有和IPSec有关的扩展（RFC＋IETF），包括IKEv2、x.509 数字证书、NAT Traversal等    |
|   IKE      |  互联网密钥交换，是一项在 IPSec 协议套件中用于设置安全协会（SA）。   |

此插件是将Neutron的VPNaaS扩展安装到Mirantis OpenStack 环境中，让你的OpenStack环境支持VPN。也就是意味着，在OpenStack 不同的云中，或者是OpenStack不同的租户之间建立一个私有的网络。


### Nuage Networks VSP/VRS Fuel 插件

Nuage 网络虚拟服务平台（VSP）是一款overlay的SDN，能够为OpenStack 提供完整的虚拟网络能力。它替代了OVS，即将VSD（虚拟服务目录）和VSC（虚拟服务控制器）整合的VRS（虚拟路由和交换）。

和所有的插件一样，此插件的功能是在Fuel安装、部署OpenStack的时候，可以将Nuage 网络VSP/VRS 整合到 Mirantis OpenStack 环境中。

Nuage 网络是一款强大的SDN解决方案。更多内容请参考其官方站点[介绍](http://www.nuagenetworks.net/products/virtualized-services-platform/)。

### PLUMgrid ONS Fuel 插件

本插件是将 PLUMgrid 开放网络套件（ONS）的SDN安装和部署到 Mirantis  OpenStack中。

来介绍下 PLUMgrid 合作伙伴对其产品的描述：ONS 是一套集成的套件，能够提供TB级的性能级别、生产环境的可靠性、以及为混合的数据中心提供安全的多租户环境。基于先进的 PLUMgrid 平台和 IO Visor技术，ONS套件能够让用户创建虚拟私有域来提供隔离、安全和跨租户的规则增强。

主要特性有：

* 性能：每服务器达到40 Gbps；硬件支撑；TB级的聚合。
* 扩展：跨虚拟域、负载、和多机柜和地理位置的扩展。
* 同时支持软、硬件的网关，并支持高可用。
* 安全：基于虚拟域端到端的加密，以及跨租户的隔离。
* 运维工具：包括了多个强大的网络、监控和故障诊断工具。

### VMware DVS Fuel 插件

Mirantis OpenStack 是支持 vCenter 环境，或者是和KVM混合的异构环境的。那么因此需要支持 VMWare的分布式交换机，社区本来就有Neutron ML2 插件的 vmware_dvs 驱动，提供了在如此环境只下让Neutron来掌控网络。因此而网络具备了如下一些功能：

* 创建多层网络的能力（举例：web层、db层、应用层）
* 对于IP地址的管理和安全组规则的控制
* 拥有了插入配置到自有服务中（例如，防火墙、IPS）
* VPN／网桥到远程物理主机。

在部署使用之前，要确保dvSwitch已经配置好。

### VMware NSXv Fuel 插件

VMWare NSX 是VMWare的一款网络虚拟化软件，仅此而已，更多内容请移步VMWare[ NSX](https://www.vmware.com/products/nsx)，在上述DVS的描述中，我们得知 Mirantis OpenStack是可以整合 VMWare 平台的。但是注意一些限制：murano、sahara、ironic等组件均不能够做到支持。

## 运维管理

### LMA-工具链 

针对OpenStack的监控和日志分析，Fuel／mirantis 专门开发了一套工具集，先来看看组件图：

![](http://fuel-plugin-lma-collector.readthedocs.io/en/latest/_images/toolchain_map.png)

从上图中我们可以看到，首先OpenStack环境中所有的节点都需要安装 LMA Collector（现在叫做StackLight Collector），然后不同的数据交给不同的组件去分析、确认；InfluxDB-Grafana是负责计量动态分析的；Elasticsearch-Kibana是对日志和事件做分析的；Nagios Alerting 插件顾名思义即是作告警的。下面我们作一一的介绍。

### LMA Collector Fuel 插件

所谓的LMA，就是日志、监控和告警的英文单词首字母缩写的简称。主要的组件所使用的开源项目，分别是[collectd]
(https://collectd.org/)、[Heka](https://github.com/mozilla-services/heka)以及使用Lua写的一个用于编码、处理、解码操作数据的Heka插件，主要的输出数据有下面几种类型：

1. 日志和事件，会发送给 Elasticsearch 用于索引。
2. 一些指标会发送给InfluxDB。
3. 一些 OpenStack 服务的健康指标会发送给 Nagios。
4. 注释信息会发送给InfluxDB。

此插件用于使用 Fuel 直接安装、部署 LMA Collector 到每个OpenStack节点中。

### Elasticsearch-Kibana Fuel 插件

Elasticsearch-Kibana Fuel 插件是用于安装和配置 Elasticsearch 和 Kibana，而这两款软件是用于访问 OpenStack 事件和日志分析的。

简单介绍下 Elasticsearch 和 Kibana 是什么：

| 软件名称 | 描述 | 
| ------------ | ------------- | 
| Elasticsearch |  是一款基于 Lucene™ 搜索引擎的应用，可以让数据像日志信息一样便于浏览和相互关联起来。使用Java语言撰写，目标是作一个全文检索的应用，将Lucene的复杂性隐藏起来，使之更加的简单、易用。 | 
| Kibana | 开源项目，基于Apache许可协议，是配合 Elasticsearch 使用的浏览基本的分析和搜索。非常的容易上手。  | 


更多具体的操作和分析，请查看插件[文档](https://3a98d2877cb62a6e6b14-93babe93196056fe375611ed4c1716dd.ssl.cf5.rackcdn.com/e/k/elastisearch_kibana-0.9-0.9.0-1.pdf)。其实还是非常复杂的。

### InfluxDB/Grafana Fuel 插件

InfluxDB/Grafana Fuel 插件是Fuel在安装、部署OpenStack时，可以一同部署 InfluxDB 和 Grafana。InfluxDB 是一款强大的分布式时间序列数据库，用来存储和检索基于时间序列的指标数据。此两款软件解决了这样一个问题：**我的OpenStack环境做了何种变更？什么时候？为什么？** 

简单介绍下 InfluxDB 和 Grafana 是什么：


| 软件名称 | 描述 | 
| ------------ | ------------- | 
| InfluxDB | 是一款时间序列、度量和分析的开源数据库（基于MIT许可协议），使用 Go 语言撰写，没有任何外部的依赖。目标是为 DevOps 的用例场景，度量、传感器数据、以及实时的分析等。  | 
| Grafana  | 是一款通用目的的仪表和图表编排的工具，基于 Apache 许可协议。致力于为时间序列度量的可视化提供丰富的途径，主要是体现在图表，但也以插件的面板架构允许其它的可视化数据呈现。目前支持的有 Graphite、 InfluxDB 、OpenTSDB等的数据源，Grafana 被大量的应用在基础设施监控、应用监控和度量分析中。  | 

### LMA Infrastructure Alerting Fuel 插件

此插件是为 Mirantis OpenStack 安装和配置 Nagios™，从而提供了告警和升级的功能。

| 软件名称 | 描述 | 
| ------------ | ------------- | 
| Nagios |  一款非常流行的开源监控软件！  | 
| Apache HTTPD | web服务器曾经的王者，改变格局的软件！  | 

### SNMP Trap Daemon for Zabbix Fuel Plugin

### Zabbix Fuel Plugin

### Ceilometer Redis Plugin


## 安全认证

### LDAP Fuel 插件

此插件可以扩展 Mirantis OpenStack 的认证能力，让Keystone的后端支持 外部的 LDAP 服务来做认证。使用此插件就意味着除系统用户之外的所有用户都将通过外部的 LDAP 服务器来进行认证。

需要注意两个事项：

* Fuel 本身不会对所配置的外部的 LDAP 作任何的验证。
* 要在部署Fuel和Mirantis 之前部署好 LDAP服务器，并且可以让控制节点可以访问到。

至于具体到LDAP服务，如微软的活动目录、红帽的LDAP、还是其它厂家的未知。但Open LDAP的可能性居多。但国内的场景似乎是微软和IBM的居多。


## 存储

### EMC - ScaleIO Fuel 插件

### EMC - ScaleIO Fuel Plugin for Cinder

### NetApp Fuel Plugin

### Pure Storage Fuel Plugin

### SolidFire Fuel Plugin

### SwiftStack Fuel Plugin

SwiftStack 是一家专注于OpenStack 对象存储子项目 Swift的，并提供商业产品和服务的公司。某种程度上，SwiftStack 和 Mirantis 是竞争的关系，我们知道 Mirantis 在对象存储方面是倾向于提供 Ceph 的。但是这并不影响他们之间的合作，这就是开源的力量！

使用此插件可以在Mirantis OpenStack 下运行由SwiftStack 控制器管理下的Swift集群。

让我们简单理解下SwiftStack所提供的一些功能的角色：

| 组件名称 | 描述 | 
| ------------ | ------------- | 
| SwiftStack On-Premises controller | 提供了在用户私有的地方的管理服务，从而帮助用户部署和管理Swift集群。    | 
| SwiftStack Public Controller| 和On－Premises具有同样的功能，但是是在公共的地方就可管理。  | 
| SwiftStack Nodes |  安装了Swiftstack 代理和软件包的节点，从而可以让SwiftStack controller进行管理， |

进一步了解 SwiftStack，请阅读其[官方文档](https://www.swiftstack.com/docs/)。

## VNF 

### Calico Fuel 插件

Calico Fuel Plugin 为 Mirantis OpenStack提供了无缝的、可扩展的、安全的3层虚拟网络。通过替代OpenStack 原生的网络模式，Calico 目标是解决绝大多数需要 3 层的连接的负载问题，提供高效的、容易做故障排查的网络，从而避免了复杂的、低效的覆盖网络模式。
更多内容请访问[Calico开源项目网站](https://www.projectcalico.org/learn/)。