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
