# 民生PoC 日常记录

## 8.15 

周末准备好将qcow2的文件转化为raw再试一次，将问题搞明白了。

周一先将高可用的case过了。将此问题往后顺延。


修改控制节点上的nova.conf

```
service_down_time = 180 ->15
```



```
ceph osd tree 
start ceph-osd id='上述命令的返回id'
lsblk
ceph-deploy --overwrite-conf osd prepare 'osd.hostsname:path'

```

```/var/log/ceph/log``` 下可以查看日志。


 到达民生现场，上传周末转换的raw格式镜像，开始创建120个实例。无论port是否active，实例都无法被访问。求助亚光到现场。亚光重启了一些服务，创建的实例网络通了。但是会有大约20%的实例会失败。问题先搁置，开始测试高可用相关用例。拔网线实例迁移（pass，仅拔管理网络），插拔，ssd和sata硬盘，勉强算是通过？
继续调试。。创建实例、网络不能正常工作的情况。



## 8.16 

9:30 到达用户现场

9:32 张希尧表示对昨天热插拔SSD和SATA的结果不满意，因为操作原因在恢复为原状时出现问题。也就是说插拔完之后，和原来的有些不同。（需要公司懂Ceph的人援助。）

9:35 开始测试120个实例创建。这是一个前提。

9 :40 张希尧的意思是先不要搞创建实例的事情了。先将昨天遗留的Ceph问题解决了。

9:45 开始搞ceph，回忆一下昨天的情景，分析现在情况，然后执行如下命令：
```
umount /dev/sdg3  
``` 

无果，按照[REPLACING A FAILED DISK FROM CEPH A CLUSTER](http://ceph.com/planet/admin-guide-replacing-a-failed-disk-in-a-ceph-cluster/)一文所描述，没有任何的效果。

10:10 针对出问题的机器，将所有磁盘列出来。对比一下。 然后决定重启昨天出问题的计算（融合）节点。

10:30 删除没有创建成功的实例。接着创建。

10:50 重启节点失败，系统无法启动。只有重装了。使用Fuel删除节点。并重新添加。
（张希尧说，是会影响到系统，系统已经废了，100%再现，如果你不能将盘符重新加入进来的话。）

11:10 想想接下来做什么吧。（事情似乎越来越hold不住了。）

11:15 删除计算节点，添加计算节点，部署变更失效。仅试了一次。没有保留现场。（更换节点是有问题的。）

11:20 重新来过，重新部署OpenStack。

13:49 重新部署完成。耗时2.5小时。

查看一些服务是否正常的指令：

```
#crm_mon 或者是#pcs status  //查看控制器集群环境的健康状态。 
#nova service-list
#neutron agent-list
#ceph status
``` 

14:00 开始准备环境。振作起来，没有后援，我是一个人在战斗！~~销售犹豫、没有经验，公司的技术线都在忙于项目，管理一片混乱，几乎每个工程师都似乎介入多个项目。我已经向公司两次提出请求帮助了。没有可能做第三次的求助了。~~

使用Horizon更改一些租户的配额：Identity->Projects->admin->modify quota

```
VCPUs: 100 -> 300
Instances 100 -> 300
Volumes 10 -> 1920
volume snapshots 10 -> 32
Total Size of Volumes and Snapshots(GB) 1000 -> 100000
RAM(MB): 51200 --> 512000
Floating IPs 50 -> 150
Ports 50 -> 150
```

14:16 上传glance镜像。

```
glance image-create --name susesp4 --disk-format qcow2 --container-format bare --fle suse11sp4.qcow2 --progress 
```

14:25 创建第一台实例，用于制作测试使用的软件包。

并将密钥上传上去。为安全组增加两条规则，允许ping和ssh访问。

访问实例，确认一些测试工具是正常工作的：fio,iperf,clush等。

14:30 基于刚才创建的实例，做一个快照。

15:00 开始设计插拔硬盘的步骤：

### 一、删除硬盘

**df 或lsblk 将ceph对应的磁盘列出来**

1. 从集群中将旧的OSD删除：```ceph osd out {osd-num}```
2. 等待所有的数据迁移完成。```ceph -w```会看到pg的状态变化，过程中会有一些：degraded objects，最后的状态应该是```active+clean```。
3. 停止OSD：```service ceph stop osd.{osd-num}```
4. 从 CRUSH map 中将OSD移除：```ceph osd crush remove osd.{osdnum}```
5. 删除认证的密钥：```ceph auth del osd.{osdnum}```
6. 从集群中将OSD彻底删除：```ceph osd rm {osdnum}```
7. 在文件```/etc/ceph/ceph.conf```删除相应的osd项，如果存在的话。

### 二、添加硬盘

1. 列出节点中的磁盘：```ceph deploy disk list {node}```
2. 两步走：
 
```
cephdeploy osd prepare {node}:{devicename}
cephdeploy osd activate {node}:{devicename}
```
如果是有日志的分区的话，还需执行：

```
cephdeploy osd prepare {node}:{devicename}:{journal_dev_name}cephdeploy osd activate {node}:{devicename}```
3. 特别注意：哪个分区使用了日志：查看```/var/lib/ceph/osd/ceph<ID>/journal```这个软链接。
4. 将新的硬盘添加为OSD：
```cephdeploy disk zap {nodename}:{devicename}cephdeploy --overwrite-conf osd prepare {nodename}:{devicename}
```5. 在CRUSH中验证被替换掉的硬盘，以及恢复的状态。
```ceph osd treeceph -s```


**Note: If an OSD is removed from the CRUSH map, a new OSD subsequently createdwill be assigned the same number if ceph osd create is called without parameters.**


**张希尧过来论述了当ceph集群出问题之后，正确的步骤是：先在ceph集群中将出问题的节点删除，然后再从OpenStack中将计算删除，然后再从fuel删除，再添加。** 

**嗯，好吧，所有的坑都得接着填，绕不过去!**

15:15 10台＋10台的创建实例。

16:18 在控制器节点中安装iperf工具：并和虚拟机进行相互链接测试。
 
 SuSE的iperf3和Ubuntu的iperf彼此还不能沟通。

16:50 接着创建实例

## 8.17 

基本上都是测试的场景了。将坑留到最后填：

* 一次性成功的创建120台实例。
* Ceph插拔盘的恢复。

9:10  到达用户现场，开始重命名实例。

9:30 发现做的SnapShot的私钥没有上传，需要重新创建实例。这次一定要记得将这个SnapShot Download下来，如果需要再重新部署的话。

9:55 再次创建实例。

10:10 根据亚光昨天提交的patch，在```/usr/lib/python2.7/dist-packages/nova/conductor/manager.py``` 的762行处添加：

```
import time;time.sleep(0.5)
```
然后创建实例，看是否生效。

10:25 情况更加的糟糕。只有少部分生效了。

11:00 亚光开始测试。

11:10 和张希尧沟通，下午开始测试高可用。

**Bug：实例创建失败之后，neutron-plugin-openvswitch-agent会有错误。要重启一下(计算节点)**

12:38 又出现了网络端口没有随着实例删除的情景。

12:48 使用SuSE的快照来创建实例。一个也没有创建成功

13：34 开始测试下面的用例：

以后则根据测试场景来记录，设计标准格式（测试用例）：

### 测试用例：高可用之控制（网络）节点

测试步骤：

1. 通过IPMI将物理OpenStack控制服务器断电。
2. 在客户端浏览器，访问Horizon，能够列出实例、主机、卷、网络。
3. 在客户端终端，可以访问openstack客户端，执行如```nova service-list```、```neutron agent-list```、```cinder service-list```等操作。
4. 实际创建一个实例。

预期结果：

* 浏览器能够列出实例、主机、卷、网络。
* 客户端可以执行OpenStack客户端。并返回成功。
* 实例可成功被创建。

实际结果： 

* 浏览器能够列出实例、主机、卷、网络。
* 客户端可以执行OpenStack客户端。并返回成功。
* 实例可成功被创建。


**TODO:** 分析高可用的关系
```# crm_simulate --live-check -VVVVV --save-graph tmp.graph --save-dotfile tmp.dot```

### 测试用例：云硬盘的QoS限速

测试步骤：

0. 在某一实例中，测试出当前的iops平均值
1. ```cinder qos-create disk-qos-limit consumer="front-end" read_iops_sec=2000 write_iops_sec=1000``` 此处的值要低于上述的平均值。
2. ```cinder type-create disk-qos-limit```
3. ```cinder qos-associate qos-uuid type-uuid```
4. ```cinder create --display-name disk-qos-limit --volume-type disk-qos-limit 10```
5. 在挂载此qos云硬盘的实例中，再运行fio测试工具。


预期结果：

* iops的值小于等于所设定的值。

**随机写，抖动非常的厉害，64并行写，从3，到370都有。**

实际结果： 

15:36 堡垒机登陆不了。研究shell 脚本。打印出ssh服务器的footprint。

## 8.18 

9:30  到达民生生产运维办公区。
9:33 拷贝iperf2到服务器。


1. 利用clush上传iperf2到各个实例中。 ```clush -g cmbc -v --copy iperf --dest /root/```
2. 

### 测试用例：高可用之融合（计算＋存储）节点

测试步骤：

1. 在ceph集群内部任意节点执行：```ceph osd set noout```
2. 通过IPMI将物理OpenStack融合服务器断电。

预期结果：

* 此服务器上的所有虚拟机平均分布迁移到其他物理机


实际结果： 

执行完恢复环境操作：

1. 将关机的服务器启动
2. 确认启动之后，执行```ceph osd unset noout```


```
ceph osd in osd.id
```


### 测试用例：网络南北向效率

测试步骤：

1. 在网络（控制）节点上运行```ip netns exec qroute iperf -s```
2. 在120台实例中使用clush运行```iperf -c qroute -i -t 300```

记录了测试结果：
 
* 观察并记录网络服务器上的CPU利用率情况。

```
# sar -o output.file 2 500 >/dev/null 2>&1 &
# sar -f output.file
```
* 观察并记录（随机）实例上的网络抖动情况。

见文件north-to-south-preformance.result 

记录secCRT的会话。

**Tips** ssh-keyscan 可以扫描主机到figreprint，将之写入know_host，可以绕过yes回答。从而直接使用clush。

### 测试用例： 热插拔存储节点上的硬盘（模拟）

测试步骤：

1. 使用rbd_fio在某一节点上执行
2. 在某一存储节点上执行``` kill -9 ceph-osd-process-id```


预期结果：

iops 会有短暂的停顿
ceph －w 会有一个恢复的过程。


实际结果： 

模拟杀死某osd的进程失败。start守护进程会瞬间启动。只能等到下次用户有去机房的人员等时候再说了。

环境恢复，参见上面恢复节点的步骤。

### 插曲：

在执行fio的过程中，一个错误将其中一个控制节点的根分区给充满了。导致一些服务down掉：重新启动服务：

* openstack相关：
```
service pacemaker restart
```

* ceph-mon:
```start ceph-mon-all-starter```

### 测试用例：实现网络的Qos功能

测试步骤：

1. 创建flavor
2. 设置此flavor的属性
outbound limit是 1024 kbyte ,inbound limit 是 2048 kbyte 

```
nova flavor-key  matt_test set quota:vif_outbound_average=1024
nova flavor-key  matt_test set quota:vif_inbound_average=2048
```

3. 使用这个flavor启动instance.

预期结果：

带宽会按照预先设定的值。

实际结果：

### 测试用例：使用非DHCP方式注入IP

测试步骤：



预期结果：

实际结果：


### 后续工作

1. 将Mirantis 工作环境所有的服务器信息，发给张希尧。（测试工具）


fuel访问地址：197.1.53.1

用户名：admin 密码：admin


fuel所在宿主机地址：197.1.53.2

OpenStack 图形访问界面：197.1.53.11

用户名：admin 密码：admin

Fuel操作系统及OpenStack节点：

用户名：root 密码：r00tme

控制节点主机名：node-2,node-6,node-9 ,使用Fuel所在节点登录。

融合节点主机名：node-1,node-3~5,node-7~8,node-11

Fuel和OpenStack集群所有节点之间进行了SSH信任。无须密码登录。

LMA监控访问：使用iDrac登录到199.0.130.105终端，使用Firefox访问地址：10.20.0.16:8000， 或者在堡垒机访问197.1.53.20:8000

用户名：lma 密码：abc123

2. 汇报，产品信息，及测试结果。
3. 测试报告。

#### 硬件配置表

| 角色| Fuel| OpenStack控制节点 |计算／存储融合节点| LMA监控节点 |
| ------------ | ------------- | ------------ | ------------ |
| 节点数 | 1 |3  |6  | 1 
| 服务器 |Dell R730	  | Dell R730 | Dell R730XD |  Dell R730
| CPU | E5-2650v3 2.3GHz*2  20核 | E5-2650v3 2.3GHz*2 20核 |E5-2630v3 2.4GHz*2 16核  |  E5-2650v3 2.3GHz*2 20核 
|内存  |192GB  | 192GB |192GB  |  192GB
| 磁盘 | 300GB 2.5 SAS 10K  *2 |300GB 2.5 SAS 10K  *2  | 300GB 2.5 SAS 10K  *2＋4TB 3.5 SATA * 12＋480G SSD *4 |  300GB 2.5 SAS 10K  *2
| 网卡 |板载4端口1GB网卡/Intel  82599ES 10-Gigabit SFI/SFP+万兆网卡（2端口）*2  | 板载4端口1GB网卡/Intel  82599ES 10-Gigabit SFI/SFP+万兆网卡（2端口）*2 | 板载4端口1GB网卡/Intel  82599ES 10-Gigabit SFI/SFP+万兆网卡（2端口）*2 |  板载4端口1GB网卡/Intel  82599ES 10-Gigabit SFI/SFP+万兆网卡（2端口）*2


