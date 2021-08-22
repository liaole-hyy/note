---
typora-root-url: images
typora-copy-images-to: images
---

#### 一、Lvs介绍

LVS(linux Virtual Server)即Linux虚拟服务器。它用于多服务器的负载均衡，工作在网络四层，可以实现高性能，高可用的服务器集群技术，它稳定可靠，即使在集群的服务器中某台服务器无法正常工作，也不影响整体效果。是基于TCP/IP做的路由和转发，稳定性和效率极高。

#### 二、Lvs负载均衡模式

lvs提供了3种负载均衡模式，每种负载均衡模式适用的场景差异、

1.NAT模式
用户请求到分发器后，通过预设的iptables规则，把请求的数据包转发到后端的RS上去，RS需要设定网关为分发器的内网ip。用户请求的数据包和返回给用户的数据包全部经过分发器，所以分发器称为瓶颈。在NAT模式中，只需要分发器有公网IP即可，
所以比较节省公网ip资源

2.TUN
这种模式需要有一个公共的ip配置在分发器和所有RS上，我们把它叫做vip.客户端请求的目标ip为VIP,分发器接收到请求数据包后，会对数据包做一个加工，会把目标ip改为RS的ip,这样数据包就到了RS上，RS接受数据包后，会还原原始数据包，这样目标ip为vip,因为所有RS上配置了这个VIP,所以他会认为是它自己,这里VIP分发走了外网，分发的性能会受到限制

3.DR模式
和IP Tunnel较为像似，不同的是，它会把数据包的mac地址修改为RS的MAC地址。真实服务器将响应直接返回给客户。这种方式没有ip隧道的开销，对集群中的真实服务器也没有必须支持ip隧道协议的要求，但是要求调度器与真实服务器，都有一块网卡连在同一个物理网段上。

#### 三、Lvs DR模式配置

综合上面的分析，我们可以得出结论，DR模式性能效率比较高，安全性很高，因此一般公司都推荐使用DR模式，我们这里也配置了DR模式
实现Lvs+Nginx集群

我们准备3台机器

```
1:192.168.5.128 (DS) （对外提供服务）
2:192.168.5.129 (RS) （处理真实服务）
3:192.168.5.130 (RS) （处理真实服务）
```

VIP:192.168.5.150

##### 3.1vip配置

关闭网络配置管理器(每台机器都要做)

systemctl stop NetworkManager
systemctl disable NetworkManager

配置虚拟ip(vip)

在 /etc/sysconfig/network-scripts创建文件ifcfg-ens33:1,

cp ifcfg-ens33 ifcfg-ens33:1 内容如下

```
BOOTPROTO=static
DEVICE=ens33:1
ONBOOT=yes
IPADDR=192.168.5.150
NETMASK=255.255.255.0
```

重启网络服务:service network restart

##### 3.2 RS绑定虚拟vip

在 /etc/sysconfig/network-scripts创建文件ifcfg-lo:1

cp ifcfg-lo ifcfg-lo:1,vi ifcfg-lo:1 需要更改内容如下

```
DEVICE=lo:1
IPADDR=192.165.5.150
NETMASK=255.255.255.255
```

执行刷新:ifup lo:1

查看:ip addr

##### 3.3LVS集群管理工具安装（DS服务器安装即可）

ipvsadm用于对lvs集群进行管理，需要手动安装

安装命令

```
yum install ipvsadm
```

版本查看

ipvsadm -Ln

![image-20210529224309827](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210529224309827.png)

##### 3.4地址解析协议

arp_ignore和arp_announce参数都和ARP协议相关，主要用于控制系统返回arp响应和发送arp请求时的动作。这两个参数很重要，特别是在LVS的DR场景下，它们的配置直接影响到DR转发是否正常。

arp-ignore: arp_ignore参数的作用是控制系统在收到外部arp请求时,是否要返回arp响应（0~8，2~8用的很少）

DS新增如下配置

配置文件: /etc/sysctl.conf,将如下文件拷贝进去

```
net.ipv4.conf.all.arp_ignore = 1
net.ipv4.conf.default.arp_ignore = 1
net.ipv4.conf.lo.arp_ignore = 1
net.ipv4.conf.all.arp_announce = 2
net.ipv4.conf.default.arp_announce = 2
net.ipv4.conf.lo.arp_announce = 2
```

刷新配置
sysctl -p

RS中新增如下配置

```
添加路由:此时如果无法识别route,需要安装相关工具，yum install net-tools
route add -host 192.168.5.150 dev lo:1
添加了一个host地址，目的是用于接收数据报文，接收到了数据报文后交给lo:1处理。
(防止关机失效，需要将上述命令添加到/etc/rc.local中)
添加完host后，可以查看一下: route -n ,能明显看到效果
```

##### 3.5集群配置

ipvsadm命令详解

```
ipvsadm -A:用于创建集群
ipvsadm -E:用于修改集群
ipvsadm -D:用于删除集群
ipvsadm -C:用于清楚集群数据
ipvsadm -R:用于重置集群配置规则
ipvsadm -S:用于保存修改的集群规则
ipvsadm -a:用于添加一个rs节点
ipvsadm -e:用于修改一个rs节点
ipvsadm -d:用于删除一个rs节点
```

添加集群TCP服务地址:(外部请求由该配置指定的VIP处理)

```
ipvsadm -A -t 192.168.5.150:80 -s rr
参数说明
-A:添加集群配置
-t：TCP请求地址(VIP)
-s: 负载均衡算法
```

负载均衡算法

| 算法 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| rr   | 轮询算法，它将请求依次分配给不同的rs节点，也就是RS节点中均摊分配，这种算法简单,但只适合于RS节点处理性能差不多的情况 |
| wrr  | 加权轮询调度，它将依据不同RS的权值分配任务。权值较高的RS将优先获得任务，并且分配到的连接数将比权值低的RS更多。相同权值的RS得到相同数目的连接数。 |
| Wlc  | 加权最小连接数调度，假设各台RS的全职依次为Wi,当前tcp连接数依次为Ti,依次去Ti/Wi为最小的RS作为下一个分配的DS |
| Dh   | 目的地址哈希调度，以目的地址为关键字查找一个静态hash表来获得需要的RS |
| SH   | 源地址哈希调度以源地址为关键字查找一个静态hash表来获得需要RS |
| Lc   | 最小连接数调度，IPVS表存储了所有活动连接。LB会比较将连接请求发送到当前连接最少的RS |
| Lblc | 基于地址的最小连接数调度:将来自同一个目的地址的请求分配给同一台RS，此时这台服务器是尚未满负荷的。否则就将这个请求分配给连接数最小的RS，并以它作为下一次分配的首先考虑 |

配置rs(2个)节点

```
ipvsadm -a -t 192.168.5.150:80 -r 192.168.5.129:80 -g
ipvsadm -a -t 192.168.5.150:80 -r 192.168.5.130:80 -g

参数说明:
-a: 给集群添加一个节点
-t: 指定vip地址
-r: 指定真是服务器地址
-g: 表示LVS的模式为dr模式

集群列表客户端请求数据和TCP通信数据会持久化保存，为了看到更好效果，时间设置成2秒保存
ipvsadm --set 2 2 2
查看列表
ipvsadm -Lnc
```

