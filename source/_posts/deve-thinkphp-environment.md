---
title: ThinkPHP开发环境搭建
toc: true
date: 2016-08-15 16:15:01
tags:
- php
categories: 设计开发
---
# 前言
8月12号，抵达东北师范，开启了闭关修炼模式。入手的第一个项目，小太阳，有意思的名字。这是一个物业管理系统，分为业主端、物业端、CMS端，计划使用ThinkPHP框架来开发。PHP，好久不见，倍感亲切。

<!--more-->

# PHP运行环境
PHP运行，需要两个条件，一个是Apache容器，另一个是PHP程序。很多情况下，还需要Mysql数据库。
本次环境搭建中，我们使用集成环境WAMPServer，本环境包含了以上三个软件。省却了很多配置，很方便。其中，W代表Windows，A代表Apache，M代表Mysql，P代表PHP。

## 下载WAMPServer
WAMPSever官网：http://www.wampserver.com/
不翻墙的话，可能下载不下来，这里提供一个360网盘的下载链接：https://yunpan.cn/c6CkFLeumMZVv  访问密码 38f7

## MSVCR110
运行WAMPServer，报错：
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/thinkphp/msvcr110.jpg)

如果是64位系统，下载安装vcredist_x64.exe，即可解决该错误，32位同理下载vcredist_x86.exe。
微软官网下载：http://www.microsoft.com/zh-CN/download/details.aspx?id=30679

360网盘的下载链接：https://yunpan.cn/c6C2FxHGXTabm  访问密码 f641

## 查看运行效果
成功运行WAMPSever后，我们发现桌面右下角多了一个WAMPSever的logo。这时在浏览器地址栏中输入`http://localhost`，即可看到WAMP的一些信息。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/thinkphp/wamp.jpg?imageView2/0/w/700)

## phpmyadmin
在浏览器地址栏中输入`http://localhost/phpmyadmin/`，可以看到phpmyadmin的登录界面。默认用户名为root，密码为空。

# 下载安装ThinkPHP
## 下载
ThinkPHP官网：http://www.thinkphp.cn/
360网盘的下载链接：https://yunpan.cn/c6CvrUtM6C6X4  访问密码 ebd1

## 安装
1、解压到thinkphp_3.2.3_full。
2、重命名thinkphp_3.2.3_full文件夹为thinkphp。
3、剪切thinkphp文件夹到WAMP安装目录下的www文件夹中。比如我的WAMP安装目录为`D:\Server\wamp64`，那么就把thinkphp放到`D:\Server\wamp64\www`中。

## 查看运行结果
在浏览器地址栏中输入`http://localhost/thinkphp`，即可看到thinkphp的一些信息。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/thinkphp/thinkphp.jpg?imageView2/0/w/700)

# 后记
至此，ThinkPHP的开发环境搭建成功，接下来可以愉快地开发了！

附上另外几款常见的PHP集成环境：
1、AppServ
2、WAMPP
3、ServKit（原名PHPnow）
4、UPUPW

# 书签
PHP入门篇
http://www.imooc.com/learn/54

ThinkPHP3.2完全开发手册
http://document.thinkphp.cn/manual_3_2.html