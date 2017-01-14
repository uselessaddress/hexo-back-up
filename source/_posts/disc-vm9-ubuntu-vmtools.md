title: VM9下ubuntu安装vmtools
date: 2013-11-23 10:36:58
tags: Linux
categories: 点滴发现
---
13年的时候写的一篇文章，那时域名还是imsnail.com。

本文适合linux初学者

1.右击“ubuntu”，选择安装Vmware tools
![](http://voidking.qiniudn.com/@/imgs/linux/ubuntu/VM9下ubuntu安装vmtools106.png)

2.设备（CD-ROM）中会出现Vmware tools
![](http://voidking.qiniudn.com/@/imgs/linux/ubuntu/VM9下ubuntu安装vmtools137.png)

3.选中里面的压缩文件，复制到“主文件夹”（为了下面的操作方便），解压

4.ctrl+ait+t，打开终端

5.输入“cd”，回车（为了确保进入了自己的家目录）
<!--more-->
6.输入“su”，回车，输入root用户密码，回车（这步是为了切换到root用户）
![](http://voidking.qiniudn.com/@/imgs/linux/ubuntu/VM9下ubuntu安装vmtools263.png)

7.输入“ls -l”，回车，会看到文件列表中出现了vmware-tools-distrib这个文件夹 

8.输入“cd vmware-tools-distrib”，回车

9.输入“ls -l”，回车，会看到vmware-install.pl这个文件

10.输入“./vmware-install.pl”，回车

11.接下来一路回车

12.等到再也没有提示“yes”或“no”时，关闭窗口即可。

13.再次右击“ubuntu”，你会发现“安装vmware tools”的提示变成了“重新安装vmware tools”，至此，大功告成！

14.检验一下：重启虚拟机，从主机复制一个文件
![](http://voidking.qiniudn.com/@/imgs/linux/ubuntu/VM9下ubuntu安装vmtools557.png)
然后粘贴到虚拟机
![](http://voidking.qiniudn.com/@/imgs/linux/ubuntu/VM9下ubuntu安装vmtools568.png)
![](http://voidking.qiniudn.com/@/imgs/linux/ubuntu/VM9下ubuntu安装vmtools570.png)


