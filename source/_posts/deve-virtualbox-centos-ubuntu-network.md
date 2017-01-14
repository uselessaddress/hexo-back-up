---
title: VirtualBox下CentOS7和Ubuntu16.04网络配置
toc: true
date: 2016-08-23 12:13:16
tags:
- php
- centos
- ubuntu
- 网络
- xshell
- xftp
categories: 设计开发
---
# 前言
开发中，经常需要用到虚拟机，虚拟中的网络配置，让小编头疼了很久。今天，以CentOS7和Ubuntu16.04为例，彻底解决这个问题。过程中，会穿插xshell和xftp的使用说明。

<!--more-->

# 桥接
## VirtualBox设置
选中CentOS，设置，网络，连接方式选择桥接网卡。

## CentOS的ip设置
1、启动CentOS，使用root用户登录。
2、查看ip：`ip add`。
3、编辑ifcfg-enp0s3，`vi /etc/sysconfig/network-scripts/ifcfg-enp0s3`，把ONBOOT=no改为ONBOOT=yes。
4、重启网络服务，`service network restart`。
5、再次查看ip：`ip add`。
![ip](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-php/ip.jpg?imageView2/0/w/600)

## CentOS的ssh设置
1、启动CentOS，打开终端，切换到root用户。
2、检查是否安装了ssh：`rpm -qa | grep ssh`
如果没有安装ssh，则执行命令：`yum install openssh-server`

3、启动ssh：`service sshd start`
如果提示:redirecting to /bin/systemctl start，那么改用命令：`/bin/systemctl start sshd.service`

4、检查是否已经成功启动：`netstat -antp | grep sshd `
如果看到如下信息，则表示启动成功。
![sshd](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-php/sshd.jpg?imageView2/0/w/600)

5、设置开机自启动：`chkconfig sshd on`

PS：利用右ctrl键来切换主机和虚拟机的鼠标，利用右ctrl+F来切换虚拟机全屏状态。

## xshell设置
1、文件，新建，主机填入`192.168.1.114`。
2、选择用户身份验证，输入用户名和密码。
3、点击确定，建立连接。

## xftp配置
1、文件，新建，主机填入`192.168.1.114`。
2、协议选择SFTP。
3、输入用户名和密码。
4、点击确定，建立连接。

# 改进
采用桥接方式，虚拟机中CentOS的IP地址是DHCP服务器动态分配的。所以每次使用前需要使用`ip add`命令，重新查询一下CentOS的IP地址，然后修改xshell和xftp中的主机地址。

当外部没有DHCP服务器时，虚拟机也需要拨号才能上网，很麻烦。于是，再次研究Virtualbox网络设置，盗图一张说明四种连接方式区别。

![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-php/compare.png)

比较简单的设计，是添加两张网卡。一张选择仅主机（Host-Only）适配器，用于宿主机和虚拟机之间的通讯，使宿主机可以访问虚拟机的服务；一张选择网络地址转换（NAT），用于分享宿主机网络给虚拟机，使虚拟机可以访问外网。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-php/host-only.jpg)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-php/nat.jpg)

下面详细说明一下四种连接方式的不同。
## NAT
NAT：Network Address Translation，网络地址转换
NAT模式是最简单的实现虚拟机上网的方式，你可以这样理解：

> Guest访问网络的所有数据都是由主机提供的，Guest并不真实存在于网络中，主机与网络中的任何机器都不能查看和访问到Guest的存在。

Guest可以访问主机能访问到的所有网络，但是对于主机以及主机网络上的其他机器，Guest又是不可见的，甚至主机也访问不到Guest。

虚拟机与主机的关系：只能单向访问，虚拟机可以通过网络访问到主机，主机无法通过网络访问到虚拟机。

虚拟机与网络中其他主机的关系：只能单向访问，虚拟机可以访问到网络中其他主机，其他主机不能通过网络访问到虚拟机。

虚拟机与虚拟机的关系：相互不能访问，虚拟机与虚拟机各自完全独立，相互间无法通过网络访问彼此。

## Bridged Adapter（网桥模式）

网桥模式，你可以这样理解：

> 它是通过主机网卡，架设了一条桥，直接连入到网络中了。因此，它使得虚拟机能被分配到一个网络中独立的IP，所有网络功能完全和在网络中的真实机器一样。

网桥模式下的虚拟机，你把它认为是真实计算机就行了。

虚拟机与主机的关系：可以相互访问，因为虚拟机在真实网络段中有独立IP，主机与虚拟机处于同一网络段中，彼此可以通过各自IP相互访问。

虚拟机于网络中其他主机的关系：可以相互访问，同样因为虚拟机在真实网络段中有独立IP，虚拟机与所有网络其他主机处于同一网络段中，彼此可以通过各自IP相互访问。

虚拟机与虚拟机的关系：可以相互访问，原因同上。

## Internal（内网模式）

内网模式，顾名思义就是内部网络模式：

> 虚拟机与外网完全断开，只实现虚拟机于虚拟机之间的内部网络模式。

虚拟机与主机的关系：不能相互访问，彼此不属于同一个网络，无法相互访问。

虚拟机与网络中其他主机的关系：不能相互访问，理由同上。

虚拟机与虚拟机的关系：可以相互访问，前提是在设置网络时，两台虚拟机设置同一网络名称。如上配置图中，名称为intnet。

## Host-only Adapter（主机模式）

主机模式，这是一种比较复杂的模式，需要有比较扎实的网络基础知识才能玩转。可以说前面几种模式所实现的功能，在这种模式下，通过虚拟机及网卡的设置都可以被实现。

我们可以理解为Guest在主机中模拟出一张专供虚拟机使用的网卡，所有虚拟机都是连接到该网卡上的，我们可以通过设置这张网卡来实现上网及其他很多功能，比如（网卡共享、网卡桥接等）。

虚拟机与主机的关系：默认不能相互访问，双方不属于同一IP段，host-only网卡默认IP段为192.168.56.X 子网掩码为255.255.255.0，后面的虚拟机被分配到的也都是这个网段。通过网卡共享、网卡桥接等，可以实现虚拟机于主机相互访问。

虚拟机与网络主机的关系：默认不能相互访问，原因同上，通过设置，可以实现相互访问。

虚拟机与虚拟机的关系：默认可以相互访问，都是同处于一个网段。

## 修正
采用主机模式，虚拟机与主机的关系，默认可以相互访问。因为，主机IP地址默认为192.168.56.1，虚拟机的IP地址为192.168.56.X。所以，理论上虚拟机和主机可以相互访问。亲测证明，虚拟机和主机确实可以互相访问。

# CentOS7相关
## 添加网络命令
CentOS7没有netstat 和 ifconfig命令，`yum install net-tools`。
CentOS7查看网络配置，`ifconfig -a`或者`ip add`，如果没有获取到IP地址，`vi /etc/sysconfig/network-scripts/ifcfg-enp0s3`，修改`ONBOOT=no`为`ONBOOT=yes`，然后`service network restart`。

## 修改时区
CentOS7修改时区，`cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
`。

## 设置静态IP

第一网卡设置为固定IP，ifcfg-enp0s3原配置：
```
TYPE=Ethernet
BOOTPROTO=dhcp
DEFROUTE=yes
PEERDNS=yes
PEERROUTES=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
IPV6_FAILURE_FATAL=no
NAME=enp0s3
UUID=459b17a4-fd16-4ea7-8a4b-9509b6e899e7
DEVICE=enp0s3
ONBOOT=yes
```

ifcfg-enp0s3修改为：
```
TYPE=Ethernet
BOOTPROTO=static
IPADDR=192.168.56.101
NETMASK=255.255.255.0
NM_CONTROLLED=no
DEFROUTE=yes
PEERDNS=yes
PEERROUTES=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
IPV6_FAILURE_FATAL=no
NAME=enp0s3
UUID=459b17a4-fd16-4ea7-8a4b-9509b6e899e7
DEVICE=enp0s3
ONBOOT=yes
```

# Ubuntu相关
## 修改root密码
修改root密码，`sudo passwd root`。

## 安装ssh服务
安装ssh服务，`apt-get install openssh-server`，如果提示找不到安装包，执行`apt-get update`。

允许以root用户通过ssh登录，`vi /etc/ssh/sshd_config`，找到：
```
# Authentication:
LoginGraceTime 120
PermitRootLogin prohibit-password
StrictModes yes
```
修改为：
```
# Authentication:
LoginGraceTime 120
#PermitRootLogin prohibit-password
PermitRootLogin yes
StrictModes yes
```

然后重启ssh服务，`service ssh restart`。

## 设置静态IP
查看网络配置，`ifconfig -a`，如果只显示一个网卡，执行`vi /etc/network/interfaces`。
interfaces原配置为：
```
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto enp0s3
iface enp0s3 inet dhcp
```

添加如下内容：
```
# The second network interface
auto enp0s8
iface enp0s8 inet dhcp
```

然后重启网络服务，`/etc/init.d/networking restart`。

第一网卡设置为固定IP：
```
# The primary network interface
auto enp0s3
iface enp0s3 inet static  
address 192.168.56.102 
netmask 255.255.255.0  
```

# 后记
以后不玩图形界面的linux了，专注于服务器配置。

# 书签
如何开启Centos6.4系统的SSH服务
http://jingyan.baidu.com/article/3ea51489f9efbf52e61bba05.html

通过SSH连接VirtualBox中的CentOS
http://www.2cto.com/os/201212/172712.html

VirtualBox + CentOS 虚拟机网卡配置
http://my.oschina.net/duangr/blog/182541

CentOS 7 网络配置
http://simonhu.blog.51cto.com/196416/1588971

centOS7在VirtualBox中装好后的网络连接问题
http://jingyan.baidu.com/article/456c463b4a98460a5931444c.html

快速理解VirtualBox的四种网络连接方式
http://www.cnblogs.com/york-hust/archive/2012/03/29/2422911.html

CentOS 7 修改时区
http://blog.csdn.net/robertsong2004/article/details/42268701

centOS7在VirtualBox中装好后的网络连接问题
http://jingyan.baidu.com/article/456c463b4a98460a5931444c.html

virtualBox下Centos系统扩展磁盘空间详细教程
http://blog.csdn.net/timecolor/article/details/48468377

virtualbox中ubuntu硬盘扩展，Gparted无法进入图形界面
http://blog.sina.com.cn/s/blog_7149fc900102wvxh.html








