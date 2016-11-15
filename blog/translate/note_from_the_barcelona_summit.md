# 巴塞罗纳峰会掠影：OpenStack 在科研机构的应用

历届 OpenStack 峰会，都会有一些科学家们站出来分享一下他们基于 OpenStack 做的事，将诸如大数据处理和分析等科学研究迁移到 OpenStack 中，这次的巴塞罗纳峰会也不例外。本次峰会上，学术界到参会者们可谓是大放异彩，在具有代表性的领域：核能物理、天文学、医学等项目的规模和价值上都有新的突破，在数据的存储和处理上我们有了完全新的级别，从PB（1,000,000 GB）上升到了ZB（1,000,000,000,000 GB），而且还用到了数十万颗 CPU core。如此之规模，是该严肃对待。

首先，我们来看下 Tim Bell 的演讲吧，Tim Bell 可谓是 OpenStack 峰会演讲的常客了，这次带来的是 CERN 在云基础设施方面的更新。他提到，在大型强子对撞机中，为了监视几十亿颗粒子的碰撞，科学家们每天获得的数据是0.5 PB，如此巨大的数据量需要使用超过[190，000 个core 的 OpenStack](https://www.youtube.com/watch?v=KPZXsPsD1hg)方能满足。

![](https://cdn.mirantis.com/wp-content/uploads/2016/11/timbell.png)

接下来是来自剑桥大学的 Rosie Bolton 博士，她向我们解释了她的团队是如何探索我们银河系和暗物质的起源，他们使用了巨大的软件定义无线电望远镜，称之为**Square Kilometer Array**，且它们是跨地域分布在南非和澳大利亚。[此望远镜每天会产生 1.3 ZB 的数据，且要保存 1 个PB的数据，](https://www.youtube.com/watch?v=xsbCPSp-X3Q)所有这些均是在 OpenStack 云平台中存储和处理的。

![](https://cdn.mirantis.com/wp-content/uploads/2016/11/RosieBolton.png?resize=1043x588)

还有同样是来自剑桥大学的 Paul Calleja 博士，他向我们介绍了研究人员是如何创建生物医学云平台的，从医院收集病人的数据，并存储和分析这些数据，希望得出新的医疗方案，举例来说，该项目开发了统计模型，能够在进行外科手术期间对病人的医疗数据进行实时的处理，能够有效的降低58%的手术部位的感染率。

他还介绍了另外一个称之为 OpenCB 的云平台的案例，OpenCB 使用的是 Hadoop 基础设施用于下一代数据分析，而这些是被英格兰的基因学会用来研究在英国的100,000人的基因组。目前，OpenCB 已经用于超过  10,000 位疾病患者的基因分析。

Paul Calleja 博士还额外的介绍了生物医学云平台是如何被用来计算和存储大脑扫描设备的数据的，详情可以观看他的[ Summit](https://www.youtube.com/watch?v=KPZXsPsD1hg)。

![](https://cdn.mirantis.com/wp-content/uploads/2016/11/Biomedical.png?resize=1046x588)

OpenStack 在学术界并不仅仅是用于研究的，而且还用于学生的学习，比如，在基于[OpenStack 的云平台上](https://www.mirantis.com/openstack-cloud/?utm_source=wwwmirantiscom&utm_medium=text&utm_campaign=OC%20weight%20increasing)做诸如网络、安全等的上手试验。一些大学机构使用OpenStack构建他们的超级计算中心，从而帮助教师和学生来完成他们的硕士和博士的研究课程。另外需要强调的一点是，OpenStack是开源的，对于学术机构有限的预算来说，不存在厂商锁定是非常重要的一个考虑因素。

在过去的这些年里，人们对于OpenStack在企业界的应用给予了太多的关注，但是，要记得，它最初是由 NASA 所设计的，这就是之所以 OpenStack 即使是在今天也这么受到学术界和科学界欢迎的缘故。
