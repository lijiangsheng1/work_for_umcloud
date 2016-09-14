# 沟通交流作业题

1. 公司组织架构和各部门人力情况

2. 公司发展战略

3. 公司成功案例

4. 对象存储适用场景及技术选型

非机构化数据，如图片、音频、视频、文档等，用于 write-once-read-more 的场景，且使用http方式来访问。

作为Ceph的支持者，Mirantis 推荐 开源SDS分布式存储系统——Ceph。

S3 的生态是蛮好的，备份虚拟机效果很好！

硬件堆叠＋好的架构 ＝ 最佳性能 

视频流和小文件做过很多优化：参数调整，（冷数据）

看用户是如何思考和选择的？产品 OR 非锁定。

影像、票据是对象存储的最佳适用场景。

传统的存储相对Ceph的TCO要便宜？

S3的需求越来越迫切。

5. 对象存储和磁带备份的比较

Tape is	lower in cost but cannot provide immediate access for recovery.

唯一的就是应用可能要改动，毕竟过去的应用、备份都是基于磁带写的。但是对象存储的优势实在是过于诱人：

* 基于地理位置的冗余
* 可以索引、支持搜索
* 媒体／内容管理
* 标准API访问
* 可移植性
* Ongoing re-indexing
* Compliance 


6. 开源战略，定制研发部分如何处理？是否通过接口兼容性认证

我们并不建议定制，Mirantis 是上游优先，如果需求能够获得每个版本的BP通过，Mirantis有实力去完成。

两方面来理解： 

a、接口兼容性： V1、V2、V3 一般会两个同时支持。（RESTful style）

b、兼容性认证： 我们保持和开源版本的一致。

7. 装一套OpenStack需要多久？（带控制节点高可用的）

前提条件准备好的话：带外管理、硬件配置、网络配置、存储配置等

安装 Fuel ＋ OpenStack 部署 5个小时左右。


8. OpenStack的已有的企业特性，是否具备虚拟机HA、备份能力，并介绍技术原理

播放一个奥斯丁的说明：[OpenStack VM ha](http://aspiers.github.io/openstack-summit-2016-austin-compute-ha/#/about)

首先更正一下，OpenStack 需要 VM 的高可用嘛？

准确一点说，是传统应用。

Do we really need compute HA in OpenStack?

有简单的实现：nova evacuate


9. 关于标准化和自动化运维相关思路

无论是Fuel还是OpenStack 本身，都提供良好的接口，以及各种语言（java、python）的SDK，

或者是使用诸如CloudScaling的混合云管理平台来实现。或者是沃尔玛开源的OneOps平台。

10. 关于云服务目录的建设思路

Murano，Mirantis OpenStack 的强项，将用户现有的应用堆栈迁移到OpenStack中，举个例子：Linux ＋ Apache ＋ MySQL ＋ PHP ，一键部署。或者是 Java中间件＋ Apache 一键部署。提前做好模版。


11. 监控相关思路

使用Mirantis提供的 StackLight，或者是Zabbix，

或者是整合用户自己的平台。看用户自己的思路。

12. KVM和操作系统等底层能力如何

KVM 是目前开源的主流硬件虚拟化 Hypervisor，OS 默认使用的Ubuntu，也可支持诸如RHEL、SUSE等。

KVM 本身是硬件扩展等虚拟化，Intel／AMD 在芯片上支持很好，Virtio技术等成熟，I/O的性能接近原生。

Ubuntu 事实证明是非常稳定、可靠的企业级操作系统之一。

## 需要了解的几个问题

**Q: 对 OpenStack 的认识处于何种阶段？**
> A: 原来使用过国内 UStack 的产品。

**Q: 为什么想利用 IaaS 云计算平台？**
> A:

**Q: 为什么不考虑使用公有云？**
> A:

**Q: 想利用 OpenStack 解决什么问题 ？迫切程度如何？**
> A:

**Q: 对于开源的开发模式、商业模式有何独特的看法？**
> A:

～～～～～～～～～～～～～～～～～～～～～

**Q: 邮储、深交所是否提出了很多的定制需求？你们是怎么处理的？ **
> A: 是否列入下个版本的RoadMap。 所谓的企业级特性是什么？虚拟机HA、虚拟机备份、虚拟机QoS。

**Q: 用户提到使用Ironic，遇到的网络和存储的问题。**
> A: Fuel的agent 和 Ironic的agent

**Q:国际大厂都是24小时连轴转，那么UMCloud是如何打算计划的？**
> A:

**Q:我们对美国的 Mirantis 有多少了解？**
> A: 确实是一个很大的问题。

用户事先调查过，对UMCloud的弱点很是了解和担心。


**Q: UCloud 和 UMCloud 的关系如何？**
> A:


 全国有1200多家银行，除几大国有银行之外，民营银行有很多。
 
 打算做成金融行业的“金融云”。
 
 多租户，VxLAN，烟台、北京、上海 6个DC，烟台1500节点，6选3。
 
 ~~主要是沟通的问题，正式的会议室，能否让人真正的放松下来？真正暴露自己的问题还是会有所忌惮的。~~
 
 思科 VTS，
 
 美国的团队和中国的团队之间的合作问题是XX比较关心的。我们现在正在解决这些问题。
 
 国内的研发团队规模似乎是更加关心的，打算发展为多大？
 
 XX非常关注UMCloud 的长远规划，Roadmap，

 