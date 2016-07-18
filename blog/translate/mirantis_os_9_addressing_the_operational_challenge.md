# Mirantis OpenStack 9.0 —— 解决运维面临的挑战

OpenStack ，正如我们所耳熟能详的那样，是一款分布式的软件系统。而且它还是100%开源的——比如，没有哪家单个的厂商能够决定其最终的走向，也因此而很难将其产品化。另外，其发展是异常的迅猛，如果你没能及时的学习的话，最多半年技术就会落伍。所有的这一切都造就了企业的 IT 团队难以驾驭 OpenStack。而这也正是 [Mirantis OpenStack](https://www.mirantis.com/products/mirantis-openstack-software/?utm_source=wwwmirantiscom&utm_medium=text&utm_campaign=MOS%20weight%20increasing)为何让运维更加简单的缘由。

[Mirantis OpenStack 9.0](http://software.mirantis.com/)，我们最新发布的版本，基于 OpenStack Mitaka，但是增加了新的功能，不仅在初始化部署方面（Day 1），还在部署之后的运维方面发力（Day 2）。另外，新版本还引入了新晋的负载应用更加的简单，当然，我们依然坚守稳定性这个原则。在发布了两个积淀的版本之后，我们俨然已成为业界最为稳定的发行版，我们将会持续的在这个重要的维度上发力。也正是由于我们在稳定性上坚持不懈的努力，我们赢得了大型企业客户的信任。

## 解决令人头疼的运维问题

下面示意图很好的解释了 Mirantis OpenStack 的生命周期管理：

![](https://www.mirantis.com/wp-content/uploads/2016/07/mirantis-openstack-9-0_ops-diagram.jpg)

因为没有任何的云计算平台是一成不变的，9.0 版本的发布是聚焦于上图中的阴影部分的，即部署之后的变更部分。有太多的原因在部署之后作变更了，比如有可能是增加容量、又或者是通过插件机制引入[ StackLight](https://www.mirantis.com/blog/stacklight-logging-monitoring-alerting-lma-toolchain-mirantis-openstack/)、又或者是更改配置参数，比如 DHCP 的地址范围等等。

作为 OpenStack 的管理项目——[Fuel](https://www.mirantis.com/products/mirantis-openstack-software/openstack-deployment-fuel/)，在此9.0当中，能够保证在部署之后添加插件，还毋需重新部署整个云平台环境。依然在诠释着我们“无锁定”哲学，哪怕是在初始化部署之后。如果保持设置无锁定，就可以做类似的变更而毋需重新部署。

作为 Mirantis 希望每个人都使用 Fuel 即可完成所有的任务，大型的云的运维人员需要更为灵活的环境来集成他们现有的工具，基于此，Mirantis OpenStack 9.0 可以导出Fuel的设置给其它的配置管理工具。

另外，在9.0当中，Fuel继承了执行任务的历史（用于审计、故障修复等），允许运维人员自定义部署任务，从而实现复杂的生命周期管理编排，比如回滚升级。

当然，我们依然保持我们一向的优势——即让初始化部署更加的简单。在原来的版本中，如果你在部署当中遇到了网络问题或者是硬件问题，你会纠结自己是运气不太好。你需要修复问题之后，然后重启所有节点的整个部署流程。在9.0当中，此种现象将不复存在，你可以随时停止和重启部署过程，而且仅需重新部署哪个失败的节点即可。

在9.0中，运维人员拥有了非常灵活的部署操作系统的部署平台，而且可以分别部署 OpenStack 到任何的节点中（或者是和过去老的版本一样，同时去部署）。同样，这依然为整个初始化部署带来了更大的灵活性，通过将Fuel安装到基础的操作系统中，等同于快速的准备好了环境，可以测试一些诸如网络和硬件兼容性的问题，然后，再去触发部署 OpenStack 的组件和服务。此种灵活性，让用户更加的节省时间，避免过多的在故障修复和重新部署方面浪费时间。

基于任务的部署特性现在并行化了，经过我们完整的测试，相比于过去串行的部署，速度要比之前快2倍之多，而且可以用于生产环境（在8.0中属于技术预览特性）。

## 没有负载的云不能称之为真正的云

当然，OpenStack 本身依然在以惊人的爆发力在继续前行。这里的终点指的是运行在其之上的负载。然而不幸的是，这个显而易见的事实，却常常被人们忽略。结果就导致没有租户登录、云平台无人问津的孤立局面。为了避免这样的情景发生，我们不遗余力的让 Mirantis OpenStack 更加容易的运行负载。

在9.0中，我们引入了整个 NFV 基础设施加速特性（英特尔称之为[增强的敏感平台](https://software.intel.com/en-us/articles/openstack-enhanced-platform-awareness)），例如支持大页、SR-IOV、NUMA/CPU 绑定、以及作为技术预览的DPDK。尽管这些特性在社区版本 Kilo 发布时就有了支持，我们之所以这个时候才引入，是因为我们坚持两个原则：

1. 这些功能足够的稳定，可以用于生产环境了。
2. 获得 Fuel 的支持，并能够容易配置。

对于开发者和 DevOps 的实践者来说，我们在 Murano 中支持了[TOSCA](https://en.wikipedia.org/wiki/OASIS_TOSCA) 基础设施编排框架，Murano 是 OpenStack 的应用程序目录和编排，Murano 现在还可以模拟工作流的执行，能够让技术人员先行测试而毋需先部署它，从而加速 Murano 应用程序软件包的开发流程。

最后但依然重要的是，在9.0中囊括的 OpenStack 大数据项目——[Sahara](https://www.mirantis.com/products/data-processing-sahara/)，支持了新版本的[Cloudera](http://www.cloudera.com/)CDH (v5.5) 和支持高可用的[Hortonworks](http://hortonworks.com/)HDP (v2.3 with Ambari)，以及[Spark](https://en.wikipedia.org/wiki/Spark_(software))(v1.6)。Sahara 和 [Ironic](https://wiki.openstack.org/wiki/Ironic)的集成也已成事实，所以大数据的科学家们再也不用为虚拟机不能满足而惆怅了，可以放在物理裸机上来运行。另外，块存储设备驱动的性能也作了一定的优化。

## 最为稳定的发行版

作为最为稳定的 OpenStack 发行版，我们继续保持着这个记录，这次，我们还增加了额外的测试，而且所增加的自动化测试要比原来的多96%。在 Mitaka 的版本，我们修复的 Bug 数社区[排名第一](http://stackalytics.com/?release=mitaka&metric=resolved-bugs)，共计修复了 3,700 个 Bug，另外，我们还额外在 Mirantis OpenStack 9.0 中修复了 1，900 个 Bug。

用户经常会问我们：“发行版的价值何在？” 除了生命周期管理和新晋负载之外，稳定性是最大的理由！如果你跟进的是社区主干版本，你将需要全新的重建内部专有的知识来对最新的特性作大量的 Bug 修复（此例的数量是 1，900）。

我们还对改进稳定性作了多项酷炫的功能，首先，用户现在可以对诸如 RabbitMQ 之类的服务进行设置资源阀值，如CPU、内存、磁盘等消耗选项。这可以让在同一台物理机（或虚拟机）上其它服务更好过点。对于 SAN 连接的多路径驱动支持良好，用户现在可以为其存储配置冗余的路径从而实现高可用。老的 L3 路由（被DVR取代的遗留技术）也支持高可用模式。

## 更为灵活的基础设施选择

现在，作为用户的你可能对于我们的纯粹的哲学思路已经非常的熟悉了。OpenStack 是我们竭尽全力所做的唯一的事情，而不是去利用它来推进我们其它的产品，我们确保 OpenStack 能够在广泛的合作伙伴生态圈中很好的工作，为用户带来业界一流的解决方案。~~我们这样的理念其实是直接否定了我们的竞争对手所推动的 co-engineered 理念的，他们的状态是在 OpenStack、宿主操作系统、以及Hypervisor之间有依赖，由多个不同的供应商来提供。~~ 时间会证明一切，但是我们也看到历史是站在我们这边的。毕竟，如果说我们的竞争对手是对的，那么Oracle 数据库就不会超越IBM、HP 或 DEC。

通过和 Oracle 的通力合作，我们在9.0中增加了对 Oracle Linux 的支持。这也就是说 Mirantis OpenStack 不仅能够和 Ubuntu、RHEL、VMWare、Xen进行互操作之外，现在还可以和 Oracle Linux进行互操作了。

此外，Fuel 社区开发的插件列表也一直在持续增加，从上个版本的170到现在发布的已经增加到了[220](https://github.com/search?p=16&q=fuel-plugin-&type=Repositories)。和过去一样，用户可以在我们 Fuel 插件[目录](https://www.mirantis.com/products/openstack-drivers-and-plugins/fuel-plugins/) 中找到 Mirantis 验证过的插件。

最后，Ubuntu 用户的福音来了，可以动态的构建 Ubuntu bootstrap OS了（在8.0时引入），用户还可以在部署的时候注入额外的软件包到主机到镜像，能够让用户不仅可以定制 bootstrap还能定制部署中的镜像。

## 从哪里获得？

还在等什么？从[这里下载](http://software.mirantis.com/)并开始体验吧！看看你是否能够将所有的这些激进的创新给利用起来。

请加入我们在7月14号周四举办的线上发布会：[Mirantis OpenStack 9.0 新特性](https://content.mirantis.com/2016Q3-Webinar---Whats-New-in-Mirantis-OpenStack-9_Landing-Page.html)。

通过下面视频来了解更多：

* [Mirantis OpenStack 9.0 概览](https://youtu.be/9fmRsb9Q768)
* [管理 Murano 应用的灵活性扩展](https://content.mirantis.com/Murano-TOSCA-for-Mirantis-OpenStack-9-Landing-Page.html)
* [改进的运维和生命周期管理](https://youtu.be/W71__59GirY)
* [新的 NFV 特性](https://www.youtube.com/watch?v=5tZDT_Ua8W8&feature=youtu.be)

想知道更多新特性的细节和改进，请移步阅读我们的[发行概要](https://docs.mirantis.com/openstack/fuel/fuel-9.0/release-notes.html)和[9.0有何新特性](https://www.mirantis.com/products/mirantis-openstack-software/what-is-new-in-9-0/)。

 