# Fuel 及其生态介绍

Hi，大家下午好，如果你是开发者的话，这个时间恐怕都是在午休，昨晚或许已经熬夜很晚了，下午的生产力全靠这会的休息。嗯，没错，你来对了会场，请大家闭上眼睛。即使不闭上，接下来我的分享也一定会让你进入香甜的梦乡。

我这边的议程其实就是本次分享的题目：Fuel 和它的生态介绍。


先吐槽大会，本来我是以个人的身份，申请过演讲的，内容是我最热衷和擅长的开源文化。可惜被pass掉了。现在能够站到这里，其实是因为公司花了很多钱的结果。所以提前警告下大家，演讲内容可能有暗示的商业宣传。

首先来介绍下我自己，不知道会场有没有过去就认识我，看到议程之后过来给我捧场的？先谢过！今天我只有一个身份、一个标签，那就是一名来自 UMCloud 工程师。

转入正题，首先我们来看下什么是Fuel，请允许我做个调查，在座的各位有多少是使用过Fuel的？有多少是从来没有听说过的了呢？好吧，谢谢大家。那我还是来普及一下吧。

在正式的介绍之前，我们先来看看一个典型的OpenStack场景的生命周期，UMCloud认为首先分为两大部分：部署前的初始化和部署后的运维。 我这里有一张Mirantis OpenStack 9.0 发布时的示意图，可以非常清晰的看到 Fuel 在OpenStack 的整个生命周期管理中所占有的重要的位置：安装、部署、配置和变更！

用一句话来描述Fuel就是：OpenStack 的部署和管理工具。让运维的日子轻松一点。

再来看几个关键的特性：

1. 拥有强大的命令行工具，Hacker的最爱。
2. 可以在部署之前就进行一些验证配置，比如网络。
3. 硬件自动发现
4. 可以分别管理多个 OpenStack 环境
5. 在web界面上看所有的系统日志（实时哦）
6. 更多内容请访问社区网站


Fuel 一直都在成为 OpenStack 的安装、部署工具事实上的标准而努力，无论从架构设计，还是开发原则、社区协作上，都在努力的遵从 OpenStack 的哲学思路。

接下来，让我们来了解下其技术架构，按图索骥：

![](https://www.fuel-infra.org/static/img/fuelScheme.png)

Fuel 组件有：Nailgun、Astute、Cobbler、OSTF、MCollective、PostgreSQL、web界面／命令行工具。组件之间的通信方式：AMQP消息队列和 RESTful API。

* **Nailgun**：任务管理、卷管理、网络管理、其它编排数据管理
* **Astute**：provisioning 驱动到Cobbler，部署驱动、部署后执行动作到MCollective然后再到Node的各种代理：puppet、网络验证、节点删除。
* **Cobbler**：一套强大的物理机（虚拟机）自动、批量部署服务器套件。
* **OSTF**：健康检查，在安装完成后，确保所有的组件都属于正常运行状态，如Horizon、创建实例等。

需要指出的是，所有的这些组件都是容器化了的，你在Fuel的所在操作系统查看它们需要执行命令：

```
docker ps 
```

以上便是Fuel的大体介绍，限于时间，只能到这里了，有兴趣的同学可以到[如何构建和持续集成](http://docs.openstack.org/developer/fuel-docs/devdocs/buildsystem.html)，实际编译一把。或者到IRC、邮件列表中进行讨论。

既然今天的主题是生态，那么接下来我们就看看，Fuel是如何实现生态的构建的。也就是Fuel的插件！

Fuel插件实现了简单扩展Fuel功能（安装和配置）的途径，通过预先定义好的规范和提供相应的SDK和文档。

插件的编写主要是按照SDK来写Yaml配置文件，以及Puppet实际部署步骤。

想要实现Fuel插件怎么写，最好的方式莫过于查看已有的插件的源代码，大家可以到github上去找找看，说不定你要实现的，已经有人已经实现了。发展至今已经有220个插件可用，内容涉及：网络SDN、存储、运维等等。

以上便是今天要谈论的Fuel的全部内容了，接下来是后半部分：生态。

OpenStack 为了社区、为了开源，走的是情怀和理想路线。所以不会对任何的商业产品有所妥协。所以OpenStack和商业产品的整合在社区是蛮困难的一件事情。幸亏有Fuel，对于商业软件的支持作了改进，并以插件的形式提供给合作伙伴。

Slide所展示的是经过Mirantis验证计划的合作伙伴，这里举几个例子，比如 Juniper 的 SDN ——Contrial、WMware的DVS／NSX插件、StackLight、Citrix XenServer、SwiftStack、EMC等等。最近我正在写一篇文章，就是对目前Mirantis所验证过的插件作一个全面的介绍，已经完成了80%，相信不久大家就能够阅读得到。

谈到Fuel的生态，就不得不提到连Fuel本身都在服务的OpenStack，前几周，Mirantis 的 Boris Renski 发表了振聋发聩的文章 [Infrastructure is dead](https://www.mirantis.com/blog/infrastructure-software-is-dead/) ，OpenStack 本身不是一件产品，你将它做成一款软件产品是不现实的。看看 Mirantis OpenStack 是如何做的！将社区、公司、用户放在一起，通力协作，构建生态。Mirantis 不遗余力的在社区贡献，看下奥斯丁发布的M版，Mirantis的贡献度排名。

UMCloud 所提供的 Mirantis OpenStack 是这样子的。

Mirantis 无锁定哲学思路的最好体现。

我们深刻了解开源，也对生态有着深入的理解，并践行和完善这一生态。实现用户、社区、自身的共赢！此处我想给大家举个例子，比如 Mirantis 所支持的对象存储是 Ceph，并不支持Swift，但是SwiftStack 有 Fuel 的插件。这个例子很好的说明了Mirantis 对于开源生态系统的理解。那就是对最终用户的尊重与理解！

在最后，正如我开始时警告大家的一样，广告时间，UMCloud 作为国内最大的独立公有云UCloud和纯粹的演绎 OpenStack 的硅谷公司Mirantis 的合资公司，在中国正在加速让 OpenStack 落地，我们需要优秀的你的加入。关注UMCloud 公众号，获得更多技术干货和OpenStack案例。

Q&A 时间。
