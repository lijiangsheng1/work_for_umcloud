# 公有云对比私有云：AWS 和 OpenStack 的细节比较

## 引子：如何选择云计算平台，以及何时该上混合云

本文比较了 AWS EC2 和 OpenStack 的以下几个方面：

* 此两个平台的技术特性对比
* 业务特征的对比
* 开销的对比
* 如何决定使用哪个？又或者是二者都用

## 二者各自的定义

OpenStack:

> 来自[OpenStack.org](http://www.openstack.org/)的定义：“OpenStack 可以掌控数据中心中的计算、存储和网络的资源池，既可以使用[图形界面](http://www.openstack.org/software/openstack-dashboard/)也可以使用[OpenStack的应用程序接口](http://developer.openstack.org/)，OpenStack 可以运行[在流行的企业或开源技术](http://openstack.org/marketplace/drivers/)之上，这让其管理不同的基础设施成为了可能。”

AWS EC2

>  亚马逊可伸缩的计算云平台（Amazon EC2）是一种web服务，在云平台中提供可调整大小的计算能力，它被设计为让开发者更加容易作出 web-scale 的[云计算](http://aws.amazon.com/what-is-cloud-computing/)。

## AWS EC2 和 OpenStack 的技术对比

以下使用表格来进行比较

### 计算

|  | OpenStack  | AWS |
| ------------ | ------------- | ------------ |
|定义：计算是指虚拟机或服务器  |实例 |  实例／虚拟机|
| 规模：会为实例／虚拟机分配多少内存、CPU、以及多大的临时存储 |Flavors：默认的有：微、小、中等、大 |这个就非常的多了，比如X.large等  |
|提供的操作系统：云平台可以为最终用户能提供的操作系统类型  | 基本上所有基于X86架构的操作系统都可运行 | 由 AWS[市场](https://aws.amazon.com/marketplace/ref=csl_ec2_ami)提供的AMI镜像  |
|模版／镜像：装备有基本配置的虚拟机，可以基于此来创建虚拟机  |Glance OpenStack 管理员可以上传镜像并且为用户创建目录|AMI（亚马逊机器镜像）AWS，提供的将预先配置的镜像放在了[在线的市场](https://www.google.com/url?q=https%3A%2F%2Faws.amazon.com%2Fmarketplace%2Fref%3Dmkt_ste_amis_redirect%3Fb_k%3D291&sa=D&sntz=1&usg=AFQjCNFmcY6l7-YNSxjqFtEy7Gwysvjp-A)上  |
|  |用户可以上传他们自己的镜像 |用户可以上传自己做的镜像  |

### 网络

用来将上述的虚拟机／服务器彼此通信。当然，你也需要控制谁能访问这台虚拟机／服务器，如果是暴露在互联网，你还想保护这些虚拟机／服务器或采用防火墙。

|  | OpenStack  | AWS |
| ------------ | ------------- | ------------ |
|定义： 网络提供的是让用户和虚拟机能够通信的能力，让虚拟机不仅能彼此互联互通，还能够和外网连通。 | Neturon| Networking |
|虚拟机会拥有一个私有IP地址，还仅内部，不会路由到互联网 |每个虚拟机实例都会自动分配一个私有IP地址，通常使用DHCP。 | AWS也是使用DHCP服务来为实例分配一个私有IP地址。 |
| 公网IP地址 |浮动IP就是一个公网IP，可以让用户动态到添加到运行中的虚拟实例中。 |AWS 公网IP地址可以映射到主要的私有IP地址。  |
| 网络服务 |用户可以创建网络和网络功能，举例：L3转发、NAT、边缘防火墙、以及 IPSec VPN| AWS提供了 [VPC](http://aws.amazon.com/vpc/) 产品，用户购买了，可以创建虚拟路由、交换机等。 |
| 虚拟机流量负载均衡 |OpenStack提供了LBaaS（负载均衡即服务）从一个网络到应用服务提供流量等负载均衡 |ELB（可伸缩的负载均衡）会自动的将 EC2 实例的进入流量给分流。  |
|DNS，为虚拟机和web应用提供DNS条目的管理 | OpenStack 的DNS 项目(Designate) 正在孵化中，还未进入核心项目。|Route 53， AWS 的DNS 服务  |
|SRIOV，为了提供高I/O的性能和降低CPU的利用率，相比于传统的实现方式，是将网络设备虚拟化的技术  |每个 SRIOV 的端口都有虚拟功能分配（VF），SR－IOV 端口可以由基于硬件的虚拟以太网网桥提供，也可以是上游的物理交换机设备的扩展功能提供(IEEE 802.1br) | AWS亦提供SR－IOV的网络增强能力。 |

### 监控

每位管理员都希望获得资源（物理的或虚拟的）的使用范式和利用率的真实情况，也希望能够随时观看某个单独的资源的利用情况，甚至更希望能够将这些都和计费整合起来。

|  | OpenStack  | AWS |
| ------------ | ------------- | ------------ |
|定义： 监控、计量，云平台的使用情况 | Ceilometer| Cloud Watch |
|系统范围的计量和使用情况。 |能够收集云平台所有的物理、虚拟资源的使用情况 | AWS 云平台资源及其中的应用的监控服务， |
|用户针对自身的使用情况的计费  |当定义的条件得到满足持续以后的检索和分析，以及触发操作之后的数据。 | 收集和跟踪指标，收集和监控日志文件，并设置报警。 |


### 安全

你得为你的帐户设置SSH的公钥，以及密码，你也需要控制设置虚拟机实例的防火墙，能够让进出的流量有所控制。

|  | OpenStack  | AWS |
| ------------ | ------------- | ------------ |
|定义：用户虚拟机的访问控制  |密钥对，安全组 | 密钥对，安全组 |
| 密钥对：用户若要登录到虚拟机或实例，必须创建一个密钥对，Linux的话使用SSH，windows的话使用管理员密码 |当用户启动一个虚拟机时，用户可以注入密钥，从而实现SSH到实例。 |若要登录到实例，在启动实例时需指定密钥对的名称，当需要连接到实例时则提供私钥。  |
| 所谓的安全组是指一组网络访问的规则，比如端口，从而达到限制访问的目的。 |支持 |支持  |


### 认证

要达到控制某些用户可以访问你的云平台，以及管理云资源的授权，出于安全的考虑，一定会考虑到多元的认证。

|  | OpenStack  | AWS |
| ------------ | ------------- | ------------ |
|定义：用于云平台当中的虚拟机、存储以及其它资源的访问控制的认证和授权的方法。  | KeyStone| IAM——身份和访问管理 |

### 存储

块存储：

* 为虚拟机分配虚拟硬盘／卷，以让虚拟机拥有除启动卷之外的存储能力。
* 为虚拟机提供快照和备份。

对象存储：

* 存储诸如文件、视频、照片等对象。


|  | OpenStack  | AWS |
| ------------ | ------------- | ------------ |
|对象存储 存储文件、媒体、文档、图片等 | Swift／Ceph| S3- 简单存储服务 |
| 块存储 创建虚拟磁盘（卷） |Cinder | EBS——可伸缩块存储 |


### 数据库

可以使用数据库服务，而不是安装和配置他们自己的数据库。

|  | OpenStack  | AWS |
| ------------ | ------------- | ------------ |
|定义：  | Trove| RDS |
| 关系型数据库 |MySQL，PostgreSQL |用户客户选择是MySQL或Oracle11g的实例  |
|非关系型数据库  |Cassandra, Couchbase, MongoDB | Amazon SimpleDB，用户可以存储键值对到简单数据库中，适用于大量到读操作的应用。 |


### 编排

可以重复性的使用已经准备好的应用。

|  | OpenStack  | AWS |
| ------------ | ------------- | ------------ |
|定义：允许开发者将云应用的需求存储为一个文件，或者是定义资源的模版，（虚拟机、网络、存储、安全、模版、镜像等） ，就是一个应用运行起来所需要的资源。 | Heat | Cloud Formation |


### 大数据／并行处理

可以为用户的高负载的大型数据处理提供灵活的基础设施。

|  | OpenStack  | AWS |
| ------------ | ------------- | ------------ |
|定义：让用户能够处理大型的、可扩展的、高并发的数据处理  |Sahara | EMR——（可伸缩 Map Reduce） |


### 消息

|  | OpenStack  | AWS |
| ------------ | ------------- | ------------ |
|定义：可以在托管的应用和虚拟机／实例之间缓存和移动数据。  |Zaqar | SQS——简单队列服务 |


### 图形化用户界面（GUI）

你可以管理自己的云平台，或者按需自助服务。且兼容大部分主流浏览器。

|  | OpenStack  | AWS |
| ------------ | ------------- | ------------ |
|定义：浏览器管理或使用计算、网络、存储资源。  | Horizon | Console |


### 命令行界面（CLI）

从命令行来自动化、脚本化管理任务，即使用／消费云上的资源。

|  | OpenStack  | AWS |
| ------------ | ------------- | ------------ |
|定义：命令行界面为管理员提供了部署和拆除云计算资源（虚拟机、存储、网络）的能力  | 支持|支持  |


## 业务层的组件

### 多租户

分为两种情况，一种是按照业务单于、部门、和组织，将用户隔离，以满足规章的要求。二、为资源设置配额限制。


|  | OpenStack  | AWS |
| ------------ | ------------- | ------------ |
|定义：一个租户是一组用户，这些用户可以共享的访问资源（云平台），但用户是彼此隔离的  |项目／租户，管理员可以按照每个项目／租户定义资源的配额。 | 可通过使用 AWS [VPC](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Introduction.html)实现用户的隔离。 |

> 关于此处AWS的租户隔离，我心存疑问。

### SLA（服务水平协议）

要运行关键的业务应用，用户需要就最小宕机时间和云平台供应商达成SLA。

|  | OpenStack  | AWS |
| ------------ | ------------- | ------------ |
|定义：SLA 是一个云平台的可用性的保障。  | SLA 是 OpenStack 私有云供应商（企业内部IT部门／托管服务商）和使用私有云的业务部门之间的一种协议。 | 参考[AWS SLA](http://www.google.com/url?q=http%3A%2F%2Faws.amazon.com%2Fec2%2Fsla%2F&sa=D&sntz=1&usg=AFQjCNHwOcw5pMIXKf1mpdqE66A-enIouw) |

### 所有权和数据掌控

用户应该知道都有谁可以访问云平台中所存储的数据。各行各业均有转机的法律法规，比如保健行业、金融服务、政府等等，均会规定谁可以访问应用和数据。在有些用户／国家会面临政府安全和间谍部门的许可，才能获准访问公有的云数据。


|  | OpenStack  | AWS |
| ------------ | ------------- | ------------ |
|定义：当用户在云平台中存放应用和数据时，需要考虑谁拥有数据？以及谁能够访问它们  |OpenStack 云平台的用户 | 用户拥有数据，参考[AWS 协议](http://www.google.com/url?q=http%3A%2F%2Faws.amazon.com%2Fagreement%2F&sa=D&sntz=1&usg=AFQjCNEZvGwe9525Betg8aJpk_dJvH3o6Q) 第八章。|


### 生态系统

作为用户，你可能在使用私有或公有云时需要咨询或社区的帮忙，如果你部署的是OpenStack 私有云，经过认证的软硬件供应商会保证所有的兼容性问题都解决了，

|  | OpenStack  | AWS |
| ------------ | ------------- | ------------ |
|定义：硬件供应商、软件供应商、社区同行（开发者、用户、管理员）以及咨询确保云平台的运行  | OpenStack的[生态系统](http://www.google.com/url?q=http%3A%2F%2Fopenstack.org%2Fcommunity%2Fcompanies%2F&sa=D&sntz=1&usg=AFQjCNG0umToW6yrvqJTD6DgSZAU3IPwNA)：硬件、软件、和服务提供商，以及最终用户。OpenStack的代码是开源的。 | AWS的生态系统是顾问和独立供应商帮助用户使用AWS，AWS 是闭源产品。 |


### 高可用

如果一个云平台提供高可用的话，则托管在云平台上的应用是可以做失效切换的，作为用户是不会体验到服务间断的。

|  | OpenStack  | AWS |
| ------------ | ------------- | ------------ |
|定义：Region 和可用区域  | 数据和实例可以存放在不同的地理位置 ，满足冗余、延迟和法律的需求。| AWS EC2 在全球范围内有多个地区的数据中心，即Region，每个Region都拥有多个相互隔离的可用区域。 |


## 开销

在云平台中运行的服务器和应用的开销是可以操作的（OPEX）以及资本化的（CAPEX）。


|  | OpenStack  | AWS |
| ------------ | ------------- | ------------ |
|定义：使用云服务的开销  |使用[托管服务](http://www.openstack.org/marketplace/hosted-private-clouds/) | 基于小时／分钟的[计费](http://aws.amazon.com/ec2/pricing/)，只有使用了之后才会收取费用。 |
|  |购买硬件，然后搭建自己的OpenStack云平台 | 也可以使用预购的服务如：[预留实例](http://aws.amazon.com/ec2/purchasing-options/reserved-instances/)，[打折](http://aws.amazon.com/ec2/purchasing-options/spot-instances/) |
|  |从[OpenStack](https://www.openstack.org/software/start/)社区下载组件，然后雇佣工程师来安装、维护、增强、升级，这样的开销很难估计，因为运行云平台的大多是雇佣人员的开销。比如需要雇佣多少工程师？怎么知道什么时候该雇佣更多的工程师？如果对于云平台的需求较少，又该辞退多少名工程师？ |  |
|  |从OpenStack发行版厂商处购买许可，这涉及的前期授权费用，每年的支持成本以及后续的牌照续期。 |  |
|  |从 Mirantis 处购买订阅服务，获得高性价比的咨询、培训、支持等服务。 |  |

## 那么，你会怎么选？

因为云平台提供的同样的服务，那么就该考虑你自身的需求。对于即时的、临时的需求，AWS 和它的按需付费模式是蛮合适的，对于一些长期的项目，应该慎重一点，可以参考AWS官网上的[成功案例](http://aws.amazon.com/solutions/case-studies/)，OpenStack 网站上[也有](https://www.openstack.org/user-stories)。

根据AWS所列出的[用户案例／场景](http://aws.amazon.com/solutions/)以及[Gartner所推荐](https://www.gartner.com/doc/2976226/openstack-ready-mainstream-private-cloud)的OpenStack使用场景，可以总结如下的场景适用于OpenStack。

* **采用了DevOps 的开发 **：开发者可以访问 OpenStack 的API，实现基础设施即代码。
* **针对开发／测试的支持**：这种情况类似过去IaaS的自助门户。
* **高性能和网格计算**：很多这样的环境和OpenStack一样，均是开源的，OpenStack是非常适合它们对于基础设施灵活的需求的。
* **扩展的通用硬件来支持大数据**：比如Apache Hadoop、Apache Spark、Apache Cassandra。
* **哪些新兴的云原生应用**：这样的话，不会刻意的去迎合传统应用的兼容性，这也是大多数IaaS的私有云实施者使用的情景。


------

原文链接：[Public vs Private, Amazon Web Services EC2 compared to OpenStack®](http://redhatstackblog.redhat.com/2015/05/13/public-vs-private-amazon-compared-to-openstack/)