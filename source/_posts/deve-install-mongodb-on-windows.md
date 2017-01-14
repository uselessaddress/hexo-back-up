---
title: 在Windows上安装mongodb
toc: true
date: 2016-06-15 16:02:21
tags: 
- mongodb
- 数据库
categories: 设计开发
---

# 下载mongodb
mongodb官网 https://www.mongodb.com/download-center#community ，如果下载msi版本，直接按照提示安装即可。

如果下载的是zip版本，则往下接着看。

<!--more-->

# 新建文件和文件夹
1、解压安装包到喜欢的位置，小编解压到`D:\Server`目录下，重命名解压出来的文件夹为mongodb。

2、在mongodb文件夹下，新建文件夹data；在data下新建文件夹db和log；在log下新建文件mongodb.log。

3、在mongodb文件夹下，新建文件mongo.config，内容如下：
```
dbpath=D:\Server\mongodb\data\db
logpath=D:\Server\mongodb\data\log\mongodb.log  
```

# 安装命令
使用管理员打开cmd，输入命令：
```
sc create mongodb binPath="D:\Server\mongodb\bin\mongod.exe --service --config=D:\Server\mongodb\mongo.config"
```

至此，安装结束。使用`net start mongodb`，检查能否正常启动。启动后，使用`mongo`，检查能否正常连接。

# 开机自启动
右击计算机，管理，服务，右击mongodb，属性，启动类型设置自动。

# 后记
顺便推荐个mongodb可视化管理工具——Robomongo，用起来很顺手。
官方地址：https://robomongo.org/
