# 运营商在部署 NFV 时应考虑什么？

原文链接：[What should operators consider when deploying NFV](https://www.mirantis.com/blog/operators-consider-deploying-nfv-vudr-subscriber-data-management/)

NFV 本来就是为解决问题而生，其中最为主要的一点就是能够让运维人员快速的启动和伸缩新的应用程序。就目前的情景而言，一位运维人员若是要启动一个新的应用程序，依旧是一个相当复杂的过程，需要经过长时间的准备、规划，诸如数据中心分配空间、必须获得哪些特定的服务器／网络／存储等等。它需要被设计为具备99.999%的高可用架构，而且还需要和其它的网络环境整合。这一整套下来，花费可不小，且每个项目都需要财务部分的核准，而这样的话给运维人员留下的创新空间就极为有限了。

然而在拥有NFV之后，每一个应用都是软件的一个部分，可以运行在虚拟机的服务器、存储和网络环境中，将硬件和软件分离，这一下子将灵活性就提高了一个级别。NFV 的基础设施被当做一常见的实用程序，当需要启动一新的应用程序时，运维人员再也不需担心寻找机架、集成服务器，包括存储在内，所有的这些，NFV 早已提供，运维人员唯一需要去做的事情就是分配适当的资源。

另外，由于网络已经虚拟化且预先集成，所以使得集成变得更加容易。在应用程序相对简单、非订阅者感知的情况下，集成会工作的蛮不错。但是如果应用程序是订阅者感知的，那么它就需要与部署系统集成，这对于运维来说，每次这样的集成，都要花费 9〜12 个月的漫长过程，费用高达数百万美元。因此，对于订阅者感知的应用程序，NFV 是毫无灵活性可言的。

幸运的是，你可以把内置的虚拟用户数据仓库（vUDR，或订阅者数据管理即服务）作为NFV基础设施的一部分，这样就能恢复其灵活性。这也是为什么部分具有前瞻性的运维人员把vUDR纳入其 NFV 云计算上第一批用户感知应用。

这种做法带来了显而易见的益处。一旦 vUDR 到位，所有订阅者相关信息随时可以提供给那些需要用到它的应用程序。这样一来，在NFV上启动新应用不需要一对一的配置集成，运营商在订阅者感知的应用上也能享受到“敏捷”的滋味了。

![](https://cdn.mirantis.com/wp-content/uploads/2016/11/IaaS.png)

> **上图诠释:** 订阅者数据管理（SDM）是一项关键任务应用。在建立任何语音连接、访问任何数据服务、或发送任何消息之前，内部系统需要验证订阅者及其设备，都需要对他们的请求进行授权。SDM对于通信网络，就像氧气对于人类一样不可或缺 —— 不验证用户就无法提供服务。Openwave Mobility 公司的 vUDR SDM 解决方案已经在 Mirantis 的 OpenStack 环境中获得了验证，将其作为第一个NFV应用部署可以帮助运营商最大限度地利用NFV的敏捷性优点。

## Openwave Mobility vUDR 已获 Mirantis OpenStack验证

Openwave Mobility vUDR 是业界第一个基于NFV的订阅者数据管理解决方案，并已经在几家一级运营商进行了全球部署，在其整个语音和数据网络内管理用户配置文件数据。

Openwave Mobility 基于云的 vUDR 超越了传统 UDR 系统。其内置的联邦（federation）和复制功能意味着网络应用可以读写任何数据中心或数据仓库的数据，同时 NFV 基础设施一般所用的商用服务器充其量提供99.9％的可用性，而通过应用专用软件进程，Openwave Mobility 的 vUDR 在商用虚拟机上能够提供 99.999％ 的可用性。vUDR 保持轻巧灵活，它使我们的客户在短短两周内就可以上线新的应用，而传统的订阅者数据整合供应（subscriber data provisioning integration）平均需要9个月。

Openwave Mobility的vUDR，已经通过了Mirantis OpenStack环境的[验证](http://owmobility.com/press-release/mirantis-openwave-mobility-collaborate-on-subscriber-data-management-for-nf)。它提供了NFV 云计算的关键组件——SDM，这样运营商在部署NFV的时候可以真正实现其应有的敏捷性。