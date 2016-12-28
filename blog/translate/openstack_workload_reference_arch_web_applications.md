OpenStack 应用架构参考：web应用

在今天的业务场景中，web应用是普遍存在的。当我们使用浏览器来和互联网交互的时候，web应用是这一切背后的驱动力。通常所讲的web应用包括：网页邮箱、在线零售、在线拍卖、线上银行、即时通讯服务等等。

Web 应用通常会表现为对于IT资源的波动，包括预期的和意料之外的。失去响应则意味着不仅仅是用户不能满意，可能是丢失了一笔买卖。那么可以伸缩的Web应用以及支撑它的基础设施就显得格外的重要。和传统的静态的环境不同，云计算能够使得IT资源能够通过应用的负载（CPU利用率、内存使用率等）波动而动态的伸缩。

OpenStack 云计算平台能够使得Web应用进行自动的扩展，当然，OpenStack 远远不止于此，它还可以为几乎所有的IT应用提供灵活而省钱的平台。OpenStack 是一款开源的云计算平台软件，可以以数据中心的形式控制由计算、存储、网路组成的资源池，所有的管理均通过图形化的操作界面或者是友好的API。目前为止，全世界有上千家企业都在利用OpenStack来运行他们的日常业务。

本篇文章的目标群体是企业的IT架构师，文中所描述的架构和服务是典型的三层web应用，即在OpenStack云平台中运行的常见的LAMP软件栈。所谓的LAMP，即 Linux 操作系统、Apache web服务、MySQL数据库和PHP／Python／Perl，这是经过实践证明的具备高性能特性的web应用。

本文针对静态的虚拟化和动态伸缩两种场景，验证并推荐关于OpenStack必须的服务和可选的服务。另外，还提供了经过测试的配置文件，这些配置文件是利用一个WordPress实例来构建动态伸缩的Web应用，也就是Heat模板文件，功能是诸如为LAMP栈的每一层创建服务器、网络、负载均衡、以及自动伸缩等。

下图即为常见的web应用三层架构：

![](https://apigee.com/about/cdn/farfuture/ld85yTsB0f59wkEXksUD1Hzp5H6FQmnZeXoJ-K1wY6Q/mtime:1439316191/sites/mktg-new/files/3tierarchitecture1.png)

## 在OpenStack中跑Web应用

一个典型的web应用如上图所示，是分为三个层：表现层、应用逻辑层、数据层：

* web 表现层 —— 由 web服务应用集群来构成，用于响应浏览器客户端的静态内容或者是由应用动态生态的内容。
* 逻辑应用层 —— 应用服务器的集群所承载，用于流程内容和业务逻辑。
* 数据存储层 —— 数据库服务器集群，用户持久性的存储数据。

OpenStack 云计算平台由多个不同的服务（在OpenStack的世界中叫做项目）所构成和驱动。我们知道，OpenStack 将其旗下（大帐篷）的服务区分为**核心服务**和**可选服务**，（有些类似于Apache基金会治理下的顶级项目和孵化项目），若是仅仅利用OpenStack的核心项目，这样一个三层的web应用也就是相当于虚拟化一样，我们可以手动的去完成诸如扩展之类的动作。如果是添加了可选服务的话，则不可同日而语：

* OpenStack 编排服务（项目代号Heat），实现应用负载自动化的部署。
* 将OpenStack编排服务和Telemetry服务（项目代号Ceilometer），就可以随性的动态扩展和减缩应用负载了。
* OpenStack 数据库即服务（项目代号Trove)提供了数据库即服务的功能，实现了数据库的自动化部署和管理。Trove对于web应用来说的话是可选组件，在本文中我们并没有使用到。

如下图所展现的核心和可选的服务，

![](https://raw.githubusercontent.com/lijiangsheng1/work_for_umcloud/master/media/openstack_service.png)

以下表格对于一个典型的web应用所用到的OpenStack 核心和可选服务的简短介绍，更详细的参考，请访问[OpenStack 项目导航](http://www.openstack.org/software/project-navigator/)。

<dl>
<table style="width:100%">
  <tr>
    <td>计算（Nova）</th>
    <td>管理计算实例的生命周期，即按需生成、启动、销毁实例。</th>
  </tr>
  <tr>
    <td>镜像服务（Glance）</td>
    <td>存储和索引虚拟机磁盘镜像，在实例准备期间为OpenStack计算服务所使用。</td>
  </tr>
  <tr>
    <td>块存储（Cinder）</td>
    <td>将块存储设备的虚拟化管理，为使用块存储的用户提供自助式的API，而用户还毋需关心物理存储的位置和设备类型。支持流行的存储设备。</td>
  </tr>
  <tr>
    <td>网络服务（Neutron）</td>
    <td>为OpenStack的其它服务提供连接即服务的功能，提供统一的API，用来定义网络及其附件，支持常见的网络供应商和技术，也提供LBaaS和FWaaS，即负载均衡即服务和防火墙即服务。</td>
  </tr>
  <tr>
    <td>认证服务（KeyStone）</td>
    <td>为OpenStack的其它服务提供认证和授权。</td>
  </tr>
  <tr>
    <td>对象存储（Swift）</td>
    <td>可以通过 RESTful 基于HTTP的API 任意的存储和检索非结构化数据，内置数据复制，进而提供高容错的，本身具备大规模扩展的架构。</td>
  </tr>
  <tr>
    <td>图形化的管理和操作（Horizon）</td>
    <td>提供了一个可扩展的基于web的图形化的自助门户，和OpenStack的各个服务进行交互，可以完成一些常见的操作，如启动实例、分配IP地址，以及配置访问控制等等。</td>
  </tr>
</table>
</dl>

可选的服务有：

<dl>
</table>
  <tr>
    <td>编排服务（Heat）</td>
    <td>对OpenStack的服务进行自动化的编排，既可以使用其自带的HOT模板也可使用AWS CloudFormation模板，API既支持OpenStack自身的，也兼容AWS CloudFormation的API。</td>
  </tr>
  <tr>
    <td>计量（Ceilometer）</td>
    <td>为OpenStack的计费、基准测试、可扩展性、统计目的等提供监视和计量的服务，</td>
  </tr>
  <tr>
    <td>数据库（Trove）</td>
    <td>数据库即服务，可以提供关系型和非关系型的数据库引擎。</td>
  </tr>
</table>
</dl>

下图显示了上述服务的最为基本的交互模式，更多细节请参考：[OpenStack概念架构图](http://docs.openstack.org/admin-guide/common/get-started-conceptual-architecture.html)。

![](https://raw.githubusercontent.com/lijiangsheng1/work_for_umcloud/master/media/openstack_arch.jpeg)

## OpenStack web 应用的结构

web 应用典型的三层包括：web表现层、应用程序层、以及持久数据层。本章的内容将会讨论它们，也会根据基于OpenStack的web应用所增加的架构进行进一步讨论。

<dl>
<table style="width:100%">
  <tr>
    <th>架构组件</th>
    <th>概要描述</th>
  </tr>
  <tr>
    <td>web表现层</td>
    <td>一组Web服务的集群，用来为客户端的浏览器准备静态内容或由动态程序所生成的静态内容。</td>
  </tr>
  <tr>
    <td>应用层</td>
    <td>一组应用服务器的集群，用于处理内容和业务逻辑。</td>
  </tr>
  <tr>
    <td>数据库层</td>
    <td>数据库服务集群，用户持久化数据的存储。</td>
  </tr>
  <tr>
    <td>负载均衡器</td>
    <td>为了分发负载，至少需要两个负载均衡器，一个用来均衡前端表现层的web流量，另外一个来平衡应用服务器的负载。</td>
  </tr>
  <tr>
    <td>关系型数据库管理系统</td>
    <td>本文的例子中使用的数据库是主备模式的配置，数据持久的存放在块存储中，且会周期性的进行备份。</td>
  </tr>
  <tr>
    <td>防火墙</td>
    <td>考虑到安全性，每一层均必须强制的配置防火墙。</td>
  </tr>
  <tr>
    <td>网络配置</td>
    <td>网络务必配置在不同的层过滤掉非必要的流量。</td>
  </tr>
  <tr>
    <td>自动伸缩</td>
    <td>自动伸缩希望应对意想不到的负载／流量高峰，当负载减少时,则恢复到正常。</td>
  </tr>
</table>
</dl>

图4，OpenStack 的web应用架构

![](https://raw.githubusercontent.com/lijiangsheng1/work_for_umcloud/master/media/OpenStack_web_app_arch.jpeg)

### 负载均衡

负载均衡一般基于的算法是轮询、最后连接、随机等。如果应用程序并非是云原生应用，且需要维护会话状态，负载均衡即服务（LBaaS）要被配置为一直到某一固定的虚拟机上请求服务。Neutron 支持专有的和开源的负载均衡的驱动，可以让OpenStack运维人员灵活的作出选择。本文所使用的LBaaS版本是1.0，2.0也在 OpenStack Liberty 版本中得到了支持。无论哪个版本默认都是使用HAProxy作为后端的。

### 镜像管理

在生成web、应用、数据库虚拟机实例的时候，是有很多的工具和选项可以选择的，如此多的配置项确实佐证了虚拟化的灵活性，但是如果真的去每次都配置一遍的话，会浪费很多宝贵的时间的。那么镜像是可以做一些预先配置好的工作的，其中文件、软件包、补丁等统统可以做成镜像。预配置可以节省实例的构建时间，但是并不能包含自身存在的问题，比如补丁或者是许可证的更新。对于我们本文中所用到的例子来说，利用Heat来做编排，可以很好的弥补镜像的不足之处。

### 持久存储

和我们现实中的硬盘是一个道理，Cinder卷就是一块持久化的块存储虚拟设备，可以挂接到虚拟机上，然后由虚拟机操作系统来进行mount和umount操作，Cinder卷每次只能挂接到一台实例，本文中所创建和挂接的Cinder卷是给数据库虚拟机实例所使用的，用来满足数据库层的数据持久化的需求。假如发生了数据库虚拟机宕机的情况，一个新的虚拟机再立即创建，将原来的Cinder卷挂接到此新的虚拟机上即可。


### 网络子网

对于本文所用到的应用来讲，Neutron 被用来创建多个子网，每一层一个子网：web子网、应用子网、数据子网。Neutron的路由会在各个子网间创建相应的路由器。

### 网络安全

通过使用安全组可以过滤来自外部的流量，可以创建不同的安全组，以应用于不同层的实例，从而实现过滤掉不需要的网络流量。OpenStack安全组可以制定进出网络的许可和禁止多种规则，可以基于某个协议、端口，又或者是IP地址，又或者IP范围。每个实例可以应用多个或单个的安全组，所有的OpenStack项目都会有一个默认的安全组，假如用户没有创建自己的安全组的话，实例默认就会使用默认的安全组，除非作出改变，默认的安全组是禁止所有访问的。

### 编排

Heat 使用模板来自动化的部署复杂的云应用程序及其环境。编排的意义并不仅仅是启动一台虚拟机，它还可以安装软件、应用补丁、配置网络和安全等等，Heat模板所提供的能力和参考架构可以让用户快速的、完全自动化的配置和设置此LAMPweb服务环境。

### 自动伸缩

横向的扩展能力是云计算优势最为明显的一个功能，通过联合使用Heat和Ceilometer，OpenStack云平台可以实现自动启动额外的web和应用层虚拟机，即某些阀值被触发了之后，Ceilometer扮演系统资源的监控，也能够实现阀值的触发警报，Heat则响应警报所触发的扩展的规则。当然，扩展也是支持自动的缩减，当需求减少时关闭一些资源，从而节省开销。

## 演示和实例代码

本章描述了针对web应用栈Heat模板，为了更好的解释本文所创建的，这里友好提醒一下，未经修改可能在你的生产环境中并不能直接使用。

以下的Heat模板演示了如何配置和部署WordPress，众所周知，WordPress是一个典型的LAMP应用，三层架构的web应用。主要的模板有两个版本：其中一个是创建了一个静态的环境，（需手动的进行扩展）另外一个则是整合了Ceilometer，能够做到基于CPU 负载来动态的伸缩。

Heat模板的源代码，可以从[这里](http://www.openstack.org/software/sample-configs#web-applications)下载到。

<dl>
<table style="width:100%">
  <tr>
    <th>对应的层</th>
    <th>功能</th>
    <th>细节</th>
  </tr>
  <tr>
    <td>Web层</td>
    <td>反向代理服务</td>
    <td>Apache + mod_proxy</td>
  </tr>
  <tr>
    <td>应用层</td>
    <td>WordPress 服务</td>
    <td>Apache, PHP, MySQL Client, WordPress</td>
  </tr>
  <tr>
    <td>数据库层</td>
    <td>数据库服务</td>
    <td>MySQL</td>
  </tr>
</table>
</dl>

### Heat 文件细节

Heat模板使用嵌套的结构，对于本文中用到的例子中，两个完全不同的yaml文件，共用了同样的4个嵌套的文件。我们在文件中添加了注释，也有在配置环境时需要注意的事项。模板在OpenStack的Mitaka版本测试通过，操作系统分别支持Ubuntu Server 14.04 和 CentOS 7。

**WebAppStatic.yaml:** 此模板文件是为静态环境而写就的，它会创建一个拥有两台负载均衡的web服务、两台应用服务器、以及一台数据库服务器，其中数据库服务器会挂接Cinder服务抛出的块存储。此yaml文件用到的Heat资源组会调用 heat_app_tier.yaml 和 heat_web_tier.yaml，会启动多个web和应用服务。
**WebAppAutoScaling.yaml:** 对于动态自动伸缩的环境，运行此yaml文件。它同时为web和应用服务设置了Heat自动伸缩组以及Ceilometer告警，其中Ceilometer的告警配置了high-CPU，当CPU的平均负载超过50%，并持续5分钟时，会自动增加一个实例。当CPU利用率低于 20% 的时候，low CPU 告警会去自动缩减实例。当配置Ceilometer CPU告警的时候，千万要谨记一点，告警默认是在同一个OpenStack项目或租户中寻找的，一些元数据的使用，如创建独一无二的节点组标识，然后只有警报触发时的平均CPU利用率超过阈值。Ceilometer 不会去查找每单个实例的CPU利用率的；仅仅只会关心已经报告的CPU平均值。另外需要特别注意的一点：确保监控节点所选择的“间隔”要大于 ```/etc/ceilometer/pipeline.config```配置文件中的采样率，如果采样率高于这个间隔，告警将永远不会被触发。

以下yaml文件会被上述两个主要的文件所调用：

 * **setup_net_sg.yaml:** 此文件是主模板首先调用的。会创建三个相互隔离的私有网络，还会创建两个负载均衡器（使用Neutron LBaaS V1.0），其中一个是使用共有IP，将公有网络和web层的私有网络连接起来。另外一个是私有IP，将web层网络和应用层网络连接起来。此模板也会创建一个路由将应用网络和数据库网络连接起来，出网络和路由之外，还会为每一层创建一个安全组。
 * **heat_web_tier.yaml:** 此模板是启动web层节点的描述，除启动实例之外，它还会安装和配置Apache 和 Apache modproxy，其中modproxy是用于重定向流量到应用程序节点的。
 * **heat_app_tier.yaml:** 此模板是启动应用层节点的描述，除启动实例之外，它还会安装和配置Apache、PHP、MySQL 客户端、以及所必须的WordPress。
 * **heat_sql_tier.yaml:** 此模板是启动数据库层节点的描述，它也会创建一个Cinder的块设备，用来存放数据库文件，还会创建WordPress需要的用户、数据库。

## 范围和假设

上面所描述的Heat模板所假设的场景是此三层的web应用是部署在单一的region、单一的zone下的，如果是实际的应用环境的话，比如需要更高的SLA，建议部署的OpenStack环境是多region、多zone下的，这也是我们没有在本章所讨论的。

正如开始我们所提示的，这次实现并没有用到Trove项目，Trove是OpenStack的DBaaS，其可以提供关系型和非关系型数据库的引擎。我们也正在考虑整合Trove的详细方案。

另外还有一个服务是可以用在三层web应用架构中的，那就是Neutron的防火墙即服务（FWaaS），FWaaS在Neutron路由的这一层起作用，当然，整合FWaaS也在我们的考虑范围之内。

## 总结

当然，“条条大路通罗马”，对于实现三层web应用有很多种部署策略，即使是OpenStack的部署中也有很多的选择。本文所做的架构参考，尽可能的是实现了一个通用的，基于LAMP技术栈的，并通过组合OpenStack的服务来实现，其中服务包括核心服务也包括可选服务。用到了Heat编排服务，然后，Heat是可以使用很多流行的工具替代的，例如Chef、Puppet、Ansible等。

<dl>
<table style="width:100%">
  <tr>
    <th>在线资源</th>
    <th>概要介绍</th>
  </tr>
  <tr>
    <td><a herf="http://www.openstack.org/marketplace">OpenStack Marketplace</a></td>
    <td>来自全球的发行版、驱动厂商、培训、服务等提供的一站式服务。</td>
  </tr>
  <tr>
    <td><a herf="http://docs.openstack.org/arch-design/">OpenStack 架构设计指南</a></td>
    <td>这是一本指南，针对常见的用户场景，该如何设计OpenStack云平台的架构。而且有一些实例。</td>
  </tr>
  <tr>
    <td><a herf="http://docs.openstack.org/mitaka/networking-guide/">OpenStack 网络指南</a></td>
    <td>一本描写如何部署和管理OpenStack网络的书，（Neutron）。</td>
  </tr>
  <tr>
    <td><a herf="http://docs.openstack.org/security-guide/">OpenStack 安全指南</a></td>
    <td>关于OpenStack云平台的最佳实践和概念信息。</td>
  </tr>
  <tr>
    <td><a herf="http://docs.openstack.org/ha-guide/">OpenStack 高可用指南</a></td>
    <td>安装和配置 OpenStack 的高可用。</td>
  </tr>
  <tr>
    <td><a herf="http://docs.openstack.org/">完整的OpenStack文档</a></td>
    <td>所有的OpenStack有史以来全部文档，无论读者是何种角色，都能找到对应的文档来学习和参考。</td>
  </tr>
  <tr>
    <td><a herf="http://apps.openstack.org/">社区应用程序目录</a></td>
    <td>本文所使用的LAMP／WordPress就是在这里下载的，当然这里还有很多的应用程序。</td>
  </tr>
  <tr>
    <td><a herf="http://www.openstack.org/community/">欢迎加入OpenStack社区</a></td>
    <td>加入邮件列表和IRC频道，寻找工作和相关事宜，访问源码，等等。</td>
  </tr>
  <tr>
    <td><a herf="https://groups.openstack.org/">用户组</a></td>
    <td>寻找离你最近的用户组，参与当地的Meetup、HackHackathon，或者自己组织一个？</td>
  </tr>
  <tr>
    <td><a herf="http://www.openstack.org/community/events/">OpenStack事件</a></td>
    <td>大名鼎鼎的OpenStack峰会和当地的 OpenStack Day 都在这里可以找到。</td>
  </tr>
</table>
</dl>
