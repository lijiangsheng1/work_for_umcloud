
## Q & A

**Q: 华大运用open stack 的场景和业务。邮件中提及支持bgionline, 我希望有更详细的阐述，说明为什么 open stack是支持BGI online的最好选择。**

**A: **
	
	1. HPC裸机管理：支持计算资源的裸机统一管理
	2. 应用平台：
	3. BGIonline：
	4. 大数据：一件部署大数据集群
	5. SGE集群各体系公共节点(研究院、科服、基因库、)
	6. 集群应用(Home，mysql，网络监控、硬件监控、DCM、计算资源监控、Docker需求、资产管理、数据中转、国外业务对接)
	7. 联合实验室(Proton，SEQ-500)
	8. 测试系统：

**Q:二. 买服务和花同样的经费雇用有openstack 专业知识的人才，自建团队的比较.**

**A:**

	1. 整体投资：
	2. 服务质量：OpenStack功能丰富，单靠小团队无法运维。
	3. 
	
**Q:三. Open stack 和其他云平台方案的技术比较.**

**A:**

	1. 开源
	2. 长远
	3. 自建团队

	

**Q: (重要)OpenStack是重量级的，google docker是轻量级的，同样能满足需求，至少运行的linux的应用全部可以docker上运行。**

** A: **

> 首先，需要澄清一下这其中涉及的一些概念，OpenStack是基础设施即服务平台项目、Docker是操作系统层虚拟化的容器技术、Kubernetes（Google Docker的理解）是基于容器技术的如Docker（还支持rocket,lxc）的编排工具。三者之间的涉及的技术并不是一个层次上的。

> 第二，他们不是相互替代的关系，而是相互补充的关系，正因为Linux容器技术，比如Docker，是运行在某个特定的Linux操作系统之上的，所以对于底层的资源调度仍然会用到基础设施即服务平台所提供的功能，这也是全球领先的公有云如AWS、GCE等都是在基础设施服务层之上架构的容器编排之精要所在。

> 要知道运行在Linux上的应用全部可以在Docker中运行，这句话没错，问题是为什么要运行在Docker中？Docker的优点在于其灵活性，能够进行版本控制，升级、扩展方面有先天的巨大优势。但是，也不是所有的应用都适合Docker运行，比如对持久性存储严重依赖的数据库就不适用。

> 再就是关于微服务架构，容器技术的出现为现在业界所认可的微服务架构所采纳，二者之间的契合程度也是浑然天成。但是不是所有的服务架构都是微服务的，比如企业常见的J2EE、LAMP等。