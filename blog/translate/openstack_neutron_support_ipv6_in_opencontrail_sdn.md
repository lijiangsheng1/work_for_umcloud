
## 摘要

作为一家知名的私有云（主要是基于OpenStack）部署专家和集成商，经常被客户问到的一个问题就是关于对IPV6的支持，众所周知，Mirantis的客户通常有很多是部署 OpenContrail SDN／NFV的，具体原因我们在文章:[OpenContrail SDN 测试案例](http://www.tcpcloud.eu/en/blog/2015/07/13/opencontrail-sdn-lab-testing-1-tor-switches-ovsdb/)有过更为详细的描述，其实，OpenContrail SDN 支持IPv6 由来已久，只是缺乏一些实际的测试数据，因此我们决定和大家分享一些有关在OpenStack中使用IPV6的配置过程。

本文即是对在OpenStack支持IPV6的情况，Neutron的插件使用的是OpenContrail。

## IPV6的实际需求

对于云供应商来说，公有的IP地址是一个重量级的需求。那么这就会面临一个问题：IPV4 地址的紧张，那么其中的一个解决办法就是使用IPV6。

在本文中，我们先是实现了一个虚拟网络下虚拟机之间的内部通信，然后是跨不同的虚拟网络之间的通信，最后是实现扩展IPV6的公有地址到外部互联网。环境使用的是Juniper MX 路由器作为云的网关。

## 创建IPv6 网络

在创建IPv6的虚拟网络之前需要考虑清楚几件事，第一，是增加一个IPv4的子网，因为若没有IPv4的子网的话，实例就无法连接到nova的元数据API，云的镜像都是构建的利用cloud-init去访问 169.254.169.254:80 这个地址的，所以，加入你没有创建这个IPv4的子网的话，你的实例就无法接收到元数据。第二个考虑的是，使用了IPv6的实例如何访问外网的问题，当前的问题是拥有一个基于IPv6的资源池，所以如果你打算访问外部的网络，就需要在启动实例时关联到目标路由。

我们首先为演示程序创建私有的IPv6网络：

![](https://www.mirantis.com/wp-content/uploads/2016/12/ipv61.png)

当网络创建完成之后，我们就可以启动实例了。为了达到试验目的我们启动了2个虚拟机实例，用户可能需要自己更改虚拟机操作系统的网卡配置，因为通常Ubuntu默认对IPv6的支持是禁用的。对于非抢先接收，您可以使用：

``` #dhclient -6 ```

![](https://www.mirantis.com/wp-content/uploads/2016/12/ipv63.png)

正如你所看到的，实例中的网卡同时拥有IPv4和IPv6。

![](https://www.mirantis.com/wp-content/uploads/2016/12/ipv64.png)

在测试通信之前，我们需要更改流量的安全组策略。这里仅测试之用，所以将端口全部打开：

![](https://www.mirantis.com/wp-content/uploads/2016/12/ipv62.png)

如下图所示，我们使用主机名叫做 ubuntu-ipv6-1 的实例，来ping 主机名为ubuntu-ipv6-2，IPv6地址为fd00::3

![](https://www.mirantis.com/wp-content/uploads/2016/12/ipv65.png)

以下为终端ping的输出：

![](https://www.mirantis.com/wp-content/uploads/2016/12/ipv66.png)

以上即是实现内部实例之间的IPv6互通的情形，但是通常是公有IP地址的短缺，所有IPv6真正的用途应在公有网络，我们将创建关联到路由目标，以通过BGP扩展路由到 Juniper MX，下图架构示意图，在每个MX路由上均创建有VRF CLOUD-INET，路由目标关联到VRF匹配的路由目标在Contrail中添加到虚拟网络，此图所展示的是同时有IPv4和IPv6传播到相同的VRF，还有INET虚拟路由器，通过运行ospf和ospf3的隧道接口连接到VRF。从这个虚拟路由器是从上游EBGP的所有互联网路由聚合的默认route :: / 0。

![](https://www.mirantis.com/wp-content/uploads/2016/12/ipv6-expand.png)

在云平台中还需要配置MX 路由来启用IPv6，首先通过mpls隧道来启用IPv6的隧道。

```
protocols {
    mpls {
        ipv6-tunneling;
        interface all;
        }
```

将路由能够导出和导入是很好的实践，我们仅需要默认的路由在当前的云平台中。而且我们也只需要从Contrail中导入的IPv6地址，因为IPv4的地址池是此IPv6虚拟网络所创建：

```
policy-statement CLOUD-INET-EXPORT {
    term FROM-MX-IPV6 {
        from {
            protocol ospf3;
            route-filter ::/0 exact;
        }
        then {
            community add CLOUD-INET-EXPORT-COMMUNITY;
            accept;
        }
    }
    term LAST {
        then reject;
    }
}
policy-statement CLOUD-INET-IMPORT {
    term FROM-CONTRAIL-IPV6 {
        from {
            family inet6;
            community CLOUD-INET-IMPORT-COMMUNITY;
            route-filter 2a06:f6c0::/64 orlonger;
        }
        then accept;
    }
    term LAST {
        then reject;
    }
}
community CLOUD-INET-EXPORT-COMMUNITY members target:64513:10;
community CLOUD-INET-IMPORT-COMMUNITY members target:64513:10;
```

现在，我们已经创建了2a06:f6c0::/64网络，为将此网络关联到路由目标64513:10，我们可以在此网络中共享给所有的租户，一旦我们在此网络中创建实例，此实例就会默认路由到MX路由表的信息：

```
# run show route table CLOUD-INET.inet6.0

CLOUD-INET.inet6.0: 8 destinations, 9 routes (8 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both

::/0               *[OSPF3/150] 20:37:13, metric 0, tag 0
                    > to fe80::6687:8800:0:2f7 via lt-0/0/0.3
2a06:f6c0::3/128   *[BGP/170] 00:00:15, localpref 100, from 10.0.106.84
                      AS path: ?, validation-state: unverified
                    > via gr-0/0/0.32789, Push 1046
                    [BGP/170] 00:00:15, localpref 100, from 10.0.106.85
                      AS path: ?, validation-state: unverified
                    > via gr-0/0/0.32789, Push 1046

```

我们可以通过Contrail的界面来验证这一结果：

![](https://www.mirantis.com/wp-content/uploads/2016/12/ipv610.png)

若实例拥有IPv6的公有地址话，是可以访问互联网的：

![](https://www.mirantis.com/wp-content/uploads/2016/12/ipv67.png)

ping google即可。

## 总结

以上即是我们验证OpenContail SDN解决方案，在私有云平台下完整的支持IPv6的能力，并通过诸如 Juniper MX, Cisco ASR,等路由器实现外网的访问和连接。
