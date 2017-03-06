
来自中油瑞飞的需求问题汇总：

我们目前的现状上次和您沟通过了，针对我们现在自动化程度不够（传统SAN网络和TRILL网络），成本较高的情况。我们未来计划引入OpenStack的Nova、Cinder、Neutron、Ironic项目，同时需要相关glance、keystone、ceilometer项目。在存储方面计划引入Ceph保存冷数据和备份数据
希望能您能给我们指导的主题主要包括：
1. 社区版OpenStack是否具有用于企业私有云的生产环境的条件。根据我们目前选择的项目，需要有哪些完善工作和解决方案才能用于生产环境
2. 目前的监控利用IBM Tivoli，在云环境海量监控对象和指标的场景下性能出现瓶颈，计划利用Zabbix、Calamari和ceilometer对OpenStack和Ceph环境进行监控，这样的方案是否可行，UMCloud建议的监控解决方案是什么
3. 目前我们环境对物理机的管控力度较差，如何利用Ironic和Nova、VMware、容器等相结合，实现计算资源全流程管控的自动化
4. 网络方面Neutron+OVS的Host Overlay方案在可靠性和性能上是否能够满足生产环境运行需要，是否仍需要Network Overlay（华为AC、H3C VCFC等）。UMCloud的网络解决方案是什么
5. 考虑到Horizon的功能、效率较差，企业私有云环境又往往有特定的运营/运维需求，云管理平台CMP实现方法和思路是怎样的
6. 目前中石油环境中存在大量异构存储，包括很多高端、终端SAN存储，这些存储都在Cinder的支持列表中，利用Cinder对这些存储进行管控是否可行，是否有成功案例。
7. 关于OpenStack HA，UMCloud建议的解决方案是什么，尤其是Neutron的L3的HA，VRRP和VDR是否可以应用到生产环境，Cinder Volume组件是否有A/A的方案，计算节点(虚机)的高可用是如何考虑的。
8. 关于OpenStack部署，目前Mirantis号称已经重新设计OpenStack的部署系统Fuel，那样OpenStack可以作为一系列Docker容器，在极简安装的Linux系统上运行。这个项目和Kolla的关系是什么，有哪些优势，具体什么时间发布，今年下半年能否应用到生产环境。
9. 关于OpenStack升级，UMCloud有哪些实际的经过生产验证的经验，可否举几个案例给予分享。
10. 关于OpenStack和Ceph的容灾方案，UMCloud有哪些实际的经过生产验证的经验，可否举几个案例给予分享。
11. OpenStack与VMware的整合方案，有没有什么经验分享，架构以及优劣势等方面。
12. 贵公司在开发OpenStack主要采用什么样的开发环境，是否能提供一些开发环境用的工具之类的参考。
----------------
English translation
----------------
Our IT Environment is not automation enough,(Origin SAN network and TRILL  network),So we afford very high cost. So we want to launch OpenStack IaaS Platform, OpenStack core project, Nova,Cinder,glance,keystone,Neutron etc. about storage ,we will use Ceph project,use for store cold data and backup.

We need UMCloud/Mirantis support, not just blow:

1. Could Upstream OpenStack is ready for Enterprise Cloud platform production environment? Please tell me ,If we selected the above core and option project, What we need done can meet production environment?
2. We use IBM  Tivoli for monitor solution at present, as you know ,this can't meet Cloud environment , it's performance have bottleneck,Next step, we plan to use Zabbix，Calamari and ceilometer open source project, try to monitor Ceph distributed storage and OpenStack cloud platform, is this possible? What's the suggestions from umcloud/Mirantis?
3. At present, Manage physical machine is coarse-grained，How to used Ironic,Nova, VMware, Container and their combination，implementation all Computer resource automation?
4. OpenStack network solution: Neutron + OVS ,Host overlay ,could this meet production environment in performance and reliability？ Does need network overlay (like HUAWEI AC ,H3C VCFC etc.)? Which network solution is umcloud/Mirantis can provide？and suggestions?
5. Horizon have poor function and inefficient, Enterprise private cloud have special needs for Ops，What can we think about CMP（cloud management platform）?
6. CNPC had a large number of heterogeneous storage, Include high/middle SAN product,We have checked Cinder supported driver,Can we use Cinder for manage that? Do you known Whether or not successful use case?
7. What is solution about OpenStack High available? Especially Neutron L3. Can Neutron's VRRP and VDP used for production environment? Did Cinder Volume have A/A(active/active) solution? What about computer node (instance) high available?
8. As we know, Mirantis redesign Fuel deploy system, as a result , all OpenStack component will run in Container(Docker) that not need general Linux Operation System. Our question is: What is relationships between project(Fuel CCP) and Kolla? What is develop planning? and roadmap? Can use in production environment this third and fourth quarter？
9. About OpenStack Upgrade, What is experience in project of UMCloud/Mirantis? Could you share with us your use cases?
10. Does UMCloud/Mirantis have experience of disaster and restore solution ? OpenStack and Ceph, Share something with us.
11. What's provide solution OpenStack and VMWare integration, Did have a number of use case? architecture design advantage compare with others.
12. When you develop OpenStack,Which develop environment used? could you share your environment, just like tools, test cases,CI,CD etc.
