---
title: 在CentOS7上配置PHP运行环境
toc: true
date: 2016-08-23 13:13:16
tags:
- php
- centos
categories: 设计开发
---
# 前言
阿里云到期了，穷人买不起服务器，以后只能在虚拟机中练手了。VMware和VirtualBox都很好用，VMware可以安装MacOS，VirtualBox更轻量。

CentOS7安装到了VirtualBox中，下面，来学习配置一下PHP的运行环境。假设采用桥接，CentOS7的IP地址为`192.168.1.114`。

<!--more-->


# php环境配置
常用的PHP环境为LAMP和LNMP，这次小编选择LNMP，也就是Linux、Nginx、Mysql和PHP。
听说EZHTTP可以简化安装配置过程，尝试一下。

## 安装screen(可选)
由于编译安装Nginx Apache PHP MySQL等软件会花费比较长的时间，难免会出现由于网络意外中断而导致安装也中断了，所以为了避免此问题，可以用screen来安装。
`yum install -y screen`

## 安装unzip和wget
执行ezhttp安装程序，至少需要unzip及wget工具。
`yum install -y wget unzip`

## 安装git
`yum install -y git`

## 安装EZHTTP
```
git clone https://github.com/centos-bz/ezhttp.git
cd ezhttp
chmod +x start.sh
./start.sh
```
软件选择问题，参见书签中《使用EZHTTP安装LNMP(Nginx MySQL PHP) 》。
网络条件良好的话，一个小时左右就可以安装完成。

## 测试
在浏览器地址栏输入`http://192.168.1.114`，可以看到，EZHTTP已经安装成功。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-php/ezhttp.jpg?imageView2/0/w/600)

## 上传thinkphp
1、下载thinkphp，解压到thinkphp_3.2.3_full。
2、重命名thinkphp_3.2.3_full文件夹为thinkphp。
3、利用xftp上传thinkphp文件夹到`/home/wwwroot`目录下。

## 测试
在浏览器栏输入`http://192.168.1.114/thinkphp/`，可以看到，thinkphp已经可以访问。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-php/thinkphp.jpg?imageView2/0/w/600)


# 后记
再次启动centos，利用`ip add`命令查看到ip地址，在浏览器地址栏输入该ip地址。然后，神奇的事情发生了——网站无法访问！打开命令提示符，ping该ip，提示“无法访问目标主机”。

解决办法：
关闭centos防火墙，执行命令`systemctl stop firewalld.service`。


# 书签
在CentOS上搭建PHP服务器环境
http://www.cnblogs.com/liulun/p/3535346.html

EZHTTP使用教程 
https://www.centos.bz/forum/thread-69-1-1.html

EZHTTP安装前准备工作
https://www.centos.bz/forum/thread-50-1-1.html

使用EZHTTP安装LNMP(Nginx MySQL PHP) 
https://www.centos.bz/forum/thread-52-1-1.html

EZHTTP相关进程管理及目录位置
https://www.centos.bz/forum/thread-70-1-1.html
