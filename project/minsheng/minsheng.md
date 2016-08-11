# 网络拓扑图

![](file:///Users/lijiansheng/Documents/UMCloud/%E9%A1%B9%E7%9B%AE/%E6%B0%91%E7%94%9F%E9%93%B6%E8%A1%8C/%E7%BD%91%E7%BB%9C%E6%8B%93%E6%89%91%E5%9B%BE.png)

在UMCloud OpenStack中有以下网络类型：
*  带外IPMI：用于裸机管理。* Admin/PXE：用于节点发现与操作系统安装。* Public：用于外部网络流量和对外访问。* Management：用于OpenStack API流量、消息队列流量、OpenStack数据库流量、虚拟机和分布式存储之间的流量。* Storage：用于分布式存储副本复制流量。* Private：用于虚拟机之间(私有网络)的流量。

# 硬件接口／vlan对应列表

| 节点 | iDrac  | 网卡  | vlanID | 功能 |
| ------------ | ------------- | ------------ |  ------------ | ------------ |
|Fuel(R730)  | 199.0.50.54  |  网卡1(电口)|  1100|  Mirantis OS 部署服务器。|
| 控制节点（网络） （ R730 ）1|199.0.50.55   |网卡1(电口)  | 1100  | Admin/PXE |
|  |   |  网卡2（光口）| 1124| Public  |
|  |   | 网卡3（光口） |247 |  Management |
|  |   | 网卡4 （光口）|255 | Storage  |
|  |   | 网卡5 （光口）| 252| Private  |
| 控制节点（网络） （ R730 ）2| 199.0.50.81  |网卡1(电口) |  1100|  Admin/PXE |
|  |   |  网卡2（光口）| 1124| Public  |
|  |   | 网卡3（光口） |247 |  Management |
|  |   | 网卡4（光口） |255 | Storage  |
|  |   | 网卡5 （光口）| 252| Private  |
| 控制节点（网络） （ R730 ）3| 199.0.50.82   |网卡1(电口) |  1100|  Admin/PXE |
|  |   |  网卡2（光口）| 1124| Public  |
|  |   | 网卡3（光口） |247 |  Management |
|  |   | 网卡4（光口） |255 | Storage  |
|  |   | 网卡5 （光口）| 252| Private  |
| LMA（监控）（R730） |  199.0.50.90  |网卡1(电口) |  1100|  Admin/PXE |
|  |   |  网卡2（光口）| 1124| Public  |
|  |   | 网卡3（光口） |247 |  Management |
|  |   | 网卡4（光口） |255 | Storage  |
|  |   | 网卡5 （光口）| 252| Private  |
|计算节点＋存储节点 1  |  199.0.50.105  |网卡1(电口) |  1100|  Admin/PXE |
|  |   |  网卡2（光口）| 1124| Public  |
|  |   | 网卡3（光口） |247 |  Management |
|  |   | 网卡4（光口） |255 | Storage  |
|  |   | 网卡5 （光口）| 252| Private  |
|计算节点＋存储节点 2  |  199.0.50.51  |网卡1(电口) |  1100|  Admin/PXE |
|  |   |  网卡2（光口）| 1124| Public  |
|  |   | 网卡3（光口） |247 |  Management |
|  |   | 网卡4（光口） |255 | Storage  |
|  |   | 网卡5 （光口）| 252| Private  |
|计算节点＋存储节点 3  |  199.0.50.52  |网卡1(电口) |  1100|  Admin/PXE |
|  |   |  网卡2（光口）| 1124| Public  |
|  |   | 网卡3（光口） |247 |  Management |
|  |   | 网卡4（光口） |255 | Storage  |
|  |   | 网卡5 （光口）| 252| Private  |
|计算节点＋存储节点 4  |  199.0.50.60  |网卡1(电口) |  1100|  Admin/PXE |
|  |   |  网卡2（光口）| 1124| Public  |
|  |   | 网卡3（光口） |247 |  Management |
|  |   | 网卡4（光口） |255 | Storage  |
|  |   | 网卡5 （光口）| 252| Private  |
|计算节点＋存储节点 5  | 199.0.50.79  |网卡1(电口) |  1100|  Admin/PXE |
|  |   |  网卡2（光口）| 1124| Public  |
|  |   | 网卡3（光口） |247 |  Management |
|  |   | 网卡4（光口） |255 | Storage  |
|  |   | 网卡5 （光口）| 252| Private  |
|计算节点＋存储节点 6  |  199.0.50.86  |网卡1(电口) |  1100|  Admin/PXE |
|  |   |  网卡2（光口）| 1124| Public  |
|  |   | 网卡3（光口） |247 |  Management |
|  |   | 网卡4（光口） |255 | Storage  |
|  |   | 网卡5 （光口）| 252| Private  |
|计算节点＋存储节点 7  | 199.0.50.90   |网卡1(电口) |  1100|  Admin/PXE |
|  |   |  网卡2（光口）| 1124| Public  |
|  |   | 网卡3（光口） |247 |  Management |
|  |   | 网卡4（光口） |255 | Storage  |
|  |   | 网卡5 （光口）| 252| Private  |

注意：**若有连接的网卡而未在上述表中体现的，则在实际操作中会以 Bonding 的形式来进行测试。**

# 网络vlan及ip规划建议
| 网络类型 | 网络IP段 | VLAN |
| ------------ | ------------- | ------------ |
| iDrac(带外IPMI) |199.0.50.｛51，52，54，55，60，79，81，82，86，87，90，105｝   |  |
| Admin/PXE | 10.53.0.0/24  | 1100，untagged |
| Public及Floating IP | 10.53.24.0/24  |  1124|
|management  | 197.1.47.0/24  |  247|
| storage |  197.1.55.0/24 | 255 |
| private |  197.1.52.0/24 | 252 |

注：**IP段和VLAN均借鉴了客户给的例子。


## PoC 实际记录  (27日)

安全环境，用户操作，将Mirantis OS 8.0 iso文件及repo和bootstrap等文件上传到内网。

了解网络拓扑结构，准备开始安装。

在访问iDrac的时候发现，客户端的Java环境版本太低。升级Java。

安装完成Fuel后，开始配置网卡。至少Fuel的web 的UI是可以访问的。fuelmenu

第一个难题：在Fuel的节点为可以从内网访问的IP地址。由于民生技术部这边的特殊的网络环境，办公网是访问到这次PoC 环境的IP的。那么Fuel该如何使用就成了大问题。（民生的网络环境，上周才做了变更。经过和网络部门的协调之后，直到下午4点半，才将办公地址访问到Fuel的地址开放。）

复制ISO光盘文件到服务器的过程：

1. 从U盘拷贝到Office的机器上。
2. 从Office的机器上传到经审计的服务器，通过RDP映射磁盘实现。
3. 然后在经审计的服务器操作iDrac的java客户端，即建立虚拟的ISO设备。


**注意：iDrac 的介质映射看起来是非常的方便，但是拷贝起来东西，还真的慢！！！时间长了还可能会出错。**

### 修改默认不使用eth0

启动时，修改fuel的启动参数：eth0为eth2，

### 上传文件小贴士

由于只能使用带外管理来操作机器，所以使用非常规的做法来上传bootstrap和deb文件：

1. 使用Windows的UltrISO将这些文件制作成ISO文件。
2. 使用iDrac的挂载远程介质来将1中制成的iso挂载上来。
3. 进入fuel系统，执行：```mount -o loop /dev/cdrom t ```
4. ``` cp -rvfp t/* ~/umcloud/```


发生的问题非常的“诡异”，在出现Fuel Setup，无论是手动配置，还是自动配置，就没有了后续的动作了！邓锡文的意思是介质是否出现了上传错误？。


### 制作本地源

```cd ~/umcloud```

```tar -zxvf fuel_8.0_repo.tar.gz```

```fuel-mirror apply --group mos ubuntu --default --replace -I /usr/share/fuel-mirror/ubuntu.yaml ```


### 将离线bootstrap加入到系统

```fuel-bootstrap import /root/bootstrap.tar.gz ```

```fuel-bootstrap list```

```fuel-bootstrap activate```

###  安装LMA插件

```fuel plugins --install ~/umcloud/*.rpm```

检验已经安装正常。

```fuel plugins --list```

### Fuel 配置网卡

在交换机的这边均做了LCAP的链路聚合，linux这边不做网卡绑定是不会通的！否则就需要手动的给网卡作VLAN标记，即kernel打开8021q，用户有工具vconfig，因为Fuel的安装是不带有打VLAN标记的工具的。

## 7月28日 流水帐记录：

1. 10:10 到达民生办公室。
2. 10:11 开始拷贝iso文件到服务器。
3. 10:20 开始安装fuel。
4. 10:52 开始配置fuel的网络和no bootstrap等。依然没有执行Fuel setup。
5. 将ISO所有的中转站使用md5sum都过了一遍，确认ISO是完整无误的。
6. 给唐亚光打电话求助，称是eth0的问题。那么想个办法将eth2，更改为eth0. udev？ **我个人有一些疑问，难道－i参数一点都不生效？** 亚光所提供的建议，让用户将网线插拔一下，换个口。被民生这边的人们断然拒绝（他们需要作布线变更申请，那样更加的麻烦。）。
7. 11：30 使用比较dirty的做法，将eth2和eth0，的／etc/sysconfig/network-scripts/下的脚本文件互换？
8. 更改文件``````,添加一行内容```ACTION=="add", SUBSYSTEM=="net", DRIVERS=="?*", ATTR{address}=="00:50:56:8e:3f:a7", NAME="eth2"``` 无果。
9. 13:09 开始部署CentOS6.6 ＋ virtmanage 方案。
#### centos6.6 配置bonding lacp

```modprobe --first-time bonding```

```modinfo bonding```

```vi /etc/sysconfig/network-scripts/ifcfg-bond0```

```
DEVICE=bond0

NAME=bond0

TYPE=Bond

BONDING_MASTER=yes

IPADDR=192.168.1.150

PREFIX=24

ONBOOT=yes

BOOTPROTO=none

BONDING_OPTS="mode=4 miimon=100"
```

```vi /etc/sysconfig/network-scripts/ifcfg-eth5```

```
HWADDR="08:00:27:04:03:86"

TYPE="Ethernet"

BOOTPROTO="none"

DEFROUTE="yes"

PEERDNS="yes"

PEERROUTES="yes"

IPV4_FAILURE_FATAL="no"

IPV6INIT="yes"

IPV6_AUTOCONF="yes"

IPV6_DEFROUTE="yes"

IPV6_PEERDNS="yes"

IPV6_PEERROUTES="yes"

IPV6_FAILURE_FATAL="no"

NAME="enp0s8"

UUID="a97b23f2-fa87-49de-ac9b-39661ba9c20f"

ONBOOT="yes"

MASTER=bond0

SLAVE=yes
```

另外一张网卡同上。

#### centos 6.6 配置网桥

```
chkconfig NetworkManager off

chkconfig –levels 35 network on

/etc/init.d/NetworkManager stop

/etc/init.d/network restart
```

```vi /etc/sysconfig/network-scripts/ifcfg-br0```

```
DEVICE=br0

TYPE=Bridge

BOOTPROTO=static

GATEWAY=10.20.0.1

IPADDR=10.20.0.1

NETMASK=255.255.255.0

ONBOOT=yes
```

```vi /etc/sysconfig/network-scripts/ifcfg-eth2```

```
DEVICE=eth2

ONBOOT=yes

TYPE=Ethernet

IPV6INIT=no

USERCTL=no

BRIDGE=br0
````

将bond0 也同样设置为桥接。

### 开始配置Fuel 环境

10. DHCP 物理服务器获取不到kernel镜像。 系CentOS 的网桥配置问题，默认brctl show竟然没有将物理网卡加入，明明配置写的有，就是不生效。需要手动执行

```brctl addif br0 eth2```

```ethtool eth2 ```

若为linked为no的话，则需要执行：

```ifconfig eth2 up && ifconfig br0 up```

方能生效。~~廉颇老矣，尚能饭否？~~

11. 不过今天的成果是终于将Fuel安装成功。

## 7月29日计划：

1. ~~先在Fuel的环境中重新配置一次 Fuelmenu，看看是否能将rabbitmq的问题解决掉。~~
2. ~~如果1没有解决，则scp MirantisOS8.0的镜像到CentOS6.6中，这样安装Fuel会是飞速。然后再配置Fuel。~~

## 7月29日实际操作：

民生的系统管理员的高手们修复了Docker的问题。 删除Docker镜像，重新启动。

9:20 开始重启物理服务器，PXE启动，让FUEL发现他们。重复10次。并依据这边的服务器命名规则来命名。

10:30 变更了一次197段的IP地址，重启网络服务，导致了昨天的问题重现（centos的bridge不生效）

fuel的IP地址：10.20.0.2 和 197.1.53.1

fuel的宿主机： 10.20.0.1 和197.1.53.2 均为桥接bridge地址。

时间拖沓了一会！

11:30 由于无法根据Mac地址确定网卡是属于那个VLAN，所以工作停滞！ 等待和网络部门的协调。（如果让我去弄的话，只能根据现在所提供的VLAN信息，安装一个Linux操作系统之后，进行一一遍历测试。整个下午就耗进去了。如果我用乐观的心态去对待这件事，那么就是一个不错的学习和锻炼耐心的机会；如果我用悲观的态度对待这件事的话，那就是暴躁的去抱怨浪费时间。）

13：00 民生的工作人员，以试一试的心态，将网卡作了两两绑定。并不能一定能够确定网卡对应的VLAN是一一对应的。

13:30 开始配置各个参数。并使用Fuel的验证网络功能。

14:00 开始部署， 并制作民生所要求的光盘镜像：SuSE和windows2008.

16:00 部署失败，计算／存储节点 部署Ceph－osd出错，称ssh登录控制节点有误。

16:30 ～ 18:30 手动登陆节点。经验证，管理网络和存储网络彼此不通。

## 2016.8.1 人工记录

9:30 到达用户现场。

9:40 赵永杰的建议是先确认 802.3lcap 是否有问题。然后再确认VLAN是否有问题。

9:45 张希尧问我两个问题：0、该如何从Fuel的后端确认Node－X的角色是什么？1、8.0要想让出错的部署更改后继续执行，能够不需要全新来过？我的回答，0、只能靠记录的Mac地址来识别。1、8.0必须重新来过。

9:53 **张希尧开始和他们的网络部的同事沟通，在操作系统中Down 掉网卡，然后网络部那边来判断，哪个Mac是属于哪个VLAN的。**

10:15 开始Reset！

10:30 Reset成功，但是发现网络测试的任务其实是一直未完成的。网络验证会一直停留在60%。

10:50 稍作调整，继续网络测试。

11:03 尝试重启下Fuel的服务器。网络验证仍然未通过。 ```fuel task --delete``` 之后，```Fuel network --env 1 --verify```就会出错，网络验证功能就失效了。

11:41 继续再重启下Fuel服务器。

12:05 暂时放弃Fuel的网络验证功能，直接Fuel开始部署。我自己开始review Fuel的网络测试的代码，究竟搞了些什么动作。(**TODO** network-checker这个rpm包，值得去注意一下。并尝试理解一下都干了些什么。)

14:10 Fuel 部署OpenStack 完成。

14:20 开始运行Fuel OSTF。14:40，OSTF测试完成。

14:45 制作glance镜像

 ```glance image-create --name susesp4 --disk-format qcow2 --containter-format bare --fle suse11sp4.qcow2 --progress```

 ```glance image-create --name windos2008 --disk-format qcow2 --containter-format bare --fle Windows2008.image --progress```

 15:00 开始配置Rally。

 16:00 开始实验性的创建实例、卷、镜像、安全组等。发现上周做的镜像是不可用的。还需要做一些工作。

 需要接下来要做的工作：

 1. 重新制作相关的镜像，仔细阅读《OpenStack镜像指南》，并实践。
 2. 块设备的QoS设置，是怎么回事？需要阅读文档作进一步的理解。
 3. 学习fio和iperf的使用方法。
 4. **some HaProxy backend has down state. Please refer to OpenStack logs for more details.** OSTF 关于HaProxy的集群测试错误。

## 2016.8.2 人工记录

10:10 到达用户现场。开工。

10:20 拷贝windows的iso和suse的image到民生的服务器。

10:25 张希尧要求我将每天的记录发给他。每天干的事情、遇到的问题、解决办法等等。（要客观的记录，先不要先入为主，将糟粕的文化洗脑提到某种高度。）

10:30 堡垒机的中转空间不足，删除一些。

11:10 开始制作Windows2008R2的KVM镜像，参考资料：[OpenStack社区镜像制作文档](http://docs.openstack.org/image-guide/windows-image.html)。目测看起来是非常的好用。一切顺利。```scp windows2008-1.img 10.20.0.2:/var/log/```

11:40 参考了一下友商EasyStack的脚本。关于挂载磁盘等。

11:57 尝试手动创建虚拟机。OpenSuSE的镜像。

12:10 上传新制作的Windows2008R2的镜像到OpenStack环境中。并尝试手动创建一个实例。

12:50 经过检验，所做的Windows镜像是存在问题的，为了上传cloudinit，将Windows镜像配置静态IP地址。多么大的失误？重新来过。不气馁！

13:00 继续重新制作Windows镜像，记住两点：0、关闭防火墙，1、保持DHCP获取IP地址。

13:27 在等待上传镜像的同时，将OpenStack默认的配额更改了一下，其默认是不允许创建多于100个实例的。详见Horizon下管理员的默认值即可，作相应的调整。

13:35 批量创建120个实例。但是只会生效45个。这又究竟是怎么回事？

14:40 一个多小时过去了，无从下手？ 端口不够？依然是网络地址问题？

14:47 难道是public Ip范围分得太大，导致floating IP 范围太小？ 197.1.53.131～249

15:30 亚光电话过来，问我Neutron的quota设置了嘛？好吧，我执行了

```neutron quota-update --floatingip 150```

```neutron quota-update --port 150```

并且在floating ip 的地址池中重新分配了更多Floating IP 地址。

貌似是可以通过了。是可以启动100台实例了，但是报错。

16:00 张希尧想不重新部署的情况下，直接更改Neutron.conf来解决Floating IP规划不够的问题。而我的意思是使用Fuel再将OpenStack的整个环境重新部署一次。

16:30 ~~个人以为既然是网络的规划问题，那么就须将Public IP的地址范围缩小，尽可能多的给Floating IP 即可。~~ 其实还没有到分配floating IP这步。张希尧重新执行了一次竟然好了（不使用创建新卷来启动）。

16:50 继续更新openstack 的配额默认值，vcpu数配置为2000，内存为51200000MB，接着测试批量建立120个虚拟机。

16:59 **Block Device Mapping is Invalid.**只要是选择从镜像启动（创建一个新卷）就会报这个错误。 （待追究）


17:13 执行：

```neutron quota-update --floatingip 250```

```neutron quota-update --port 250```

后，可以成功的建立120个实例了。

failed to allocate the networks,not rescheduling.

明天继续，
1. 追查各种Quota,尤其是Cinder的QoS。
2. OpenSuSE的镜像登陆现象。不行的话，还需要重新配置镜像。(到susestudio下载了一个镜像。不知道靠谱不。)
3. fio和iperf 工具包的下载。（先下载下来再说。不清楚是不是会有依赖。）

## 8.3 人工记录

9:30 到达民生，开始拷贝相应的工具包。
9:45 登陆堡垒机，开启必要的终端等内容。
9:48 登陆到OpenStack Horizon 环境中，查看昨天创建的实例，发现是有错误的。查看Nutron的日志。
9:58 更改floating ip 地址范围：197.1.53.50~249.
10:05 重新创建120台实例，```tail -f neutron-server.log```.
10:30 查看Neutron的服务发现：l3、dhcp、metadata等三个服务都是停止的。手动启动：

```service neutron-l3-agent start```

```service neutron-dhcp-agent start```

```service neutron-metadata-agent start```

10:50 修改neutron.conf 的超时时间项：
```rpc_response_timeout=600
mysql:&read_timeout=60
```
重启neutron 服务```service neutron-server restart```

10:55 在重新创建实例的空隙时间，将SUSE11SP3的镜像使用```glance image-create --name suse11 --disk-format qcow2 --containter-format bare --file SLES11SP3*.qcow2 --progress```

11:09 修改neutron.conf中的```api_worker =100 ```，重启Neutron服务。 **TODO**需要深入学习crm的命令。

12:14 重启所有控制器节点！看是否能将一些服务拉起来。竟然启动不起来了。难道环境坏了？需要重新部署？hacking的时间几乎没有了。

12:50 ```service corosync start && service pacemaker start``` **TODO** 从明天开始学习《Ubuntu系统管理员指南》，并动手实践之！！我说到做到！

12: 53 ```pcs status && crm_mon 1 && pcs config```

13:10 经测试SuSE11SP3 的qcow2文件是很好用的。

13:12 继续创建120个实例， ```tail -f /var/log/nova-all.log``` 并查看```/etc/init.d/nova.conf``` ，看起来worker和一些vif还是有超时限制的。

13:20 开始修改nova.conf文件，

```ec2_worker=300
 conductor workers=100
 vif_pluggin_timeout=600
 ```
 然后执行：

 ```service nova-conductor restart && service nova-scheduler restart```

 13:30 再次创建120个实例。依然失败。可能原因是修改neutron.conf时没有将三个节点的全部覆盖。再来！重启所有三个控制节点的Neutron－server服务。

14:00 测试50个实例会是什么情形？是一样的道理。看来和数量无关。

 14:32 测试Windows 的镜像是否OK。

 14:41 更改/etc/neutron/metadata_agent.ini中的```metadata_worker=100```默认是40 ， 故障依旧。

 15:30 听从亚光的建议，暂时放弃此case，需求公司实施人员的帮助。

 16:00 开始过其它的case，看需要什么样的脚本。

 16:30 安装fio和iperf 的软件包，并开始编写测试脚本。如创建volume、删除volume等。

 16:50 再次尝试创建一台windows虚拟机。这次非常的成功。windows登陆不成功，需要旧密码才能改动。。见鬼了，难道再次做错了？只能再测试一次，不行的话，就再做一次。

 需要进一步查阅资料：

 ubuntu14.04 下开机自启动服务命令：```sudo update-rc.d service enable```

 明天进一步确认的事情：

 0、在nova.conf中配置```vif_pluggin_timeout=0```,重启nova.* 服务，再尝试一次。

 1、再测试一次Windows2008R2的镜像配置。不行的话，就得再做一次。（防火墙、DHCP、administrator密码等）

 2、继续哪些测试的脚本。并自己测试一遍。

 3、Volume和network的QoS设置，并做测试。

 4、nagios告警！文档阅读。
 
## 8.4 工作记录
 
 9:20 张希尧再次要求我将日报提交给他，可暂停一会工作。并且用他们领导来说事，这么多天了都没有个报告？
 
9:35 张希尧又过来催促我一次。

9:50 开始将每台服务器上的配置文件```/etc/nova/nova.conf```项修改如下：

```
vif_plugging_is_fatal=False

vif_plugging_timeout=0
```

并重新启动nova-*服务。参考：[spawning new instances fails to allocate the network](https://ask.openstack.org/en/question/57342/spawning-new-instances-fails-to-allocate-the-network/)

9:59 修改```/etc/cinder/cinder.conf```中的```osapi_volume_worker``` 的值为```60```。然后执行：```service cinder-scheduler restart``` 和 ```service cinder-volume restart```。

10:13 重新制作Windows 镜像。使用cloudbase-init工具时，记得不要选择```user metadata password```.

10:20 继续批量创建120个实例。并关注日志。全部创建成功。

11:00 开始测试Windows镜像。

11:35 经检验确认：The LMA Infrastructure Alerting plugin 这个插件没有安装，那么告警功能就不可用了。测试结果将会受到影响。

12:38 从良付处得知，可以手动添加LBaaS 服务。搜索实施文档，配置中。

文档如下：

以下修改操作涉及所有controller节点
安装neutron-lbaas-agent

```apt-get install neutron-lbaas-agent```

 修改/etc/neutron/neutron.conf配置文件，支持LBaaS	
 ```
 service_plugins=neutron.services.l4_router.l3_router_plugin.L3RouterPlugin,neutron.services.metering.metering_plugin.MeteringPlugin,neutron_lbaas.services.loadbalancer.plugin.LoadBalancerPlugin
```
修改/etc/neutron/lbaas_agent_conf文件
```
interface_driver = neutron.agent.linux.interface.OVSInterfaceDriver
  device_driver=neutron.services.loadbalancer.drivers.haproxy.namespace_driver.HaproxyNSDriver
user_group = haproxy
```
修改horizon配置文件(/etc/openstack-dashboard/local_settings)，页面展现负载均衡功能
```
	OPENSTACK_NEUTRON_NETWORK = {
   	‘enable_lb’: True,
   	}
```

重启neutron-server服务
```
	service neutron-server restart
```

重启neutron-lbaas-agent服务
```
	service neutron-lbaas-agent restart
```

重启apache2服务
```
	service apache2 restart
```
以上内容成功。

13:00 开始撰写磁盘创建和挂载的脚本。并批量测试。

15:12 开始测试磁盘的Qos，命令行如下：

创建一个qos

```
cinder qos-create disk-qos-limit consumer="front-end" read_iops_sec=2000 write_iops_sec=1000

```
创建一个块设备磁盘类型

```
cinder type-create disk-qos-limit

```
将所创建的Qos赋予给此类型的块设备：

```
cinder qos-associate qos-uuid type-uuid
```
创建一个磁盘：

```
cinder create --display-name disk-qos-limit --volume-type disk-qos-limit 1
```
然后将此磁盘挂接到某个实例上，然后到实例所在到主机上查看```/var/log/nova/nova-computing.log```会找到
```qos_specs:{write_iops_sec:1000,read_iops_sec:2000}```这样的，表示生效。

15:50 准备测试一次SuSE的镜像，实在不行，得晚上回去将SuSE studio的哪个折腾一下，需要将fio和iperf放到镜像中。 又是难题。 好吧，我开始手动下载SUSE的RPM包了。

以下是测试性能工具fio和iperf的使用方法：

```
scp -i /var/lib/rally/cloud.key ./*.rpm suse11_based_image:~/
ssh -i /var/lib/rally/cloud.key suse11_based_image 
scp -i /var/lib/rally/cloud.key .\/test-disk.tgz suse11_based_image:~/
tar -zxvf test-disk.tgz
cd umcloud/fio
chmod +x fiorandrw-4k-32.sh  //给此实例挂一块磁盘镜像。
./fiorandrw-4k-32.sh

```

16:29 开始准备测试iperf相关。iperf是客户端／服务端模式。

先到管理界面找到实例所在的主机信息，以及其它相邻的IP地址信息等，然后执行下面命令：

服务器端：

```
iperf3 -s -D
```
客户端：

```
iperf3 -c 192.168.111.xx -i 1 -t 120
```
收集数据即可。

疑问：网络还是可能有问题的。不知道哪个实例获得不了IP地址了。
需要继续查看的问题：ceph的冗余度计算。

## 8.5 操作全记录

9:20 到达用户现场。

9:30 开始计划测试的场景，我干脆做个表格得了：

| 测试项 | 测试结果 | 备注|
| ------------ | ------------- | ------------ |
| 批量创建、删除120台虚拟机 | pass  |  |
| 虚拟机在线迁移 | pass  | ping操作是不会中断的。|
| 使用非DHCP方式注入IP | failed  | not supported |
|无插件支持web VNC登录  | pass  |  |
| 支持自定义导入模板 |  Pass | 先做这个。```glance image-create``` |
|性能监控  | pass  | 服务器和实例 |
|修改虚拟机配置  |  pass |直接测试的是在线修改配置。  |
| Linux openssh密钥注入 | pass  |  |
|重置操作系统管理员密码  | pass  |  |
| 是否有SDK及命令行工具 | 有  | openstack、nova等 |
| 用户管理 |  支持 |  |
|windows虚拟机是否安装virt-io驱动  | pass  |  |
| 整个云平台资源展现 | pass  |  |
|单个物理机使用资源状况展现  | pass  |  |
|总可用容量  | pass  |  |
| 存储冗余度 |  pass | ceph 计算公式 |
|使用FIO进行4k随机50%读、50%写测试120个虚拟机性能  | 详见录屏及生成的报告  |  |
| 创建120×16个10G云硬盘，分别挂载在120个虚拟机，格式化成ext4文件系统，然后卸载 | pass  |  |
|使用上述120×16个10G云硬盘，每个上面做32个快照，然后删除  |   |  执行```cinder snapshot-list```|
| 删除上述创建的云硬盘 |   |  |
|对虚拟机系统盘和云硬盘进行限速，保证SLA  | pass  |  |
|测试同一物理节点内部及跨节点的网络传输效率  | 详见录屏信息。  | 同台和不同主机之间的网速非常的不一样。 |
|支持http、sock端口的负载均衡，可以动态增加删除member，调整member权重  | pass  |  |
|支持虚拟机动态加载浮动IP地址。  |  pass |  |
|测试抓取任意虚机任意端口流量并保存为文件供后续分析  | pass  |  |
| 服务器硬件状态监控 | 部分支持  |  |
|服务器硬件资源使用率监控  | 支持  |  |
| 服务器硬件资源使用率超阈值告警 |  另行测试 |  |
|云管理平台组件工作状态监控告警  |  另行测试 |  |
| 分布式存储组件工作状态监控告警 |  另行测试 |  |
|SSD硬盘故障  |   |  |
|SATA硬盘故障  |   |  |
|计算节点断电  |   |  |
|网络节点断电  |   |  |
|控制节点断电  |   |  |
|计算节点服务器网线拔出  |   |  |
|  |   |  |
|  |   |  |
|  |   |  |

9:45 开始清理原来遗留的内容。例如删除磁盘这件批处理的工作，还是非常的耗时间的。在等待删除的同时，再做一次Windows的镜像。并作为演示程序测试。

11:00 开始和民生的工作人员一起测试，我来操作和解释，他们负责监督和询问。

13:30 测试虚拟机抓包的临时方法， **TODO**： OpenVSwitch 的监控命令

先找到某台实例的ID号：

```
# nova list |grep suse12
```

然后找到其所运行的主机：

```
# nova show SuSE11
```

找到：```OS-EXT-SRV-ATTR:host``` 这一项，看所在的具体在哪台计算节点。然后登陆这台计算节点,并执行：

```
# ps -ef |grep "instance id number"
```

找到其Mac地址

```
# ifconfig |grep ""mac addr"
```
 根据此输出的信息，得到虚拟网卡设备名称（tapxxx）
最后执行：

```
tcpdump -i tap92ff9d45-b7 -s0 -w /tmp/install-SuSE11.pcap

```

14:30 开始测试高可用的case，需要机房的人员配合。但是机房那边有特殊情况，无人配合。推迟到下周。

14:40 考虑 iperf 多线程测试，同一物理机和不同物理机的情况。测试实例的网络状态。

## 8.8 记录

9:40 到达民生科技部生产运营的办公区。

9:45 张希尧帮忙登陆堡垒机。

9:50 开始整理思绪，检查下上周所测创建 120 x 16 x 32个快照的结果。结果是竟然没有执行。真是可悲。

9:55 开始分三次执行上述的命令，预计三个小时完成？

10:00 和张希尧商量下今天的计划。

10:17 troubshoting下horizon下无法将volume snapshot列出的原因。

 里面查到ceph的node－2上的ceph－mon失效。手动启动：
 
 ```/sur/bin/ceph-mon --cluster=ceph -i node-2 -f```
 
 然后执行：
 
 ```ceph status```查看状态
 
 horizon 无法列出snapshot的日志报错：

```
cinderclient.client Load http response test error: Extra data: line 1 column 5 - line 5 column 4 (char 4- 52)'
 
Not Found (HTTP 404)
```
我只能查到这里了。再debug的话，就会浪费掉太多的时间。

12:26 cinder snapshot-create 会出错，太过于频繁的节奏？
 ``` adding snapshot to header failed:(116) Stale file handle```
 
12:54 再次更改默认的系统的一些预设值：
 
 ```
 Total Size of Volumes and Snapshots (GB) = -1
 Volume Snapshots = -1
 Volumes = -1
 Snapshots Volumes Ceph = -1
 ```
13:05 经过```cinder snapshot-list |grep snapshot-*-* ```的追踪，越往后，Cinder越处理不过来频繁的请求。需求中间sleep几秒。三个并发式处理，cinder会罢工。
  在脚本中没循环16次，sleep 10秒。 以环境Cinder的创建队列压力。
  
14:00 经过和 UMCloud 的同事沟通，得知并非是cinder snapshot没有创建成功，而是cinder snapshot－list的终端有限制。
   好吧，我又学习到一条重要的坑：
 修改```/etc/cinder/cinder.conf```文件，添加如下一项内容：
 
 ```osapi_max_limit=100000``` 
 
 系统默认值是1000，导致输出有错误，但是这样的话，无论是从界面看，还是从终端输出，都是超级的慢。
 
 14:30 和张希尧说测试高可用相关，张希尧说你要对机房的操作，要提前请示，我得配工单。那么今天又测试不了了。好吧。看来我的职业生涯真的是要折在这里了。感慨一世英名？眼高手低？
 
 14:40 人家要开例会，所以不能配合我做测试。
 
 15:00 开始准备属主机的iperf测试。并收集数据。
 
 16:00 和张希尧过了一遍测试用例。挑选出来他认为需要再过一遍的内容。
 
17:00 执行删除snapshot的脚本。
 
17:10 将SUSE的实例做成快照，然后从快照启动114台虚拟机。然后上传SSH-key到原生的实例中：197.1.53.142
 
我自己需要确定的内容：

1. OpenStack  LoadBlance 的测试case
2. clush的使用，同时在120台虚拟机上执行fio的随机读、写测试。（SUSE 11SP3 的软件包）
3. nova live migrate的命令行工具。 ```nova host-evacuate-live```
4. google一下openstack中的镜像，能否随时注入ssh-key。


## 8.9 记录

1、先上传工具，windows镜像和集群shell工具。

1.5 搞定clustershell的信任、登录问题。

只需要将为实例制作的私钥上传到作为clustershell的机器即可。

2、为每个虚拟机创建一块数据盘，并挂接到对应的虚拟机中。

3、使用FIO进行4k随机50%读、50%写测试120个虚拟机性能

4、对虚拟机系统盘和云硬盘进行限速，保证SLA

5、支持http、sock端口的负载均衡，可以动态增加删除member，调整member权重

6、支持基于端口的防火墙

7、迁移一台物理机上的20台虚拟机到其他物理机，通过ping判断迁移过程中服务中断时间

8、测试高可用，（重点。）未完成，需要走工单、流程。

9、测试下Windows镜像是否带virtio的驱动。


bug： Grafana 的时间是超前的。

13:00 的时候，我邀张希尧开始测试，他说13:30才开始上班了。要知道，我早、中饭都还没有吃了。
 
16:00 测将某主机上的所有虚拟机都live migrate到其它节点，开20个窗口，ping某个地址，是否间断。重点不在这里，是因为张希尧有点小事，我重新做了两遍，迁移走再迁移回来可是需要手动操作的。

## 8.10  记录

1、按照[OpenStack, Ceph RBD and QoS](https://www.sebastien-han.fr/blog/2013/12/23/openstack-ceph-rbd-and-qos/)一文的描述，加上fio进行磁盘QoS测试。

2、再次使用iperf进行南北网络的测试。具体情形和张希尧商议。

3、测试高可用。

热插拔硬盘（SSD和SATA）前执行下面的命令：

```
ceph osd set noout
```

重新插上硬盘之后，再执行：

```
ceph osd unset noout
```

具体解释请参考官方的[Troubeshotting STOPPING W/OUT REBALANCING](http://docs.ceph.com/docs/hammer/rados/troubleshooting/troubleshooting-osd/)一节。


9:20 到达民生现场。

9:25 发现控制器上所有的服务都宕了。pacemaker显示的是MySQL 服务启动不了。

11:00 我重装的心思都有了。抱着最后的希望，重新启动了三个控制器节点。

11:10 勉强可以使用了。

11:50 pacemaker 继续将所有的服务关掉。MySQL服务再也启动不了。

14:20 问题依旧，无果。内心充满了压力。

14:50 宁鹏和冯宇祥到达。对于问题没有任何思路。Google了一些pacemaker、mysql的消息。但对问题毫无帮助。

诡异的现象：

执行了：

```
crm resource maintance p_mysql
```

之后，怎么也加不回去了。

```
crm resource manage p_mysql
```
或者是

```
crm resource maintance p_mysql off```
```
均不可以。

## 8.11 记录

9:30 到达民生系统运维中心办公区。
开始debug MySQL启动的问题。参考Pacemaker的文档：[Debuging resource failures](http://clusterlabs.org/wiki/Debugging_Resource_Failures)

执行如下命令：

```
crm resource unmanaged p_mysql
crm configure show p_mysql
export OCF_ROOT=/usr/lib/ocf
export OCF_RESKEY_test_user=wsrep_sst
export OCF_RESKEY_test_passwd=
export OCF_RESKEY_socket="/var/run/mysqld/mydqld.sock"
sh -x /usr/lib/ocf/resource.d/fuel/mysql-wss start;echo $?
```
报的错误是```mysql-wss: line 342: TMP: bad array subscript```

# 总结

一、时间耗费及测试扣分项

1. 民生的测试环境，涉及部门沟通，机房人员、网络人员、冗长的流程等。
2. 产品本身的缺陷。（fuel只接受eth0、120 instance创建失败）
3. 我个人对于PoC的预估不足,准备不够充分。
4. 民生的测试用例，没有来得及仔细评估，等于按照他们的思路（基本上是操作系统、虚拟化层的性能问题）测得，反而我们的产品优势没有得到任何的体现。如Fuel的快速安装、部署。
5. 测试工具的准备，如镜像的制作、测试工具的安装等。

二、经验教训

1. 不要同时支持多个项目，到最后哪个都支持不好。
2. 遇到官僚机构，技术本身相比沟通显得微不足道。
3. 对于OpenStack在实际中的PoC经验不足，面对用户的冥顽不化，拒绝学习／沟通的态度有些措手不及。

三、团队建议

1. 售前和技术线同事缺乏接口，以及反馈机制。
2. 公司的技术文档等严重匮乏，或者是分享通道不足。
3. 新手的磨合、适应机制短板。




