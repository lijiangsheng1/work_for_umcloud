# 在 Mirantis OpenStack 中运行 RHEL 计算节点

摘要：

Mirantis OpenStack 标榜自己是100%的纯粹开源的解决方案提供商，那么是怎么对待如红帽这样的竞争对手了呢？答案是100%的提供兼容支持。本文要阐释的就是如何在 Mirantis OpenStack 中运行 RHEL 的计算节点具体步骤。依照本文，用户可以轻松的部署REHL 到 Mirantis OpenStack 中。
_________

OpenStack 的其中一个“杀手级的功能”，就是能够在一个单一的控制面板下掌控多种类型的资源。举例来说，同样一组 OpenStack 控制器可以轻松管理多种类型的 hypervisor，诸如 KVM、vSphere、和 XenServer，从而能够让云计算的用户根据最佳组合来部署计算资源的负载。

近来常常听到的“异质云”就是 KVM hypervisor 混合 vSphere 的集群。此配置能够让云的所有者解决一系列的难题——支持“没有为云准备好的应用”（需要基础设施支持高可用）。[Mirantis OpenStack](https://www.mirantis.com/products/mirantis-openstack-software/?utm_source=wwwmirantiscom&utm_medium=text&utm_campaign=MOS%20weight%20increasing)从7.0 开始就支持了这种类型的配置。

我们最近遇到了多个云平台所有者的难题——支持RHEL认证过的应用负载。使用带有 KVM hypervisor 的RHEL 运行虚拟化的应用负载，在现在的企业环境中是比较常见的，很多应用程序的供应商都对RHEL的KVM进行了认证，以获得“支持的配置”在虚拟机中运行他们的产品。**简单来说，这就意味着如果作为用户的你是基于KVM的，而并非RHEL 的KVM的话，就是“不被支持的配置”，你就不可能从供应商哪里得到支持**。你破坏了合规性的要求，等等。

这引起了我们的思考：我们该如何为Mirantis OpenStack的用户解决此一问题？我们在 Ubuntu 下运行控制器，也将计算节点部署到Ubuntu下，而且还能够让用户轻松的将Ubuntu计算节点整合vSphere或Kubernetes——但是我们可以将RHEL也纳入吗？

事实证明,我们能够做得到。本文将会深入介绍这一切在 Mirantis OpenStack 8.0中是如何实现的。

## 在一个控制面板下的RHEL和Ubuntu KVM混合环境

站在一个很高的高度上来观看的话，RHEL 计算 v1 版本能够让云的所有者做下面两件事情：

1. 使用Fuel来部署MOS 云平台，运行Ubuntu 主机的有所有的控制节点和Ceph和存储节点。
2. 整合RHEL的节点为计算节点，使用MOS的控制器来管理。（注意：RHEL的节点必须是**预先安装好的**，整合到MOS中使用最为基本的安装即可。）

在整合 RHEL 计算节点的过程中，也就是发布版本v1当中，我们做了如下一些事情：

1. 针对计算节点，将Mirantis OpenStack 的软件打包成RPM，让其轻松部署在 RHEL 中（此处仅限于RHEL仓库中所没有的OpenStack 组件及相关依赖，RHEL原生的一些基础包是不动的，如KVM、libvirt等。）
2. 修改由[Fuel-library](https://github.com/openstack/fuel-library)所交付的 Puppet manifests来支持将 Mirantis OpenStack 安装到 RHEL 中。（Fuel－library 是基于上游的[OpenStack Puppet项目](https://wiki.openstack.org/wiki/Puppet)）
3. 开发了[解决方案指南](https://docs.mirantis.com/openstack/fuel/fuel-master/rhel.html#introduction)，来为上述到2个步骤提供必要的向导。

**在[解决方案指南](https://docs.mirantis.com/openstack/fuel/fuel-master/rhel.html#introduction)对整个过程做个非常详细的介绍**，本文是对整合的技术作了更高层次的概述。

## 将RHEL计算节点运行在 MOS 之下的 7个步骤

那么，前提条件是这样子的：我们已经拥有了运行 MOS 的云环境，其中有控制节点、有一些基于 Ceph 的存储节点、甚至还有一些基于 Ubuntu 的计算节点。然后，让我们开始将一些预装了 RHEL 节点的机器加入到 MOS 云平台中！

我们需要以下7步就可以实现目的：

* **步骤 0**：仔细到[限制和支持项配置](https://docs.mirantis.com/openstack/fuel/fuel-master/rhel.html#rhel-limitations)页面检查一遍，以确保能够接受 MOS 整合 RHEL。
* **步骤 1**：准备好安装有 RHEL 的服务器，至于具体怎么装，比如使用 foreman。
* **步骤 2**：验证 RHEL 服务器的配置，确保拥有足够的磁盘空间，正确的分区表、网卡、可访问的仓库、等等。
* **步骤 3**：为 MOS 的部署，对RHEL做些准备。
* **步骤 4**：在 RHEL 节点上配置 SELinux。
* **步骤 5**：在 RHEL 节点上部署 MOS 组件。
* **步骤 6**：验证部署后的 RHEL 计算节点。
* **步骤 7**：为了虚拟机更加的容易调度，将 RHEL 节点加入到可用域中或者是主机聚合。

那么接下来就让我们再更进一步的看看每个具体的步骤都做了些什么。

## 验证 RHEL 服务器的配置

这里我们为了进一步的确保 RHEL 服务器能够整合到 MOS 中，完成计算服务的任务，请仔细检查下面表中各项内容：

| 需求 | 描述 | 
| ------------ | ------------- | 
| 磁盘分区 | 如果是分了多个分区的话：根分区至少要有10GB的空间，```/var/log```要留有至少10GB的磁盘空间，```/var/log/nova```使用其余的磁盘空间，至少要分配30GB；如果是单个分区的话：至少要有50GB。 | 
| 网络 | 满足 Fuel 环境所要求的网络配置即可，Fuel管理（PXE）网络不是必须的。 | 
| 域名解析 | RHEL 节点必须能够解析域名。 | 
| RHEL 订阅 | RHEL 计算节点必须拥有合法的红帽订阅。 | 
| 访问 Mirantis OpenStack 仓库 |RHEL 计算节点必须能够访问Mirantis OpenStack软件仓库，或通过互联网访问Mirantis官方地址，或在本公司内网中搭建了本地的仓库镜像。| 

关于上面没一项中需要执行的命令行工具，请参考[解决方案指南](https://docs.mirantis.com/openstack/fuel/fuel-master/rhel.html#introduction),获得更多细节。

## 准备好将 RHEL 部署到 MOS 环境中

一旦用户您将红帽企业级 Linux 安装到您的服务器当中，仅需要简单的几个步骤就可以准备好将之加入到 MOS的部署中，您可以从[解决方案指南](https://docs.mirantis.com/openstack/fuel/fuel-master/rhel.html#introduction)获得详细的操作，我们这里提供一个概要：

1. 可以通过公钥访问到系统中，可以是root用户，也可以是具有执行sudo的用户。
2. 为RHEL配置添加mos-8.0的仓库地址，并倒入仓库密钥。
3. 启用额外的红帽仓库，以安装OpenStack的相关依赖。
4. 为虚拟机的迁移生成公钥。
5. 拷贝 Ceph 的 ssh 密钥到 RHEL 计算节点。（如果您使用Ceph的话）
6. 安装 Puppet 3.x 和 Ruby 2.1.x
7. 验证 KVM 模块启用所需的依赖。
8. 从 Mirantis 仓库安装 fuel-library8.0 （这里就是有着神奇魔力的puppet），使用下面命令来安装：
```
yum install fuel-library8.0 -y

```
[解决方案指南](https://docs.mirantis.com/openstack/fuel/fuel-master/rhel.html#introduction)中涵盖了每个步骤的命令细节。

接下来，需要做的就是配置 SELinux。

## 为 RHEL 节点配置 SELinux

安全增强Linux，是一种Linux 内核的安全机制，其实现了强制访问控制（MAC）。因为 RHEL 的默认配置是将SELinux 以"enforcing"模式开启的，所以我们需要配置SELinux，不让它将OpenStack的服务给屏蔽掉。否则的话，你的系统将没法工作。

我们可以将SELinux配置为下面三种模式的其中一种：

* **Custom permissive** - 此模式是所有OpenStack环境的首选。SELinux 通过授权以及禁止不安全的操作来管理系统的安全。您必须配置SELinux，从而让OpenStack的服务能够被操作。
* **Permissive** - 启用 SELinux，且允许所有操作，但是所有的操作都会被记录到```/var/log/avc.log```中。
* **Disabled** - 关闭内核的SELinux，可以执行所有的操作。

请仔细阅读[解决方案指南](https://docs.mirantis.com/openstack/fuel/fuel-master/rhel.html#introduction)以获得配置 SELinux 的详细步骤。

## 在 MOS 中部署 RHEL 节点

现在，您已经准备好了 RHEL 节点了。是时间执行真正的部署操作了。这个过程有如下几个步骤。

### 配置 astute.yaml

要想让您的 RHEL 服务器加入到正式到计算节点角色中，您首先得编辑```astute.yaml```文件，Puppet manifests 都在 **fuel-library** 中包含了。默认情况下，云平台的配置参数均是从 astute 服务取得的，所以在 Puppet manifests 下达之前，请编辑 ```astute.yaml```来满足您的配置需求，其位于```/etc```目录下，如果您还要部署额外的组件，如：Sahara、Murano、或者是 Ceilometer，您还必须更新 ```astute.yaml```的网络部分，要赋予网络正确的角色和参数。

在[解决方案指南](https://docs.mirantis.com/openstack/fuel/fuel-master/rhel.html#configure-the-astute-yaml-file-parameters)中，列出了详细的 ```astute.yaml```的配置参数，以下是一个在不同云环境中已经实际应用的例子的配置：
 
 * [Neutron VXLAN 实例](https://review.fuel-infra.org/#/c/14377/19/pages/draft/rhel7-compute-guide/_examples/astute-tun-simple.yaml)
 * [基于Bonding的 Neutron VXLAN](https://review.fuel-infra.org/#/c/14377/19/pages/draft/rhel7-compute-guide/_examples/astute-vlan-bond.yaml)
 * [基于DVR的 Neutron VXLAN](https://review.fuel-infra.org/#/c/14377/19/pages/draft/rhel7-compute-guide/_examples/astute-tun-dvr.yaml)

### 在 RHEL 节点中应用 puppet manifests

RHEL 计算节点的部署流程，可以归结为在RHEL的节点上以特定的顺序运行 Puppet manifestes。在本例中，所有的 Puppet manifeses都可在目录/etc/puppet/modules下找到。

以下表格中所列出的puppet manifeses，是必须在RHEL计算节点中被接受：

| Puppet Manifests  | 描述 | 文件路径  |
| ------------ | ------------- | ------------ |
|hiera.pp  | 配置hiera软件包及其依赖  |/etc/puppet/modules/osnailyfacter/modular/hiera/hiera.pp  |
|globals.pp  | 为Puppet优化hiera 的配置结构  | /etc/puppet/modules/osnailyfacter/modular/globals/globals.pp  |
| firewall.pp | 配置防火墙，以接受来自OpenStack组件的连接  | /etc/puppet/modules/osnailyfacter/modular/firewall/firewall.pp |
|tools.pp  | 用于调试和部署而添加如下工具：man、atop、tmux、screen、tcpdump、strace  | /etc/puppet/modules/osnailyfacter/modular/tools/tools.pp |
|netconfig.pp  | 配置网卡和网桥，根据astute.yaml文件中的指定的配置：network_metadata(incoming data) 、network_scheme、transformations  | /etc/puppet/modules/osnailyfacter/modular/netconfig/netconfig.pp |
| /roles/compute.pp | 为Nova计算节点安装必要的软件包，为nova-compute服务配置librirtd。  |/etc/puppet/modules/osnailyfacter/modular/roles/compute.pp  |
|common-config.pp  | 安装和配置Neutron需要的软件包。  | /etc/puppet/modules/osnailyfacter/modular/openstack-network/common-config.pp |
|ml2.pp  |  配置 Neutron ML2 插件和服务 | /etc/puppet/modules/osnailyfacter/modular/openstack-network/plugins/ml2.pp |
| l3.pp |（可选）如果你使用了Neutron的DVR，此 manifests会在 RHEL 计算节点上配置Neutron L3 代理。另外，不要接受此Minifests。   | /etc/puppet/modules/osnailyfacter/modular/openstack-network/agents/l3.pp |
|metadata.pp  |  （可选）如果你启用了Neutron的DVR。要在RHEL计算节点配置Neutron原数据代理。另外，不要接受此Minifests。 | /etc/puppet/modules/osnailyfacter/modular/openstack-network/agents/metadata.pp |
| compute-nova.pp | 让Nova和Neutron接受通用的配置，启动nova－compute服务。  |/etc/puppet/modules/osnailyfacter/modular/openstack-network/compute-nova.pp  |
| enable_compute.pp | 配置nova-computer在开机时启动。  | /etc/puppet/modules/osnailyfacter/modular/astute/enable_compute.pp |
| /ceilometer/compute.pp |（可选）如果你部署了Ceilometer，在RHEL计算节点上接受Ceilometer的配置，另外，不要接受此Minifests。   | /etc/puppet/modules/osnailyfacter/modular/ceilometer/compute.pp |
|/ceph/ceph_compute.pp  |（可选）如果你部署了Ceph，在RHEL计算节点接受Ceph 的配置。另外，不要接受此Minifests。   | /etc/puppet/modules/osnailyfacter/modular/ceph/ceph_compute.pp |

请参考[解决方案指南](https://docs.mirantis.com/openstack/fuel/fuel-master/rhel.html#deploy-the-rhel-compute-nodes)，列出了特殊的命令行，对于在RHEL节点上配置OpenStack 服务和接受 manifests 非常的有帮助。

## 验证部署好的 RHEL 节点

现在，您已经完成了部署，所以是时候该检验一下刚刚安装好的 RHEL节点是否是正常工作的了。因为 Fuel 不能做到管理 RHEL 计算节点，所以这些节点也没法使用健康检查这项 Fuel 的功能。不过，我们针对下面场景为您提供了一些工具来进行验证：（实际的操作使用，请参考[解决方案指南](https://docs.mirantis.com/openstack/fuel/fuel-master/rhel.html#introduction)）

| 验证 | 需求 | 
| ------------ | ------------- | 
|验证OpenStack的服务 | * 在控制器节点中查看：RHEL 计算节点是出于运行状态，且在 Nova hypervisor 列表中是启用的。 * RHEL 计算节点上的 nova-compute 服务是启动着的，且在 Nova 服务列表中是启用的。 * 在 neutron 代理列表中 OVS agent 的状态是 ```alive```  | 
|验证在 RHEL 计算节点上启动虚拟机实例 | 在启动之后，命令 ```nova list```所返回的信息中对于刚刚创建的实例必须是：＊状态栏必须是```ACTIVE```＊ 电源栏必须是```Running```. 命令```nova show```所返回的实例所在主机信息必须有：＊ OS-EXT-SRV-ATTR:host ＊OS-EXT-SRV-ATTR:hypervisor_hostname  | 
| 验证在不分配浮动IP的情况下虚拟机实例的网络互通| 执行下面的动作是成功的：＊ ping 实例的 internal IP 地址。＊在控制节点和 internal IP地址的实例之间建立TCP连接。  | 
|验证在分配浮动IP的情况下虚拟机实例的网络互通 | 执行下面的动作是成功的：＊ 实例的floating ip是可以ping通的。＊在控制节点和 floating IP地址的实例之间建立TCP连接。 ＊在实例中可以访问互联网。  | 
|验证实例可以访问元数据 |实例必须能够获取到元数据，如公钥。   | 

## 根据负载调度分离 RHEL 节点

您现在终于有机会展现一下了，您刚刚将 RHEL 的节点加入到 MOS 云平台当中，可以将它们供用户使用了，从而实现用户运行负载到RHEL之上。主机聚合和可用域均是 OpenStack 为不同负载提供不同的访问方式的方式，[解决方案指南](https://docs.mirantis.com/openstack/fuel/fuel-master/rhel.html#segregate-the-rhel-compute-nodes-from-the-ubuntu-nodes)给出了一些实例，来告诉您如何去使用它们。

## 接下来做什么？ 

现在我们成功的实现了在 MOS 环境中运行 RHEL 计算节点，这也就意味着可以管理多个基于KVM hypervisor 的类型，有的来自 Ubuntu、有的来自 RHEL。这引起了我们更进一步的思考，未来方向有：

  * 通过 Fuel 来实现，将非Ubuntu的计算节点部署到MOS环境中，且是自动化的集成流程。
  * 引入支持额外的计算主机操作系统选项：比如Oracle Linux, CentOS, openSuSE等。

如果此文引起了您的极大兴趣，请随时查看我们[完整的文档](https://docs.mirantis.com/openstack/fuel/fuel-master/rhel.html)。