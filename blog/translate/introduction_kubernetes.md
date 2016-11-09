# 初识 Kubernetes

Kubernetes ，简称 K8s[^1]，众所周知，管理庞大的容器集群是一件异常复杂的事情，而K8s的目标就是为解决复杂而生，这也就是意味着作为运行容器应用的管理员，毋需关心具体的应用在哪里运行，一切交给k8s就好了。你只需要和 K8s 所提供的REST API 进行交互即可，你可以将你的应用置为意想中的状态，而k8s会去调用基础设施进而满足程序的需求。它可以按照组的方式部署容器、对之进行自动复制、如果有失效的容器还会重新部署它们等等先进的功能。

K8s 是一款开源的项目，它几乎可以运行在现有的所有平台之上。目前所有主流的公有云提供商均提供了此项技术，基于 OpenStack 或 Mesos 的私有云亦可以运行k8s，乃至裸金属架构的服务器也可当做负载的节点。所以，如果你打算使用k8s来包装你的应用的话，你可以运行在虚拟机中、物理服务器中，也可以运行在公有或私有云中。

本文接下来打算介绍一下K8s的基本工作原理，为进一步想了解它的人们做一个良好的铺垫。

### Kubernetes 架构

Kubernetes 的架构是举重若轻的，相对是很好理解的。作为k8s的用户，你永远不会直接和具体的节点打交道，尽管这些节点中运行着用户实际的应用程序，你只能通过k8s的控制层面进行交互，也就是k8s所提供的API，而K8s会去掌控内部的调度、将容器进行复制，在K8s内叫做**Pods**。K8s还提供了命令行接口——**kubectl**，使用此程序，可以和K8s进行实际的交互，如获得想要的程序的状态，或者是基础设施当前的状态详细情形。

接下来，让我们再看看稍细节的内容。

### 节点

每个节点都会运行用户的分布式应用程序的部分，而这些节点就是[Docker](https://www.docker.com/)或相似的容器技术，比如[CoreOS](https://coreos.com/)的Rocket。节点中还会运行两个K8s的组件：一个叫做:[kube-proxy](http://kubernetes.io/docs/admin/kube-proxy/)，有了它，k8s就能够访问用户运行中的应用；另外一个叫做：[kubelet](http://kubernetes.io/docs/admin/kubelet/)，此程序会接收来自k8s控制平面的命令。节点也会运行一些诸如[flannel](https://coreos.com/flannel/docs/latest/)和[etcd](https://coreos.com/etcd/docs/latest/)等后端的容器网络集群组件。

### 主节点

K8s 控制层面运行的服务有：API 服务（[kube-apiserver](http://kubernetes.io/docs/admin/kube-apiserver/)）、调度器([kube-scheduler](http://kubernetes.io/docs/admin/kube-scheduler/))、控制器管理者（[kube-controller-manager](http://kubernetes.io/docs/admin/kube-controller-manager/)）以及[etcd](https://coreos.com/etcd/docs/latest/)。

至于etcd，则实现了用于共享配置的高可用的键值对的存储、由[Raft](http://thenewstack.io/about-etcd-the-distributed-key-value-store-used-for-kubernetes-googles-cluster-container-manager/)共识算法所实现的服务发现功能。

架构示意图如下所示：

![](https://cdn.mirantis.com/wp-content/uploads/2016/11/k8s-arch.png)

好的，看完主要架构，我们稍微解释下其中的主要术语：

### 术语解释

作为高冷的Google的出品，Kubernetes 有自己的一套词汇语言描述系统，所以要很好的理解Kubernetes，首先得理解它特有的专用词汇，如下：

* [Pods](http://kubernetes.io/docs/user-guide/pods): Pods是一个集合，它包括一个或多个容器、以及这些容器的共享存储和运行参数。每个pod都有独立的IP地址。
* [Labels](http://kubernetes.io/docs/user-guide/labels): 标签是k8s标记任何对象的一种键值对，诸如Pods、复制控制器、访问端点等等。
* [Annotations](http://kubernetes.io/docs/user-guide/annotations):用于存储任意non-queryable元数据的键/值对。
* [Services](http://kubernetes.io/docs/user-guide/services): 一种抽象的机制，定义了一组逻辑上的Pods，以及能够访问其网络策略。
* [Replication Controller](http://kubernetes.io/docs/user-guide/replication-controller):复制控制器确保在任意的时间点有特定数量的pod副本在运行。
* [Secrets](http://kubernetes.io/docs/user-guide/secrets):掌管敏感信息，如密码、TLS证书、OAuth令牌、ssh密钥等。
* [ConfigMap](http://kubernetes.io/docs/user-guide/configmap):是一种机制，它用于向容器注入配置数据，同时保持容器对Kubernetes的不可知性。

## 选择 Kubernetes 的理由

虽然k8s带来了一些额外的复杂性，但是仍然是益处多多的。其核心的好处在于，对于集群管理来说服务器的开发者们再也不需麻烦运维团队的帮助才能实现。

卓越的可靠性，绝对是k8s的亮点之一。作为Google十多年的在基础设施运维项目Brog、内部容器编排解决方案的工程经验直接成果，K8s在可靠性方面可谓是得天独厚，K8s在用于应用由于性能和可用性方面出现问题之后的宕机问题上有绝对优势。

至于可伸缩性，Kubernetes是通过在不同的层次来实现的。用户可以通过添加更多的负载节点来为集群扩容，在许多公共云均可以通过基于CPU和内存触发器的自动伸缩功能实现自动扩容。Kubernetes调度器包括类似的功能，可以将工作负载均匀地扩散到整个基础架构，最大限度地提高可用性。另外, k8s可以实现用户的应用程序的自动伸缩，具体是由可自定义触发器来控制的 Pod 自动伸缩器实现的。

既然有如此诸多的好处，是否吊足了您的胃口？如果你住在德州奥斯汀的话，就可以亲临现场，“饱餐”一顿k8s的技术盛宴了，我们将于11月15日在[奥斯汀 的 OpenStack meetup](https://www.meetup.com/OpenStack-Austin/events/234372226/)上讲解 **Kubernetes 101**课程，11月16日举办的[奥斯汀 云计算Meetup](https://www.meetup.com/CloudAustin/events/228918518)同样。没在现场也没有关系，可以访问我们的[Kubernetes和Docker 训练营](https://training.mirantis.com/instructor-led-training/kubernetes-docker-bootcamp)了解详情。

[^1]: 因为Kubernetes，在首字母K和尾字母S之间共有8个英文字符，故约定俗称K8s，英语中比较常见如此的简略方式。