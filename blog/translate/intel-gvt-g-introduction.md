# 让基于 KVM 或 XENGT* 的 OpenStack 实现 GPU 虚拟化

## 摘要

在 IDF 2015 会议上，我们曾经给大家展示过一个云计算模式下的媒体传送的例子。

![](https://01.org/sites/default/files/users/u25390/media-delivery-example.png)

在上述的场景中，虚拟机（VM）是没有任何的GPU能力的，这样的方式之所以能够降低一些吞吐量，是因为虚拟机使用CPU来硬解。

今时不同往日，在今年的 IDF 上，我们向大家展示的是利用 Intel® 显卡虚拟化技术（Intel® GVT）来解决困扰社区已久的 Xen* 或 KVM 的问题，实现一个具备处理媒体能力的 OpenStack 云平台。

英特尔的 GVT 技术是一种全GPU虚拟化的技术，借力与穿透的支持。每个虚拟机都会维护一个虚拟GPU的实例，从而实现直接分配需要的性能处理能力。

这样在虚拟机中直接安装本地的显卡驱动（而不是和Hypervisor有所关联），进而优化了性能、特性和共享能力等资源。

## 资源智能分配

在 XEN 或 KVM hypervisor 技术使用 GVT的话，有关资源智能分配的内容是基于如下三种虚拟化技术形式的，即使用分别是 -s ,-d,-g 三个参数：

* **intel GVT ** -d : vDGA: 虚拟专用图形加速（一虚拟机对应一物理GPU）
* **intel GVT ** -s : vSGA: 虚拟共享图形加速（多虚拟机使用一块物理GPU）
* **intel GVT ** -g : vGPU:虚拟图形处理单元（多虚拟机使用一块物理GPU）

![](https://01.org/sites/default/files/users/u25480/gpu-virtualization-approaches.png)

视觉云平台项目使用了 **Intel GVT-g** 的解决方案，且是基于 OpenStack 平台之上的。此解决方案又分为三个部分：

1. **Hypervios 启用** －－ Xen 和 KVM  均明确支持此特性。
2. **中间件 支持** －－ 负责联系 OpenStack 和 Hypervisor 的 Libvirt，所以 Libvirt 是必须要支持Intel 显卡虚拟化的 API的。
3. **OpenStack  支持** ———— OpenStack 相应的支持主要是 Nova 模块：

* 创建一个虚拟机类型，其元数据包含了 Intel GVT 的信息。
* GPU 资源监控
* GPU 资源感知的调度

另外，定制的虚拟机类型可以指定一些特殊的特性，还可以通过 OpenStack 的仪表盘来完成，使用 Intel GVT 包含如下三个关键的属性：

* vgt_low_gm_sz: CPU 可见的最低 gm 大小
* vgt_high_gm_sz：CPU 不可见的最大 gm 大小
* vgt_fence_sz: Fence 寄存器的数量

下图展现了总体的架构：

![](https://01.org/sites/default/files/resize/users/u25390/overall-visual-architecture_0-400x334.png)

当发起一个创建实例的请求时，由 Nova-API 到 nova-scheduler，vGPU 调度的过滤功能会选择最佳的主机，然后 nova-computer 中的GPU资源跟踪会将GPU资源监视起来。

至于 Intel GVT的参数，则是由定制的实例类型传过来的，然后Nova来解析，再交给Libvirt。当然，前提是Libvirt是修改过支持此特性的。

最后，内核模块 XenGT 或 KVMGT 接收到了这些参数，最终创建了虚拟机实例：（下图显示的是各个模块的交互）

![](https://01.org/sites/default/files/users/u25390/interactions-among-modules-02.png)

## 演示环境设置

### 硬件配置

此视觉云计算平台的演示环境，使用了第四代和第五代的Intel® Core™ 处理器的显卡，并使用了三个物理服务器作为 OpenStack 的集群。

| 角色| CPU | 内存 | Hypervisor |
| ------------ | ------------- | ------------ | ------------ |
| 控制节点 | Intel® Xeon® CPU E5-2699 v3 @ 2.30 GHz  | 64GB | N/A | 
| 计算节点1 | Intel Core i7-5775C CPU @ 3.30 GHz  | 32GB | KVM | 
| 计算节点2 | Intel Core i7-5775C CPU @ 3.30 GHz  | 32GB | XEN | 

### 软件配置

* OpenStack: Mitaka
* 操作系统：Ubuntu* 14.04.4 LTS x86_64
* Xen:  4.6.0
* Libvirt: 1.2.2

### Intel GVT 主机设置

关于 Intel GVT 主机一些特殊的配置，更多细节请参考[ Intel GVT 文档](https://github.com/01org/igvtg-kernel/tree/2016q1-4.3.0)。

> 注意：上述链接中的 GitHub 上的文档是针对 XenGT 或 KVMGT 设置的内核配置，OpenStack 服务的配置还需要在内核配置做一些额外的动作。

### OpenStack 配置

本演示环境基于的OpenStack版本是Mitaka，Hypervisor是XEN和KVM混合，均支持intel GVT技术。

1. **Libvirt 修改** 针对GPU虚拟化，对libvirt 的源码进行了修改。
2. **Nova 支持** nova-computer 增加了 GPU 资源的监视代码用来跟踪GPU资源的使用率。而且也在nova-scheduler 中添加了 vGPU的过滤器。
3. **Guest 镜像** kernel 4.1的CentOS 7系列
4. **OpenStack web界面** 新增了可以比较普通实例和带有GPU虚拟化实例的页面。
5. **基准测试** 有下面两种类型测试用例，用来体现 Intel GVT的性能：
   * 来自 Intel® Media SDK 的转码样本——将一个825帧H264 的视频转换为MPEG2；它会展现转码时间和CPU／内存的平均利用率。
   * 3D 游戏：Xonotic* ——会展现 fps 数据。

原文链接：[ENABLING HIGH-PERFORMANCE GRAPHICS ON OPENSTACK* WITH XENGT* OR KVM ON VISUAL CLOUD](https://01.org/zh/igvt-g/visualcloud?langredirect=1)