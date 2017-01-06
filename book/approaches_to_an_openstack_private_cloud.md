
如果你经过了上述的考虑，决定使用OpenStack来作为唯一的云计算平台的话，那么，你是幸运的，因为下面的内容就是为你如此睿智的决定而准备的。其实，还有一个原因，就是接下来，你该决定如何使用、或者说以什么样的形式来使用OpenStack，通常情况下有三种方式：付费给为你管理OpenStack的公司（托管云）、使用OpenStack发行版（OpenStack发行版）、从OpenStack.org网站下载源码自行构建（DIY）。

## 交钥匙的云计算形式：托管云

托管云，也就意味着将自己的云计算平台交付给专业的公司／团队去打理，自己仅仅是个使用者。需要这个角色专门管理云平台最终用户（企业的雇员和开发者）和云平台本身之间的关系。此运维做的工作有诸如创建用户、定义最终用户可以消费的云平台资源配额、管理消费的分配、以及发生故障时配合合作伙伴修复等。更多信息，请访问OpenStack[托管云供应商](http://www.openstack.org/marketplace/hosted-private-clouds)页面了解。

## 依托于技术支持：采用OpenStack商业发行版

用户可以以很小的力量做体量很多的云平台，[OpenStack商业发行版供应商](http://www.openstack.org/marketpalce/distros)会对项目本身做一些相应的开发，如测试、缺陷修复、打包等，让OpenStack更加适应用户的架构等，其实就花钱这件事情来讲，用户并非是为OpenStack本身的软件而付费的，用户付费的是落地的过程以及之后的支持和服务，而这些都是用户可以控制和管理的。

如果用户选择了使用OpenStack商业发行版了，作为用户所在的企业就需要专门的运维人员这样的角色，这和上面谈到的托管云是一个道理。但是用户还需要云计算架构师，这个角色会为云平台选择相应的硬件、基础设施软件、设计云平台的配置、以及决定云平台最终提供何种服务——存储？计算？还是网络？

云计算架构师和商业发行版供应商通力协作，去安装、配置整个云平台，当然，架构师的角色还对云运维人员进行日常的管理，而运维人员所做的事情就是性能、容量监控，以及故障修复、安全管理等。

至于这些角色具体的承担者，则视用户所在公司的规模以及云平台的要求，具体而定。

## DIY 云平台

对于灵活性和可定制性有要求的厂商来说，基于OpenStack 来DIY一个自己的云平台是最好的选择了。除上述使用商业发行版时所需要的之外，还需要一个工程师团队来做诸如掌控任务（高级支持、功能实现、打包、测试等发行版厂商干的事情）。

DIY 云平台最快的方法是使用商业发行版供应商的OpenStack版本，区别可能仅仅是没有给供应商支付技术支持、咨询等费用罢了。这样既有好处，也有坏处。好处就是能够利用供应商的开源免费版本，最快的将云平台搭建起来，坏处也是显而易见的，受限于OpenStack组件的选择，只能用到OpenStack部分，而且软件的发行周期和bug修复都受限于供应商。

在[OpenStack 项目导航](http://www.openstack.org/software/project-navigator)中，用户可以查到有哪些项目是被鼎力支持的。

表二：OpenStack私有云模式的问题考虑及答案


<dl>
<table style="width:100%">
  <tr>
    <th colspan="4">此云平台将会是我们业务的核心吗？</th>
  </tr>
  <tr>
    <td>是</td>
    <td>DIY／自己搞定</td>
    <td>使用商业发行版</td>
    <td>托管</td>
  </tr>
  <tr>
    <td>否</td>
    <td>使用商业发行版</td>
    <td>托管</td>
    <td></td>
  </tr>
</table>
</dl>

<dl>
<table style="width:100%">
  <tr>
    <th colspan="4">云平台需要在接下来的3～6个月内上线吗？</th>
  </tr>
  <tr>
    <td>是</td>
    <td>使用商业发行版</td>
    <td>托管</td>
    <td></td>
  </tr>
  <tr>
    <td>否</td>
    <td>DIY／自己搞定</td>
    <td></td>
    <td></td>
  </tr>
</table>
</dl>

<dl>
<table style="width:100%">
  <tr>
    <th colspan="4">我需要第一时间就上线OpenStack新的特性吗？</th>
  </tr>
  <tr>
    <td>是</td>
    <td colspan="3">DIY/自己搞定</td>
  </tr>
  <tr>
    <td>否</td>
    <td>发行版</td>
    <td>托管</td>
    <td></td>
  </tr>
</table>
</dl>

<dl>
<table style="width:100%">
  <tr>
    <th colspan="4">会通过为上游贡献或者向供应商提出需求来试图影响OpenStack的发展方向吗？</th>
  </tr>
  <tr>
    <td>是</td>
    <td colspan="3">DIY/自己搞定</td>
  </tr>
  <tr>
    <td>否</td>
    <td>发行版</td>
    <td>托管</td>
    <td></td>
  </tr>
</table>
</dl>

<dl>
<table style="width:100%">
  <tr>
    <th colspan="4">我需要将合作伙伴扩大吗？</th>
  </tr>
  <tr>
    <td>是</td>
    <td>发行版</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>否</td>
    <td>DIY／自己搞定</td>
    <td>托管</td>
    <td></td>
  </tr>
</table>
</dl>

<dl>
<table style="width:100%">
  <tr>
    <th colspan="4">私有云的供应商所呈现的特性能够满足我的要求吗？</th>
  </tr>
  <tr>
    <td>是</td>
    <td>发行版</td>
    <td>托管</td>
    <td></td>
  </tr>
  <tr>
    <td>否</td>
    <td>DIY／自己搞定</td>
    <td></td>
    <td></td>
  </tr>
</table>
</dl>

<dl>
<table style="width:100%">
  <tr>
    <th colspan="4">云平台拥有最大的控制权，对于我对业务能够产生最大的利益吗？</th>
  </tr>
  <tr>
    <td>是</td>
    <td>DIY／自己搞定</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>否</td>
    <td>发行版</td>
    <td>托管</td>
    <td></td>
  </tr>
</table>
</dl>

<dl>
<table style="width:100%">
  <tr>
    <th colspan="4">我所在的组织想要对云平台对构实行一次性的投资了呢？还是只是仅为服务付费?</th>
  </tr>
  <tr>
    <td>是</td>
    <td>DIY／自己搞定</td>
    <td>发行版</td>
    <td></td>
  </tr>
  <tr>
    <td>否</td>
    <td>托管</td>
    <td></td>
    <td></td>
  </tr>
</table>
</dl>

<dl>
<table style="width:100%">
  <tr>
    <th colspan="4">我的业务有足够的资源来支持适量的IT人员运行我的云平台吗?</th>
  </tr>
  <tr>
    <td>是</td>
    <td>DIY／自己搞定</td>
    <td>发行版</td>
    <td></td>
  </tr>
  <tr>
    <td>否</td>
    <td>发行版</td>
    <td>托管</td>
    <td></td>
  </tr>
</table>
</dl>

<dl>
<table style="width:100%">
  <tr>
    <th colspan="4">我所选择的OpenStack私有云模式所需要的技能，我司是否有对应的工程师？</th>
  </tr>
  <tr>
    <td>是</td>
    <td>DIY／自己搞定</td>
    <td>发行版</td>
    <td></td>
  </tr>
  <tr>
    <td>否</td>
    <td>托管</td>
    <td></td>
    <td></td>
  </tr>
</table>
</dl>

表三：不同的形式所带来的商业价值的高低

<dl>
<table style="width:100%" >
 <tr >
   <th></th>
   <th>托管</th>
   <th>有支持的发行版</th>
   <th>DIY</th>
 </tr>
 <tr>
   <td>产品化的时间</td>
   <td>最快</td>
   <td>中等</td>
   <td>最慢</td>
 </tr>
 <tr>
   <td>配置的灵活性</td>
   <td>低</td>
   <td>高</td>
   <td>最高</td>
 </tr>
 <tr>
   <td>软件可定制化</td>
   <td>无</td>
   <td>由供应商制定的规则决定</td>
   <td>不受任何的限制</td>
 </tr>
 <tr>
   <td>支持程度</td>
   <td>供应商</td>
   <td>供应商</td>
   <td>社区</td>
 </tr>
 <tr>
   <td>额外的开销</td>
   <td>高</td>
   <td>中</td>
   <td>低</td>
 </tr>
 <tr>
   <td>内部资源的需求</td>
   <td>低</td>
   <td>中</td>
   <td>最高</td>
 </tr>
 <tr>
   <td>资源所需的技能级别</td>
   <td>低</td>
   <td>中</td>
   <td>高</td>
 </tr>
</table>
</dl>
