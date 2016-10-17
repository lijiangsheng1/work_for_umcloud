## 2016.9.19 工作记录

**Done：**

1. 部署好Fuel 9.0 
2. 配置其它所有服务器为PXE启动
3. 配置所有机器均可使用iDrac、IMPI等带外方式访问
4. OpenStack 网络规划（Public、Management、Storage）


**遇到问题及解决办法：**

交换机的配置，开始时PXE网络、Public、Management是使用同一个网卡的。Public和Management划分了VLAN（201、202），PXE是UNtagged、Access模式的。但是PXE网络无法访问到DHCP服务器。

删除角色public、management，仅保留PXE。

**ToDo:**

1. 去除带外网络，挪作public、management网络（**建议：买点网线，最好是角色都能单独进行**）
2. 网络测试
3. 添加新到的浪潮服务器节点
4. 安装LMA插件
5. 部署Mirantis  OpenStack 
6. 部署Ironic

## 2016.9.21 工作记录

```
impitool -I lanplus -H 192.168.10.x -U root -P calivn chassis bootdev pxe 

```

 Dell 开启IPMI监控，然后再执行上述命令。

```
racadmin config -g cfgIpmiLan -o cfgIpmiLanEnable 1
```


Fuel 9.0 竟然遇到slave节点IP地址冲突的现象。也是醉了。将IBM的那台机器换个网口。

记录各服务器的PXE网口IP地址。

开始配置扫描到的各节点。如网络、磁盘等。

## 2016.9.23 工作记录

不按照文档中所指示的那样，也就是说会踩其他人没有踩过的坑。比如PXE网络和管理、私有等网络混用等结果就是无法继续往下走。

重新调整网络。

按照官方文档所指定的要求配置，然后重新来过！ 这大概就是代价！


## 2016.9.27 

```
tcpdump -lnvvvei device-name
```

如若没有外网链接，则需要替换掉bootstrap中的ssh公钥。否则fuel无法连接到各个服务器。

```
unsquashfs root.squashfs
```
替换掉原来的ssh公钥，所替换掉公钥位置在 ```/var/```

```
mksquashfs root.squashfs squashfs
```

手动配置VLAN id，以让OSTF测试通过：
```
ip link add link enox name enox.$vlanid type vlan id $vlanid
```
```
ifconfig enox.$vlanid xx.xx.xx.xx netmask 255.255.255.0 up
```

遗留问题记录：

1. 激活DVR，实例南北网络不通。
2. 控制节点2/3的public网桥，ping其网关不通。
