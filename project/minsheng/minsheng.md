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