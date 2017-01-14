---
title: 搭建自己的云盘服务
toc: true
date: 2016-10-26 20:51:50
tags:
- owncloud
- 云盘
categories: 设计开发
---
# 前言
> 继华为、迅雷、新浪，115等网盘之后，近日360云盘也宣布关闭个人云存储服务。这场发起于2013年的个人网盘大战至今，排名靠前的常用网盘已经只剩下百度云盘和腾讯微云，真是令人唏嘘不已……

360云盘即将停止服务，所有用户数据将只保存到明年2月1日，可怜我的38T数据！！！首先，没有硬盘存储这么多数据，只能选择下载；其次，大家争相下载，360云盘限速，怎一个慢字了得！

考虑到别人的云盘不靠谱，小编决定自己搭建一个。

<!--more-->

# ubuntu安装owncloud
1、安装lamp-server，`tasksel install lamp-server`，报错tasksel: apt-get failed (100)。就知道不会一帆风顺，先更新个软件源，`apt-get update`，然而，接着报错：
E: Could not get lock /var/lib/dpkg/lock - open (11: Resource temporarily unavailable)
E: Unable to lock the administration directory (/var/lib/dpkg/), is another process using it?

删除两个文件：
```
rm /var/cache/apt/archives/lock
rm /var/lib/dpkg/lock
```

再次更新软件源，`apt-get update`，再次安装lamp-server，`tasksel install lamp-server`。

2、下载安装owncloud
```
wget -nv https://download.owncloud.org/download/repositories/9.1/Ubuntu_16.04/Release.key -O Release.key

apt-key add - < Release.key

sh -c "echo 'deb http://download.owncloud.org/download/repositories/9.1/Ubuntu_16.04/ /' > /etc/apt/sources.list.d/owncloud.list"

apt-get update

apt-get install owncloud
```

3、访问owncloud
访问`http://192.168.56.102/owncloud`，进入owncloud初始页面。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/owncloud/set.jpg)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/owncloud/app.jpg)

安装到这一步就完成了，已经可以通过服务器使用ownCloud的服务来存储同步分享文件了。

# 绑定域名
在/etc/apache2/sites-available文件夹下添加新的虚拟主机配置文件，备份000-default.conf文件，然后在其中添加：
```
<VirtualHost *:80>
  ServerName yun.voidking.com
  DocumentRoot /var/www/owncloud
  <IfModule mod_headers.c>
    Header always set Strict-Transport-Security "max-age=15552000; includeSubDomains; preload"
  </IfModule>
</VirtualHost>
```
然后域名解析yun.voidking.com到服务器ip地址。

# 安装插件
ubuntu安装ftp服务，`apt-get install vsftpd`。
`vi /etc/vsftpd.conf`，启用如下配置：

```
write_enable=YES
local_umask=022
utf8_filesystem=YES
```
然后`service vsftpd restart`。

下载owncloud的插件，下载的压缩包解压到/var/www/owncloud/apps，然后在浏览器中打开owncloud，选择应用页面，启用插件。
离线下载：https://apps.owncloud.com/content/show.php/ocDownloader+%28NG%29?content=169974

在线看视频：https://apps.owncloud.com/content/show.php/Video+Viewer+Plus?content=174666

# 书签
云盘一个个倒下怎么办？无需编码，手把手教你搭建至尊私享云盘
https://zhuanlan.zhihu.com/p/23156514

天下没有免费的午餐！是时候搭建起自己的云盘服务了
https://zhuanlan.zhihu.com/p/23179293

owncloud-files
http://download.owncloud.org/download/repositories/9.1/owncloud/

Installation Wizard
https://doc.owncloud.org/server/9.1/admin_manual/installation/installation_wizard.html

ownCloud Applications
https://apps.owncloud.com/

客户端
https://owncloud.org/install/#install-clients