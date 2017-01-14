title: "Android开发——-Android Source&SDK的国内镜像"
toc: true
date: 2015-07-24 23:21:39
tags:
- Android
categories: 设计开发
---
# 前言
谷歌被墙，在下载安卓源码和SDK时，各种没有进度。这时，就要感谢国内镜像站了。

# Android Source

## 清华大学TUNA镜像源
https://aosp.tuna.tsinghua.edu.cn/
https://wiki.tuna.tsinghua.edu.cn/MirrorUsage/android

## 为什么要下载Android Source
研究Android尤其是Android系统核心或者是驱动的开发，首先需要做的就是克隆建立本地Android Source版本库。

<!--more-->

## AOSP、AOKP、CM

AOSP是“Android Open-Source Project”的缩写，中文名称为Android开放源代码项目。大家都知道Android是开源操作系统，所以Google每发布一个Android版本，都会给开源社区发放对应版本的源代码，也就是我们所说的AOSP ROM，这可以称得上是最为纯净的Android系统。可以用类比来让大家更清楚一些，例如国内多数盗版的Windows系统，几乎都是基于微软的MSDN制作，AOSP ROM即等相当于微软MSDN母盘的角色。

AOKP是“Android Open-Source Kang Project”，比AOSP多了一个“Kang”。在Android社区中，Kang表示这是一个被别人定制过的ROM，是一个来自民间的ROM。

CM是CyanogenMod的简称，Cyanogen团队是全球最大的第三方ROM编译团队，覆盖机型范围特别广，国内知名的ROM例如MIUI、锤子ROM都是基于CM实现的。严格意义上来说，CM ROM属于AOKP的范畴。CM ROM虽然一直遵从原生Android，但只有Google官方的才算真正的AOSP。

## 为什么使用Repo
Android使用Git作为代码管理工具，开发了Gerrit进行代码审核以便更好的对代码进行集中式管理，还开发了Repo命令行工具，对Git部分命令封装，将百多个Git库有效的进行组织。

AOSP是由许许多多有Git管理的项目组成的，例如，在Android4.2中，就包含了329个项目，每一个项目都是一个独立的Git仓库。这意味着，如果我们要创建一个AOSP分支来做新的feature开发，那么就需要到每一个子项目去创建对应的分支。这显然不能手动地到每一个子项目里面去创建分支，必须要采用一种自动化的方式来处理。这些自动化处理工作就是由Repo工具来完成的。当然，Repo工具所负责的自动化工作不只是创建分支那么简单，查看分支状态、提交代码、更新代码等基础的Git操作都可以用Repo来替代完成。


## 下载安装配置Repo
假设在VirtualBox Ubuntu环境下，打开终端：
`mkdir ~/bin`
`PATH=~/bin:$PATH`
`git clone git://aosp.tuna.tsinghua.edu.cn/android/git-repo.git`
`cp git-repo/repo ~/bin/`
`gedit ~/bin/repo`
在该文件中修改
```
REPO_URL = 'git://aosp.tuna.tsinghua.edu.cn/android/git-repo'
```

## 初始化Repo
`mkdir anroid`
`cd android`
`git config --global user.email "voidking@qq.com"`
`git config --global user.name "voidking"`
`repo init -u git://aosp.tuna.tsinghua.edu.cn/android/platform/manifest -b android-5.0.2_r1`
这一步大概有一分多钟就完成了。
如果需要下载其他分支将android-5.0.2_r1改成其他分支名称就可以了。

Android官网详细地介绍了当前Android的各个版本名称、Version、对应的API Level、Branch TAG、以及Supported devices，该链接地址如下：
http://source.android.com/source/build-numbers.html

如果谷歌北墙，请查看https://github.com/Jhuster/AOSP/tree/master/documents 的build-numbers.html，雷锋哥哥在这里备份了一份。


## 开始下载
`repo sync`
然后就是漫长的等待了，大约32G的数据，有点大！

## 使用
1、查看源码
- 在Ubuntu下，查看没有问题。
- 如果想要在Windows下查看，那么不妨建立一个Ubuntu和Windows的共享文件夹，具体方法见《VirtualBox Ubuntu共享文件夹》。


2、开发
按照曾经修改Linux内核的经验，小编推测，Android的内核开发最好在Linux下进行；Mac是程序猿的最爱，估计也可以；各种编译工具和依赖包，Windows折腾不起哇！


# Android SDK

## 北京化工大学镜像站
http://ubuntu.buct.edu.cn/
http://ubuntu.buct.edu.cn/android/repository/

## We - 开源镜像站
http://mirrors.neusoft.edu.cn/
http://mirrors.neusoft.edu.cn/android/repository/

## 配置
1、启动Android SDK Manager，Tools，Options。
2、在弹出的Android SDK Manager-Settings中

- HTTP Proxy Server填入`mirrors.neusoft.edu.cn`
- HTTP Proxy Port填入`80`
- 选中`Force https://... sources to be fetched using http://...`复选框。

3、单击Close返回主界面，Packages，Reload。

# 后记
Android Source，前期用不上，小编还没有深入底层的打算。Android的SDK，这是个好东西！最喜欢里面的官方文档和例子，实在是学习Android的最好资料啊！


# 参考文档
同步、更新、下载Android Source & SDK from 国内镜像站
http://www.cnblogs.com/bluestorm/p/4419051.html

Android A to Z: What is the AOSP?
http://www.androidcentral.com/android-z-what-aosp

Android源代码仓库及其管理工具Repo分析
http://blog.csdn.net/luoshengyang/article/details/18195205

Android源码仓库和Repo工具使用
http://www.2cto.com/kf/201409/336722.html

windows系统中国国内镜像网站上用repo下载Android5.0源码
http://jileniao.net/post-156.html

repo用法详解
http://blog.csdn.net/sunweizhong1024/article/details/8055372

关于Android Repo
http://www.cnblogs.com/hongzg1982/articles/2101980.html

Android内核开发：源码的版本与分支详解
http://ticktick.blog.51cto.com/823160/1654759
http://www.tuicool.com/articles/RjeEZb