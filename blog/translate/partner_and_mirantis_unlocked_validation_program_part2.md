# 合作伙伴与 Mirantis 非锁定验证计划 —— 第二波：Fuel 插件


Fuel 的插件机制，方便第三方厂商的接入，也是 UMCloud OpenStack 和第三方合作伙伴实现为客户优雅的解决问题的重要技术手段。本文就为大家展示使用Fuel插件的验证做一明确的介绍。用户、合作伙伴都是 Fuel重要的思考和帮助的对象。

---------

据最近的 OpenStack 用户调查显示，Fuel 已是极为流行的部署工具了，我们相信其中一个重要的原因就是 Fuel 的插件机制。

在 Fuel 6.1 的时候，Fuel 团队将插件的框架纳入 Fuel 的整体架构之中，因为团队已经意识到了让外围的贡献者扩展 Fuel 的功能且在构建的时候不需要动核心部分的重要性。为了实现这个目标，Fuel 团队重构了 Fuel 的架构。

如果你是供应商的话，这也就意味着一旦你要为 OpenStack（或者仅仅是 Fuel） 添加新的功能，你可以直接直接在项目[ Fuel ](https://github.com/openstack/fuel-web)中来添加，或者是创建一个插件。很明显，后者要更加的容易，因为不再需要经过多轮的 blueprint 确认等等。起步也很简单，你为你的 Fuel 插件创建一个项目（例如：[fuel-plugin-mellanox](https://github.com/openstack/fuel-plugin-mellanox)），然后再在 Launchpad 上创建一个项目（例如：[https://launchpad.net/lma-toolchain](https://launchpad.net/lma-toolchain)）。当然，你仍然要遵守 OpenStack 社区的标准，但是你拥有了自主的项目。

目前来讲，在 [Driverlog](http://stackalytics.com/report/driverlog?project_id=openstack%2Ffuel)的列表中有超过30多个 Fuel 插件，在[GitHub 的OpenStack](https://github.com/openstack)仓库下有超过 **60** 个包含“fuel-plugin”名称的项目。（我们还认为还有很多我们不知道的 Fuel 插件），不过你可以想象一下，这些插件给用户带来的丰富的体验。

在我们要深入讨论细节之前，我们要必要澄清一下术语，是我们能够减少一些误解：

* **Fuel 插件框架**： 架构术语，意思是 Fuel 的设计是允许使用插件的形式来扩展 Fuel 的功能的。
* **Fuel 插件 SDK（软件开发工具包）**： 工具、实践、文档以及教程的套装，用来帮助开发者开发 Fuel 的插件，可参考其[维基百科](https://wiki.openstack.org/wiki/Fuel/Plugins)页面，请持续跟踪 SDK 的进展，保持更新。

## 为什么你要验证自己的插件？

开发者们只要对扩展 Fuel 的功能感兴趣就可以随意的创建和使用它，似乎看起来验证并非是必要的。

然而，对于商业上来说，验证却是必须的。如果你要确保自己的插件是和 MOS 兼容的，而且发布到 Mirantis 官方站点的应用目录中的话，你就得深入了解验证流程。这里有一个最大的好处，那就是得到 Mirantis 解决方案架构师和 Mirantis 客户群体的高度信任！

你可以阅读关于[ Fuel 插件的认证流程的详细内容](https://www.mirantis.com/partners/become-mirantis-unlocked-partner/fuel-plugin-development/fuel-plugin-validation/)，我们这里是解释该流程是如何工作的以及为什么会是这样。

在 Fuel 验证过程中，Mirantis 所担当的角色是核心审核者，[ Fuel 插件 SDK](https://wiki.openstack.org/wiki/Fuel/Plugins) 会告诉你：“插件的规则告诉你：程序该这么写、该如何测试、该这么去写文档、该如何去维护”。如果你不打算让 Mirantis 来验证你的插件，这些建议你是否采用则决定于你自己。但是一旦你打算利用验证的优点，这些需求就成为了你必须去满足的需求。

Fuel 插件 SDK 提供了各种类型的模板（包括代码、文档，乃至持续集成等等），但是设计、开发、测试、和文档撰写仍然是插件开发者们的职责，下面表格的对比是 Fuel 插件开发者的职责和 Mirantis 所负责的验证流程：


Fuel 插件开发者 | Mirantis 验证团队 
------------ | ------------- 
Fuel 插件开发者负责所创建的 Fuel 插件是经过了充足的架构设计和保证很好的代码质量。 |   Mirantis 会审核 Fuel 插件的设计规范并会根据实际情况建议改进。
Fuel 插件开发者负责测试覆盖、测试自动化、以及自动构建流程。 | Mirantis 会审核此测试计划、测试报告、以及持续集成的设计，并会提供反馈和必要的帮助。
Fuel 插件开发者负责创建额外支持的文档。 | Mirantis 会审核用户指南并会提供注释。
Fuel 插件开发者负责发布插件，并且能够工作，满足用户的需求。 | Mirantis 会运行用户可接受的测试，如果测试出有Bug存在，会及时的报告给开发者。
 | 插件若是有数量过多且严重的 Bug 存在，就不会通过验证，且会告知用户不建议使用。
   
## 限制

Fuel 插件的验证无法保证所有的插件都能够满足用户的特定使用场景，但是 Mirantis 可以确保插件在其的用户指南中会描述该插件如何工作，而且也会保证列出插件有那些局限性。

有时候，客户至上的 Mirantis 项目团队会遇到这样的情形：所验证过的插件不能够完全满足用户的需求。这是完全正常的现象：Mirantis 的大多数客户都是有着大型部署的机构，所以一些需求是非常“诡异”和特别的，所以一些插件是 MVP 的功能。在这种情况下，就需要为该插件增加新的功能，或者干脆需要另外再写一个。这样的话，Mirantis 项目/开发团队就会联系 Fuel 插件的维护团队，然后共同商定该如何协同推进这件事。

## Fuel 插件和 Mirantis OpenStack

Fuel 插件并非是 MOS 发行版的一部分。插件需要从[Mirantis Fuel 插件目录](https://www.mirantis.com/validated-solution-integrations/fuel-plugins/)中单独下载（前提是用户对验证过的插件感兴趣）。所谓的插件其实是一些 puppet manifest 所要执行的部署之前和之后的动作，以及软件包的安装。

Fuel 是一款开源的（作为 OpenStack 大的蓝图下的一部分），而且我们也建议开发者将插件本身也保持开源，尽管我们允许软件包是商业的。举例来说，Juniper Contrail 的Fuel 插件就是不开源的。（Fuel 社区还建议开发者将写好的插件发布到[Driverlog](http://stackalytics.com/report/driverlog?project_id=openstack%2Ffuel)，从而让现有的插件能够及时的保持记录。）

就上面所举得例子而言，那么问题来了，假如用户需要部署 Juniper Contrail 作为 Neutron 的后端和 MOS 一起会发生什么？在这样的情形下，用户需要从 Mirantis Fuel 插件目录中下载 Fuel 插件，然后到 Juniper 购买 Juniper  Contrail 软件包，在 Contrail Fuel 插件部署和配置之前，需要将 Juniper Contrail 下载下来并放在 MOS 的仓库中。

## 验证流程的其它一些重要的步骤

正如你现在的感觉一样， Fuel 插件的验证是一个复杂的流程，而且还包括了好几个阶段。举例来说，其中一个重要的步骤是我们还没有提到的，那就是制作演示程序。

演示服务有几个目的：

* 展示插件的实战经验
* 能够提供插件是用来干什么的、工作原理如何、以及有何局限等的知识传播。

（这也是为什么不仅仅是 Mirantis 合作伙伴验证团队参与演示的原因，其他参加的有支持团队、解决方案架构师团队、以及销售工程师等。）

还有一些是用户验收（合作伙伴验收）测试比较关心的。当 Mirantis 验证团队收到 Fuel 插件的 rpm 软件包时，就开始安装 Fuel 以及此插件，然后按照用户指南文档中所描述的部署步骤去部署环境，并执行整个测试计划。

基本上我们所重复的也是用户所遇到的，只是为了能够让客户更加的可靠。如果我们发现任何的不一致的地方，或者是漏洞，我们会及时的找合作伙伴，并要求他们及时的修复。

所有验证过的插件（无论是 RPM 软件包还是各种文档）都会发布到 Mirantis 官方的[ Fuel 插件目录](https://www.mirantis.com/validated-solution-integrations/fuel-plugins/)。

## **为什么我们要做这些**

作为一名外来者的视野来说，过程是蛮复杂的了，甚至我们的要求也是蛮苛刻的。但是若是作为 OpenStack 社区的知情人士的话，在开发标准验证流程上我们并未增加任何新的内容。

Mirantis 坚信我们的力量来自于整合合作伙伴！而且是我们的客户信赖我们的整合。

Mirantis 验证团队已经准备好为所有的 Fuel 插件开发者提供帮助。我们会不断的完善 Fuel 插件 SDK，并会在 IRC 聊天室（服务器 Freenode，房间号 #fuel-dev）、OpenStack 的邮件列表、以及[unlocked-tech@mirantis.com](mailto:unlocked-tech@mirantis.com)邮件列表里尽可能的回答疑问。
