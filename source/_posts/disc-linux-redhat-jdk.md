title: RedHat企业版卸载jdk
date: 2013-07-19 09:27:14
tags: 
- Linux
- jdk
- RedHat
categories: 点滴发现
---
一、卸载jdk1.4
由于Redhat Enterprise Linux 5.6 中自带安装了jdk1.4.2的，所以在安装jdk1.6前我把jdk1.4.2的卸了，步骤如下：

打开终端输入 yum remove java
终端显示 Is this ok [y/N]:
输入y ，按回车。
终端显示 Complete! 此时jdk1.4已被卸了。

二、解压jdk1.7的安装包，配置环境变量
输入javac
报错：cannot restore segment prot after reloc: Permission denied
解决办法：这是因为SELINUX的问题，需要关闭SELINX，执行：/usr/sbin/setenforce 0 
再次输入javac，已经成功安装

