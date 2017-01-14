title: ubuntu下NTFS文件系统权限问题
date: 2013-10-29 21:22:49
tags:
- Linux
- Ubuntu
- 文件系统
categories: 点滴发现
---

修改 ubuntu NTFS 文件系统下没有执行权限的问题
由于NTFS本身的特殊性，不能对其分区的文件权限进行修改，无论是suodo还是root都没有用。

安装以下两个插件解决问题：

sudo apt-get install ntfs-3g       //这个12.04已经有了。
sudo apt-get install ntfs-config   //这个是个图形界面的NTFS权限配置程序。

打开ntfs-config后，我把权限全开了。

再看NTFS目录下的所有文件，权限都有了。不过还是不能用chmod命令来修改。

同时，ntfs-config可以帮助用户自动挂载所有硬盘分区：

sudo ntfs-config      //自动挂载所有分区

实际就是在/etc/fstab中添加相应的挂载信息，如不需要，可以直接删掉，重启后就不再自动挂载了。

附：查看硬盘分区UUID的命令：
sudo blkid
