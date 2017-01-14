title: "VirtualBox Ubuntu共享文件夹"
toc: false
date: 2015-06-22 14:37:11
tags:
- Ubuntu
- Linux
categories: 点滴发现
---
### 设置步骤
1、设置，共享文件夹，添加，选择共享文件夹路径（假设为`E:\VirtualBoxShare`，对应共享文件夹名称为`VirtualBoxShare`），注意此时不要勾选自动挂载。
2、启动Ubuntu，打开Terminal。
3、`cd /mnt/`
4、`sudo mkdir shared`
5、`sudo mount -t vboxsf VirtualBoxShare /mnt/shared/`
6、`cd shared`，`ll`
<!--more-->

### 卸载
如果想卸载，可运行命令：
`sudo umount -f /mnt/shared`

### 参考文档
virtualbox+ubuntu设置共享文件夹
http://www.cnblogs.com/huanghuang/archive/2011/09/23/2185968.html

------
virtualbox中ubuntu和windows共享文件夹设置
http://www.cnblogs.com/linjiqin/p/3615477.html
