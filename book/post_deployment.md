
在成功部署了OpenStack云平台之后，你就需要为维护和升级采用一系列的流程以及过程。最佳实践就是OpenStack云平台能够如丝般顺滑的[发布版本](http://release.openstack.org)，而且要采取一些预防措施，即防止那些使用了不同版本问题的出现。版本的一致性是日常运维管理和自动化非常关键的衡量标准之一。

## 更新周期

OpenStack 标榜自己是开放的设计和开发流程，社区采用的是基于时间的发布策略，且要频繁的里程碑式的模式，即每6个月发布一个版本。在每个版本的规划阶段，社区会让开发者们举办[设计峰会](http://wiki.openstack.org/wiki/Design_Summit)，讨论形成共识之后，设定[下一个里程碑](http://www.openstack.org/software/roadmap)，作为用户的你应该积极的参与到每次的设计峰会，提出自己对于OpenStack的需求，当然要积极的参加[OpenStack Summit](http://www.openstack.org/summit)是必须的了，因为可以第一时间了解和评估新的特性。还有，在OpenStack峰会上，要去分享自己在设计和运维OpenStack云平台方面的经验，同时，因为大家都在分享，你也能够得到其他使用OpenStack同仁的分享。

如果作为用户的你，在构建OpenStack云平台时选择的是DIY方式的话，那么你可能需要在项目级别上获得OpenStack的版本信息。那么你就得决定选用OpenStack的那个分支，使用那些软件包和技术，这些我们都在前面的章节《选择应用程序》和《实现阶段》都有解释过。使用这些工具来为你的更新策略和执行提供支撑。更进一步讲，你还需要关注[项目导航](http://www.openstack.org/software/project-navigator)以及社区支持的[](https://wiki.openstack.org/wiki/ProductTeam/MultiRealse_Roadmap)。

如果你的OpenStack云平台使用的是厂商提供的发行版的话，那么你就需要密切关注发行版自己所发布的信息和版本周期了，发行版供应商通常在社区非常的活跃，你关注他们的在OpenStack社区的积极活动是有益处的。

## 要积极进行的工作

正如你所发现的，你部署了OpenStack云平台之后，是和原来的非云平台有个非常大的区别的。举例来说，在新的OpenStack云中，会设置一些规则，让出了问题的服务自动切换到正常工作的设备中，这要是在过去的话，可能需要一名技术人员会持续的紧盯损坏的设备，而使用了OpenStack云平台之后，情况则变了，只需在规划好的时间替换即可。为了能够更好的利用快速补丁和漏洞修复的优点，相比使用了OpenStack之后会更加的频繁。这些工作则会成为管理员的日常工作的一部分。

更为理想的情况是，你的组织不仅能够充分的利用社区资源，还会踊跃的参与到社区的贡献。比如[OpenStack云管理员指南](http://docs.openstack.org/admin-guide/index.html)。
