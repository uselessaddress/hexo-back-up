---
title: 翻墙教程
toc: true
date: 2016-10-26 20:50:21
tags:
- 翻墙
- 谷歌
- GFW
categories: 设计开发
---
# 前言
> 防火长城（英文名称Great Firewall of China，简写为Great Firewall，缩写GFW），也称中国防火墙或中国国家防火墙。

我大清城墙太厚，想要看一看外面的风景，必须得翻墙（架梯子）。
在谷歌寻找资料，安装Chrome插件，去YouTube看看视频，到Twitter刷刷动态。。。

<!--more-->

# 软件
翻墙软件，跪了一波又一波（详见维基百科）。当前可用的翻墙软件，首推lantern。建议买个专业版，因为免费版不稳定，而且每月限定800M流量。一年180元，两年320元，也算是比较实惠的价格了。

# VPN
首先来个科普：
> 虚拟专用网（英语：Virtual Private Network，简称VPN），是一种常用于连接中、大型企业或团体与团体间的私人网络的通讯方法。虚拟私人网络的讯息透过公用的网络架构（例如：互联网）来传送内联网的网络讯息。它利用已加密的通道协议（Tunneling Protocol）来达到保密、发送端认证、消息准确性等私人消息安全效果。这种技术可以用不安全的网络（例如：互联网）来发送可靠、安全的消息。需要注意的是，加密消息与否是可以控制的。没有加密的虚拟专用网消息依然有被窃取的危险。

> 以日常生活的例子来比喻，虚拟专用网就像：甲公司某部门的A想寄信去乙公司某部门的B。A已知B的地址及部门，但公司与公司之间的信不能注明部门名称。于是，A请自己的秘书把指定B所属部门的信（A可以选择是否以密码与B通信）放在寄去乙公司地址的大信封中。当乙公司的秘书收到从甲公司寄到乙公司的信件后，该秘书便会把放在该大信封内的指定部门信件以公司内部邮件方式寄给B。同样地，B会以同样的方式回信给A。

> 在以上例子中，A及B是身处不同公司（内部网路）的计算机（或相关机器），通过一般邮寄方式（公用网络）寄信给对方，再由对方的秘书（例如：支持虚拟专用网的路由器或防火墙）以公司内部信件（内部网络）的方式寄至对方本人。请注意，在虚拟专用网中，因应网络架构，秘书及收信人可以是同一人。许多现在的操作系统，例如Windows及Linux等因其所用传输协议，已有能力不用通过其它网络设备便能达到虚拟专用网连接。


在年轻时，小编对于免费有一种极致的追捧。后来慢慢意识到，很多时候，免费意味着牺牲体验，还会面临各种不可预知的问题。所以，在能力承受范围之内的服务，我会选择购买。比如域名，比如服务器，比如VPN，比如内网穿透，比如爱奇艺会员等等。

本次推荐的VPN，都是收费的。按照小编喜好，排序为HustVPN、多态、ExpressVPN。书签中有它们的官网链接，可以按需购买。

实际上，很多VPN提供商，号称提供VPN服务，但是并不是通过VPN的机制实现的翻墙。比如HustVPN，实际上是利用shadowsocks软件。也许是突破网络封锁技术中，大家最熟悉VPN，所以提供商们挂了个名头，便于被检索。也许是历史原因，大家一想到VPN就想到翻墙，VPN成了“梯子”的代名词。

# 自搭梯子
再来个科普：
> 云服务器和虚拟主机、vps都是空间产品，虚拟主机是最早研发出来的基础空间产品，VPS是在虚拟主机的基础上衍生出来的拥有独立IP地址的配置更好的虚拟空间，云服务器和VPS最为相似，同样是以虚拟主机为基础研发出来的，在性能方面要比VPS更强大。

自搭梯子，前提条件是要有一台国外的服务器，各种教程中推荐VPS主机，小编从众，也使用VPS。亚马逊aws有个一年的免费试用，不过注册费劲，验证费劲，遂放弃。收费VPS，推荐三个：搬瓦工、Host1Plus、Vultr。

## 购买搬瓦工
最低配版，256RAM的那个，就够了，一个月2.99dollar，约人民币20元。年费更加便宜，一年130元，等HustVPN到期，以后就用搬瓦工了。

## 设置密码
首先，进入KiwiVM Contrl Panel。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/over-the-wall/service.jpg)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/over-the-wall/kiwivm.jpg)
点击Root shell-interactive，我们可以看到一个网页版的shell界面。输入`passwd`，两次输入密码，即可设置root密码。

或者，直接点击Root password modification修改也可以。

## xshell登录
打开xshell，填入ip、ssh端口号、用户名root和密码，即可连接到搬瓦工服务器。

## 搭建shadowsocks
> 如果你在VPS上搭建了VPN并且经常使用，尤其是OpenVPN，请立即停止使用。VPN协议特征明显，GFW可以非常容易的检测到，从而盯上你的IP，轻则限速，重则彻底屏蔽。常见VPN协议根据易受干扰的程度从大到小依次为：OpenVPN > PPTP > L2TP > IPSec，尤其是OpenVPN，GFW已经可以实现对其定点清除(同样遭此待遇的还有SSH翻墙)。如果你想让自己VPS的IP快速报废，那么就请尽情的使用搬瓦工的控制面板搭建OpenVPN吧。重要提醒：在不明所以的情况下尽量不要在自己的VPS上搭建其他杂七乱八的翻墙服务尤其是一些早已过时和落后的翻墙方式，翻墙手段宜新不宜旧，只搭一个Shadowsocks是最能保证你翻墙效果和服务器稳定的好策略。

KiwiVM提供OpenVPN Server和Shadowsocks Server安装，非常贴心，在此，我们选择Shadowsocks Server。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/over-the-wall/shadowsocks.jpg)
点击Install Shadowsocks Server，几秒钟即可安装完成。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/over-the-wall/shadowsocks2.jpg)

## 使用shadowsocks
下载shadowsocks客户端，输入服务器IP、端口号、密码，点击确定，右键启用系统代理。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/over-the-wall/client.jpg)
亲测可用，很流畅，可以与HustVPN相媲美。

至此，已经大功告成了。但是，还有改进的余地——多用户shadowsocks。虽然使用同一个端口，同一个密码，也可以多用户使用，但这不方便管理。下面，我们来实现真正的多用户。

## 多用户
1、先在KiwiVM上卸载掉Shadowsocks Server（因为它是单用户版本），就像安装它一样简单。

2、使用xshell登录搬瓦工服务器。

3、下载安装多用户版本shadowsocks：
```
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-go.sh
chmod +x shadowsocks-go.sh
./shadowsocks-go.sh 2>&1 | tee shadowsocks-go.log
```
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/over-the-wall/xshell.jpg)


4、查看运行状态`/etc/init.d/shadowsocks status`，正常情况下会出现“Shadowsocks-go (pid 776) is running...”。

5、修改配置文件（/etc/shadowsocks/config.json）：
```
cp config.json config.json_bak
vim config.json
```

原config.json内容如下：
```
{
    "server":"0.0.0.0",
    "server_port":8989,
    "local_port":1080,
    "password":"voidking",
    "method":"aes-256-cfb",
    "timeout":600
}
```

修改后config.json内容如下：
```
{
    "server":"0.0.0.0",
    "port_password":{
         "8989":"voidking",
         "9001":"woaixuexi",
         "9002":"woaixuexi",
         "9003":"woaixuexi",
         "9004":"woaixuexi"
    },
    "method":"aes-256-cfb",
    "timeout":600
}
```

6、重启shadowsocks`/etc/init.d/shadowsocks restart`。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/over-the-wall/success.jpg)

哦了，亲测可用。可以愉快地多用户使用了，还可以出售哦，哇咔咔！

PS：卸载命令`./shadowsocks-go.sh uninstall`。

# 域名绑定
登录万网、godaddy或其他域名服务平台，添加解析，记录类型选择A，记录值填入VPS主机的IP地址即可。

至此，一个几乎完美的梯子搭建完成。

# 后记
最后，附上四个平台（windows、mac、android、ios）下对应的shadowsocks客户端，需要自取：
链接：http://pan.baidu.com/s/1i5BIyaD 密码：buzd

# 书签
Lantern官网
https://getlantern.org/

lantern源码
https://github.com/getlantern/lantern

[虚拟专用网-维基百科](https://zh.wikipedia.org/wiki/%E8%99%9B%E6%93%AC%E7%A7%81%E4%BA%BA%E7%B6%B2%E8%B7%AF)

ExpressVPN: The Fastest VPN Service on Earth
https://www.expressvpn.com/

HustVPN全球领先VPN服务提供商 - HustVPN
https://www.hustvpn.com/

多态
https://duotai.org/

Shadowsocks
https://shadowsocks.com/

搬瓦工
https://bwh1.net/

Host1Plus
https://www.host1plus.com/vps-hosting/

租用服务器自己搭建VPN，轻松翻墙
http://www.yunaw.com/book/111.html
http://blog.sina.com.cn/s/blog_c458907d0102w5ky.html

教你一步一步自己搭梯子（1）——服务器篇
https://www.douban.com/note/534163924/

如何在 VPS 上搭建 VPN 来翻墙
http://www.jianshu.com/p/2f51144c35c9/comments/1431428

翻墙路由器的原理与实现
http://netsecurity.51cto.com/art/201601/504141.htm

GFW技术评论
http://gfwrev.blogspot.com/

云服务器、虚拟主机和VPS的区别是什么
http://www.west.cn/cms/news/cloud/2015-06-10/2756.html

[自搭梯子shadowsocks科学上网](http://www.mianao.info/2015/01/17/%E8%87%AA%E6%90%AD%E6%A2%AF%E5%AD%90shadowsocks%E7%A7%91%E5%AD%A6%E4%B8%8A%E7%BD%91/)

Shadowsocks的多用户配置
http://everet.org/shadowsocks.html

搬瓦工bandwagonhost搭建多用户Shadowsocks
http://www.luanzun.com/136.shtml

写给非专业人士看的 Shadowsocks 简介
http://vc2tea.com/whats-shadowsocks/

GFW BLOG（功夫网与翻墙）
http://www.chinagfw.org/

