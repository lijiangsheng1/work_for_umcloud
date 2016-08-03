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



```
yum install fuel-library8.0 -y

```

## 为 RHEL 节点配置 SELinux

## 在 MOS 中部署 RHEL 节点

### 配置 astute.yaml

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

请参考[解决方案指南](https://docs.mirantis.com/openstack/fuel/fuel-master/rhel.html#deploy-the-rhel-compute-nodes)，列出了特殊的命令行，对于在RHEL节点上配置OpenStakck服务和接受manifests非常的有帮助。


## 验证部署好的 RHEL 节点

## 根据负载调度分离 RHEL 节点

## 接下来做什么？ 