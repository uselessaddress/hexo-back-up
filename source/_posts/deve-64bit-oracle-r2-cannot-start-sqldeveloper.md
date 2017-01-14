title: 64位Oracle11gR2无法启动sqldeveloper
date: 2015-04-29 16:49:05
tags: 
- 数据库
- Oracle 
- 问题
- sqldeveloper
categories: 设计开发
toc: true
---

### 详细问题描述
安装好Oracle11gR2之后，启动SOL Developer：开始，所有程序，Oracle-OraDb11g_home1，应用程序开发，SQL Developer。
第一次使用，需要选择与Oracle配套安装的jave.exe的路径，小编的安装路径为D:\Oracle\dbhome\jdk\bin\java.exe。
此时，错误出现了，Unable to find a java Virtual Machine
to point to a location of a java virtual machine,please refer to the oracle9i Jdeveloper Install guide(jdev\install.html)

### 解决思路
经百度，原来Oracle在制造64位版的时候没注意Oracle11gR2所带的SQL Developer是1.5.5.59.69版，不支持64位版的JDK，恰好64位Oracle带的JDK是64位的。
<!--more-->
知道了这个就好办了，下载安装一个32位的jdk，重新设置一下路径。

### 我的实际操作
在使用32位Oracle的小伙伴那里，从dbhome目录下，把整个jdk文件夹拷贝出来，重命名为jdk32。拷贝到自己电脑上的dbhome文件夹下。
然后，打开sqldeveloper.conf（路径为D:\Oracle\dbhome\sqldeveloper\sqldeveloper\bin\sqldeveloper.conf），修改`SetJavaHome D:\Oracle\dbhome\jdk`，为`SetJavaHome D:\Oracle\dbhome\jdk32`。
至此，SQL Developer已经可以正常使用了。

但是，初始的开始菜单中的SQL Developer，链接到的是一个bat文件，这个bat文件，又打开了sqldeveloper.exe。小编窃以为此步画蛇添足，而且打开了两个窗口，让我很不爽。
于是，小编果断删掉了开始菜单中的SQL Developer这个超链，然后右击Oracle-OraDb11g_home1下的应用程序开发，属性，复制下位置，打开此位置（C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Oracle - OraDb11g_home1），进入应用程序开发文件夹。
然后，把sqldeveloper.exe的快捷方式拷贝进刚才打开的应用程序开发文件夹。


### 题外记
Oracle11g的安装包压缩文件有两个，win64_11gR2_database_1of2.zip和win64_11gR2_database_2of2.zip，安装时，要把它们解压到同一个文件夹。解压完成，点击setup.exe，根据喜好设置一下，等待一会儿就安装成功了。

当年，第一次接触Oracle，使用的是Oracle10g，各种安装报错，泪流满面。年轻不懂事，不止安装了database，还安装了client。想想真是多余，用来学习，安装database就够了，咱又不连接远程服务器。哪怕连接远程服务器，使用database里的工具也足够了，比如sqldeveloper。
当年，还独立安装了一款PL/SQL Developer，解决完各种报错，微微一笑，这个工具真不错！如今，小编会毫不犹豫地会选择database里的自带工具！
一言以蔽之，Oracle的安装和使用更简单了。（侧面反映了小编对于Oracle的使用更加得心应手，哇哈哈哈）

### 参考文档
http://caogenit.com/caogenxueyuan/yingyongfangxiang/shujuku/2343.html