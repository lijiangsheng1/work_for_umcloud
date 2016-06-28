# OpenStack ironic 是什么？为什么它是如此的重要？

作者：Serge Kovaleff、 Pavlo Shchelokovsky、Yuriн Zveryansky、Vlad Drok、Vasyl Saienko、Anton Arefiev、Andrey Shestakov

翻译： 适兕  校审：

原博客地址：[What is OpenStack Ironic, and why does bare metal matter?](https://www.mirantis.com/blog/openstack-ironic-bare-metal-matter/)

------

在[Mirantis OpenStack ](https://www.umcloud.com/products/mirantis-openstack-software) 8.0中，有一个非常显著的特性，那就是整合了 OpenStack ironic 到 Fuel 当中。所以我们认为讨论一下什么是 OpenStack ironic 很有必要，以及它是如何做到以虚拟化的方便性还为用户带来裸机的性能的原理。

## 什么是 OpenStack ironic？

要是以简短的形式回答这个问题的话，OpenStack ironic 就是一组执行裸机部署以及裸机部署相关的活动的项目。但是它实际上该如何解释了呢？

要讲清楚这个，我们需要一点背景知识。我们知道 OpenStack 是一款开源的、扩展平台，用于构建公有或私有云。其多数时候是被作为 IaaS（基础设施即服务），由诸如计算（Nova）、网络（Neutron）、存储（Cinder）及其它项目所组成，甚至还有一些如 Murano 这样的 OpenStack 应用程序目录的平台即服务的项目。

所有的这一切的实现都是为了用户在虚拟化解决方案之上的使用案例。为了支持虚拟化，OpenStack 支持了多种 [hypervisor](https://wiki.openstack.org/wiki/HypervisorSupportMatrix)：KVM、Xen、QEMU、Hyper-V、VMWare、LXC、Docker等。除去其它优点不说，光论虚拟化让 IaaS成为了可能，能够让用户自助部署配置虚拟机，实质上是让用户能够在界面或命令行能够自己创建服务器。

在一些场景下，虚拟化的环境是不合适的，这些场景的用户需要的是真实的、物理的、裸金属的服务器。为了能够满足这些场景的自助服务需求，OpenStack 需要支持裸机的部署。

这就是 ironic 的由来！

在 OpenStack 中部署裸机就意味着用户可以直接操作硬件设施、部署应用负载（镜像）到真正的物理机器，而不是运行在 hypervisor 之上的虚拟机中。

为了实现这个功能，Nova 包含的虚拟化驱动中其中一个就是调用 Ironic 来启动裸机节点。基于 Ironic 的虚拟化驱动，OpenStack 的用户使用计算 API 就可以像原来启动一个虚拟机实例一样启动一台裸机实例。

## ironic 架构

在社区的用户[指南](http://docs.openstack.org/developer/ironic/deploy/user-guide.html)中有关于 Ironic 架构的主要组件，下图是一个较为简洁的表示：

![arch](https://www.mirantis.com/wp-content/uploads/2016/03/image1.jpg)

图一：Ironic 架构示意图

Ironic 每个交互的类型都是实际硬件的操作，例如 **电源**、**启动**、**部署**、**终端** 等等，以驱动的形式包装，Ironic 定义了一些接口，是每个驱动都能实现的。Ironic 的架构还允许厂商能够依据自己特定的硬件设备添加自己特定的厂商扩展实现。

在带外的话，Ironic 支持了一组驱动，能够让 Ironic 通过整体项目的持续集成测试，使用的是网关（在每次提交持续集成都会执行一组特定的测试）：

* **agent-ssh** 是一测试驱动，联合 ironic-python-agent 部署机制，以及基于 SSH 的电源控制来激活上游的持续集成。
* **pxe-ssh** 也是一测试驱动，联合 PXE 部署机制，以及基于 SSH 的电源控制来激活上游的持续集成。

使用 IPMI 标准，Ironic 可以管理大多数的硬件：

* **pxe-ipmitool** 在 Icehouse 和 Juno 版本中，均是默认建议的驱动。
* **agent-ipmitool** 是 Kilo 及其以后版本所建议的，使用 ironic-python-agent 实现了一些 PXE 驱动所没有实现的高级功能。

这种插件式的架构为[撰写第三方的工具和插件](https://wiki.openstack.org/wiki/Ironic/ThirdPartyVendorToolsDeveloperDoc)提供了很好的机会。举例来说，让我们来看看现有的 Ironic 生态的各种工具和插件，目前市面上主流的厂商均将他们的硬件平台列入支持，如 HP、Dell、IBM、Cray、以及富士通。

厂商  | 驱动  | 维护者  
--|---|--
Cray  | **pxe-snmp**  此驱动使用SNMP替代了IPMI，来管理电源。    |  Stig Telfer
Fujitsu  | **agent_irmc driver**  此驱动启用了基于 IPA（Ironic Python 代理）的虚拟机媒体部署，以及通过 Serverview 通用命令接口（SCCI）来做电源控制。 |  Naohiro Tamura
Fujitsu  | **iscsi_irmc driver**  此驱动启用了从 Diskimage Builder 构建的镜像的虚拟机媒体部署，以及通过 Serverview 通用命令接口（SCCI）来做电源控制。 |  Naohiro Tamura
HP  |**iscsi_ilo**  是 HP 服务器 ilo 管理的专门的驱动，用于基于 iSCSI 的部署机制，它不支持 PXE。  |  Ramakrishnan G
HPE  |**iscsi_pxe_oneview** 用于 ironic 的 HPE OneView 能够让 OneView 的用户将 Ironic 作为裸机的提供者来被管理。 此驱动实现了用于 Ironic 的核心接口，使用python-oneviewclient 来提供 Ironic 和 OneView 的 REST API 之间的通信。  |  Thiago Paiva Brito, Sinval Neto, Lilia Sampaio
HPE  |**agent_pxe_oneview**  用于 ironic 的 HPE OneView 能够让 OneView 的用户将 Ironic 作为裸机的提供者来被管理。 此驱动实现了用于 Ironic 的核心接口，使用python-oneviewclient 来提供 Ironic 和 OneView 的 REST API 之间的通信。 |  Thiago Paiva Brito, Sinval Neto, Lilia Sampaio
IBM  |**pxe_ipminative** 和 pxe_ipmitool 驱动很像，但是是 IBM 的  ‘pyghmi’ 程序库的替代品， ‘pyghmi’ 是python原生的 IPMI 工具，用于通用的 ipmitool。  |  Ling Gao

## ironic 组件

Ironic 生态包含了一系列的项目，概括如下：

* ironic 项目本身是负责在裸机节点上部署操作系统的，其包含了两个组件：
  * **ironic-api**
  * **ironic-conductor**
* **python-ironicclient** 是一 Python 客户端程序
* **ironic-python-agent** 就一代理（小程序），在 BootStrap 镜像中启动，它会为节点准备好部署和下载目标系统的镜像。
* **ironic-inspector** 协助硬件自检，目前仅工作在已知的、原已注册的节点。Mirantis 团队正在为此项目添加一些自动发现的能力。
* **bifrost** 一组 ansible playbook，用来安装和运行 ironic 依赖的其它 OpenStack 组件。
* **ironic-webclient** Horizon 的插件，基于 Angular.js 框架编写。
* **ironic-lib** 程序库，提供了一些通用的常见功能代码。
* **pyghmi** 是 IPMITool 的一个替代实现，很多运维人员都对这个基于 RedFish 规范实现的工具充满期待。

## 在 Fuel 中使用 ironic

Mirantis 正在开发的项目：**Fuel-Agent(FA)**，它是在 OpenStack Fuel 部署工具安装控制、计算以及其它节点角色之前部署到机器上的。此工具和 Ironic Python 代理（IPA）在功能上有一些显著的不同。

有一个非常显而易见的目标，就是要确保 Mirantis OpenStack／Fuel 所支持的硬件也要在 Ironic 项目中得到支持。还有，一些高优先级的客户要求我们在部署期间裸机节点要支持任意的分区。

为了完成这些任务，Mirantis 团队决定定制开发且以 FA 代替 IPA，FA 需要特定的 Ironic 部署驱动的开发。下面表格比较了这二者之间的功能差别。

![fa](https://www.mirantis.com/wp-content/uploads/2016/03/image3.jpg)

近来，一个叫做[Bareon](https://wiki.openstack.org/wiki/Bareon)的新项目，是 FA 的分支，但走得更远，已经开始开发一段时间来，可以浏览一下它们的开发[路线图](https://wiki.openstack.org/wiki/Bareon/Roadmap)。

## 集成

那么我们是如何将 ironic 集成到 Mirantis OpenStack 中的了呢？正如下图所示，这是一个组件的部署示意图，部署角色（控制器、Ironic）在 Fuel 用户界面中在集群创建期间被赋予。

![deploy](https://www.mirantis.com/wp-content/uploads/2016/03/image4.jpg)

图2: Ironic 部署示意图

当真正到了实际部署 Ironic 的时候，整个流程都是可选的；当部署集群的时候，用户可以通过一单选框启用它，如图3所示：

![choose](https://www.mirantis.com/wp-content/uploads/2016/03/image5.jpg)

图3:选择安装 Ironic

在 Mirantis OpenStack 8.0 中，裸机的部署使用了特殊的专用网络，所以如果用户选择了安装 Ironic，用户必须同时配置 L2 和 L3 层，如图4和图5所示。

![config](https://www.mirantis.com/wp-content/uploads/2016/03/image6.jpg)

图4:在Fuel界面中为 ironic 配置 L2

![l4](https://www.mirantis.com/wp-content/uploads/2016/03/image7.jpg)

图5: 在Fuel界面中为 ironic 配置 L3

现在，你已经对 Ironic 有了一个大体的认识了，接下来，让我们来看看它为何是重要的。

## 为什么用户应该在 Mirantis OpenStack 中使用ironic？

有虚拟机工作的好好的，为什么要折腾让 OpenStack 支持裸机服务器呢？是的，还有很多领域必须使用裸机服务，而虚拟机和容器均无法满足。他们有：

* 历史遗留的承担重要任务的应用程序并非是为云计算架构而设计的。
* 实时或者是“接近实时”的系统。
* HPC（高性能计算）。
* 大数据和相关的数据科学，以及机器学习等项目。
* 任务访问的设备和资源，无法被虚拟化。

OpenStack 通过 ironic 引入裸机的能力，同时具备是既能提供裸机服务器支持，又能提供虚拟机的支持：性能和可管理性兼顾。

同样，在 Mirantis OpenStack 中使用 ironic，也是两全其美的事情：既能获得 Mirantis OpenStack 一向过硬的技术，又能获得在线和社区支持的 ironic。

## 挑战、局限、正在开发的功能

利用社区项目的其中一个挑战就是需要和社区当前可用的功能进行协调，比如 Ironic 这样的项目。举例来说，目前 Ironic 社区版本并不支持多租户、自动发现、或者是和库存管理系统的整合，让我们对这些不足做一简单回顾，以及它和 Mirantis OpenStack 的关系是什么样的。

### 多租户支持

Ironic 上游目前的版本是在单租户情形下的，这也就意味着所有的裸机节点都在同一个网络中，那怕它是由不同的租户所拥有。这也意味着一个用户如果可以访问其中一台裸机节点的话，它也同样可以访问所有其它的裸机节点。访问隔离目前的状态是要么全都能访问，要么一台也访问不了。

想要让 Ironic 支持多租户，Ironic 就必须支持和虚拟机级别一样的网络隔离。要确保这样的话，裸机的实例就必须有诸如 VLAN、VxLAN或者其它的网络类型支持，要做到这样话，我们需要通过 LLC 提供给 Neutron ML2 插件必要的连接信息。连接信息允许设备驱动为裸机节点配置 TOR（机架顶部）交换机。

我们的团队，以避免网络交换机厂商锁定用户为目标，努力不懈，在上游创建了通用交换机 ML2 插件项目，此插件的源码托管在 GitHub [仓库](https://github.com/jumpojoy/generic_switch)，并且已经作为官方 Ironic 社区的一部分提交给了上游。

### 自动发现

Ironic Inspector 的发现部分就包含了这样的功能，所以一个新的节点是完全可以被发现并被检验的。

### 和库存管理系统的整合

现代的每个数据中心都有一个可靠的单一数据源——一个库存管理系统。我们对 OpenStack 运维人员做过一次小型的调查，并发现在 Google Sheets/MS Excel 和复杂的功能齐全的商业产品之间有差距。我们团队于是致力于填补这些差距（所谓之“填坑”），将 OpenStack 整合一些开源的系统。作为 PoC（概念验证）我们实现了一个“胶水层”——从给定的任意 RUL 导入 CSV。

### 更多挑战

除了上面提到的挑战之外，Ironic 目前对于在 Neutron 中支持的 DVR（分布式虚拟路由）的场景也有限，以及从 IPA 向 FA 转换中也有一些功能上的损失。当然，这些都在积极的进行改进。

## 资源

* [ironic 社区维基百科](https://wiki.openstack.org/wiki/Ironic)
* [ironic 用户向导](http://docs.openstack.org/developer/ironic/deploy/user-guide.html)
* [RedFish](https://www.dmtf.org/standards/redfish)
* [Bareon](https://wiki.openstack.org/wiki/Bareon)
* [Bareon 路线图](https://wiki.openstack.org/wiki/Bareon/Roadmap)
* [DVR](https://wiki.openstack.org/wiki/Neutron/DVR)
* [Ironic 的驱动列表](http://stackalytics.com/report/driverlog?project_id=openstack%2Fironic)
