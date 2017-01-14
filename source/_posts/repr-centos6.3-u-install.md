title: CentOS6.3的U盘安装方法
date: 2013-11-29 16:00:40
tags: 
- Linux
- CentOS
- 操作系统
categories: 精华转载
---
     最近要给服务器重装系统，由于使用dvd安装刻盘比较麻烦，所以决定采用U盘安装，U盘下安装CentOS/Red Hat比较麻烦，相对于Ubuntu来说要麻烦很多（Ubuntu只需要使用Universal USB Installer即可，非常简单），此方法经过本人亲测，真实有效。

这里选择在windows下使用UltraISO进行安装，建议使用bin DVD版的CentOS，不用live版本的，同时我们需要一个8GB及以上容量的U盘，因为完整版的安装包需要4G，加上启动引导部分，4G的U盘是不够的。这里简单介绍一下原理，首先使用UltraISO将DVD版的iso镜像写入U盘来制作启动盘，然后将完整的CentOS的DVD版安装iso复制到U盘根目录，待安装程序启动之后有一步会提示寻找安装镜像，选择U盘对应的目录就能找到。

具体步骤：
1.下载UltraISO，选择“文件”->“打开”，找到CentOS安装文件，我的为CentOS-6.3-x86_64-bin-DVD1.iso（iso格式的）
2.选择“启动”->“写入硬盘映像...”，“硬盘驱动器”选择要安装的U盘，“写入方式”选择“USB-HDD+”，在这之前最好将U盘格式化，默认的为FAT32，最后选择“写入”，再选择“是”即可，具体过程见下图
3.在U盘写入完毕之后，打开U盘，将Packages目录全部删除，其实那4G的安装镜像大部分都是相关系统软件以及服务的安装包，这里我们只是制作U盘引导系统安装，所以Packages目录不需要，删除之后再将CentOS-6.3-x86_64-bin-DVD1.iso复制到U盘的根目录即可。
![](http://voidking.qiniudn.com/@/imgs/linux/CentOS安装.jpg)
        将U盘插上电脑选择U盘启动之后就可以参考网上大量的安装步骤，不过这里有一点要注意的是`从U盘启动之后U盘被系统识别为/dev/sda而电脑的SATA硬盘被识别为/dev/sdb`，因此到了有一部让你选择安装镜像(install image)的时候请选择sda（即U盘），后面要求选择安装引导程序的时候请安装在SATA硬盘对应的设备名称上，否则如果选择安装在U盘上了之后拔掉U盘就不能启动系统了。

