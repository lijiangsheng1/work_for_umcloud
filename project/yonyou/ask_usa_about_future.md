
# 用友问Mirantis的中文问题：

1、开源云计算技术的技术趋势，Mirantis如何看待OpenStack、Ceph、OpenContrail、k8s等项目的发展前景，能否展示一下Mirantis在这些项目上的投入和技术实力？

2、在美国，SaaS类的公司会否选择私有云，他们是如何构建自己的云基础设施的，能否举一个案例进行讲解？

3、容器是非常热门的技术方向，Mirantis如何看待容器与OpenStack的关系，为容器做了哪些准备和投入？

4、现在的openstack Neutron，在很多集成项目中变成了一个框架，让后端的各种插件自己来实现各种网络的服务（cisco，juniper等），一方面是在分布式模型下的新的网络功能实现，以及解决与已有功能的冲突；另一方面仍然是各个厂商以插件的形式支持自己的网络方案，这两种方式终究是冲突的，还是会最终统一

5、openstack trove对于关系型、非关系型数据库：RDS for mysql 、 pgsql 、sqlserver的支持情况及国外实际案例、

mysql多租户监控告警支持情况，未来对Mysql5.7,8.0新特性的支持情况。

6、openstack 分布式存储是否可以适配mysql数据库及相关优缺点，以保证mysql数据零丢失、i/o性能、存储扩容需求。

# 我的中译英

1. As you know, the future is open source cloud computing technology. OpenStack，Ceph，OpenContrail，Kubernetes etc. all of that are great and popular open source projects, How Mirantis will do those projects? Please show Mirantis's capacity in community and invested.
2. Is CaaS provider selected private cloud solution in U.S.A market? If yes, How? have use case for some details?
3. Container is very hottest this summer, How Mirantis will deal with OpenStack, Container and those relationship？What's the planning of Container for Mirantis?
4. About Neutron Project, it's integrated lots of projects, and developed to be a framework, in backend, Provider (Cisco,Juniper ..) can used it's plugin mechanism, this bring new problem:0.  Implement network new feature in new distributed environment, resolved conflict,1. Network provider implement it's own solution. My question is: how to deal with 0 and 1 conflict? or unified finally?
5. About Trove project: RDBMS,like MySQL、PGSQL、MS Server ，all of supported? any use case in America？ in the future it will support mysql multi-tenant monitor/alert? will support MySQL  5.7 and 8.0 's new feature?
6. distributed storage software like Ceph, can match MySQL RDBMS ?make sure Mysql Zero data lose, high i/o performance, capacity expansion scheme etc.  
