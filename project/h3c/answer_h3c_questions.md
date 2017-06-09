
# UMCloud与H3C SDN控制器对接

1. 目前UMCloud支持哪几种虚拟化平台？从贵司官网上了解有“KVM、QEMU、VMWare”是否包括Xen虚拟化？
2. 针对KVM虚拟化平台，支持哪几种操作系统、哪几个版本？Ubuntu、CentOS、SUSE是否都支持？付费的RHEL是否也支持？
3. MOS 9.0升级9.2官网显示目前只支持Ubuntu，后续其他OS的计划是否方便透漏^^，方便我们安排对接工作的节奏
4. 据了解贵司与华为美国研究所有过对接，可否介绍一下详细的情况和资料？
5. 能否发一些UMCloud的特性，亮点等技术资料？
6. Mirantis官网上介绍了Mirantis Openstack一种部署方式，Host OS上安装虚拟机（只提到VirtualBox），在虚拟机内再部署UMCloud的方式，是否支持能在VMWare的ESXi系统上创建虚机来部署呢？
7. 能否提供UMCloud的离线安装包，官网上介绍的部署方式，Host+VirtualBox+Mirantis Openstack ISO部署过程中需要联网，我司的研发内网信息安全有一些限制，操作起来比较费时。给一个sftp下载也可以，感谢！
8. 如果暂时不能提供离线安装包，能否针对以下MOS镜像源开启SYNC同步功能（已确认没开启），这样我可以让IT同步到我司的本地镜像源？



## 作为一名工程师，正确的回答应该是这样子的

Miranits一直是以开放合作的态度与云计算上下游厂商建立合作伙伴与生态圈。我们欢迎H3C参与到其中，提供给我们的

客户更多的方案选择。Mirantis 现有的网络、存储、NFV等方面的合作伙伴插件列表：https://www.mirantis.com/software/fuel-plugins/， 厂商可以根据社区现有的插件开发SDK与MOS

进行插件开发对接和测试，同时Mirantis和UMCloud也提供插件开发和支持服务。插件开发指南：

http://docs.openstack.org/developer/fuel-docs/plugindocs/fuel-plugin-sdk-guide.html。

从技术原理上讲，作为SDN或网络设备厂商，应该直接在OpenStack进行自己的开发，如Neutron的driver的开发等，Mirantis 所开发的Fuel是为安装、部署准备的。Fuel的架构提供了插件的方式，将ISV的软件能够轻松、随意的安装到OpenStack环境中。所以开发者将自己写好的驱动，按照Fuel plugin 的规则打包就好了。

## 作为狗血的商业行为，却要一个一个的单独回复
> 你就含糊其辞的给他们一个说法就可以了，如果有进一步的工作，则推给培训部即可。  --> 售前技术总监宁鹏

1. UMCloud销售的产品是著名OpenStack大厂Mirantis OpenStack 商业发行版，以下简称MOS（下同），MOS不仅支持主流的开源Hyperviosr项目，也支持商业的Hypervisor项目，也就是说不仅支持开源项目Xen，也支持Citrix XEN Server。更多详情请访问：[Mirantis官方网站](https://www.mirantis.com/partners/citrix/)
2. 这个问题有一些歧义，按照Fule 的架构来讲，Slave端操作系统仅支持Ubuntu，版本为14.04。若是KVM Hypervisor的Guest 操作系统，全线兼容，可访问KVM站点操作系统列表。
3. MOS 的Slave端操作系统，默认仅支持Ubuntu，可通过手动部署RHEL。
4. 和华为的对接过程是正常的Mirantis Fuel插件计划：可参考：[插件开发指南](http://docs.openstack.org/developer/fuel-docs/plugindocs/fuel-plugin-sdk-guide.html)
5. Mirantis OpenStack 产品资料，可参考官方网站：[Mirantis OpenStack](https://www.mirantis.com/software/openstack/)。
6. Mirantis 为潜在客户提供的VirtualBox 脚本，仅供客户测试使用。同样，VMWare虚拟环境下亦可部署Mirantis OpenStack，具体方式可咨询UMCloud工程师。
7. Mirantis 的离线安装包有接近4G，当下并没有上传到互联网上。需和相关的人员进行线下拷贝。
8. 关于创建本地的Fuel镜像站点，可以参考Fuel社区的相关规定。Mirantis OpenStack 不会提供相应的镜像。
