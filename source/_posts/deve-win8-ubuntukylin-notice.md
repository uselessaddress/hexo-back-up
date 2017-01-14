title: Win8+UbuntuKylin注意事项
date: 2015-05-29 22:30:22
tags:
- 操作系统
- win8
- Ubuntu
- 双系统
categories: 设计开发
---
## Win8+UbuntuKylin注意事项

### 前言
接近一年半没有换过操作系统（Win7），各种大型软件，各种系统后台服务，怎一个牛逼了得！近期安装的Oracle，成功地再次拖慢了系统速度，终于达到了小编的忍耐极限！做完了所有实验，果断换个系统！

忙碌了一天，安装了Win8+Ubuntu Kylin，并且对它们进行了基本配置和简单优化。在此简单记录下过程中遇到的问题，以及解决办法，分享给大家。

### 安装方法
详细方法请见《安装系统》系列，这里简单叙述一下。
1、准备两个U盘，一个使用U大师做成启动盘（U盘1），另一个利用Universal-USB-Installer-\*.exe做成Ubuntu安装盘（U盘2）。
2、使用U盘1启动，进入WinPe。
3、分区，分成四个，然后删除最后一个分区（等会儿安装Ubuntu）。
4、安装Win8。
5、使用U盘2启动，进入Ubuntu的安装界面。
6、在空白分区上分区，swap分区4096MB（Logical），/分区占用剩下的全部空间（Primary）。
7、安装Ubuntu。

### Win8不见了
安装完成，小编发现，开机不会出现启动选项，直接进入Ubuntu。应该是因为安装Ubuntu时重写了引导扇区，没关系，我们只要在Ubuntu系统下执行`sudo update-grub`。该命令会自动检测启动项，重新生成/boot/grub/grub.cfg文件。重新启动，已经多出了Win8的启动选项。

很多小伙伴认为，必须要GPT分区，必须要EFI分区。其实没必要，小编就是传统的MBR分区，毫无问题。

### Win8初始软件
卸载完自带的软件，接下来就要个性化定制了。先美化一下桌面，设置一下头像密码等，需要的软件有：
- 搜狗输入法
- Notepad++
- JDK
- Eclipse
- WPS
- FastStone Capture
- Photoshop
- 360安全卫士
- 360极速浏览器
（IE不好用，Firefox笨重且历史记录太丑，Chrome插件受限，UC连主页都可以设置）
- 360云盘
- 百度云盘
- QQ
- QQ飞车
- ADSafe
- 其他。。。
<!--more-->

### Ubuntu初始软件
- 搜狗输入法
- JDK
- Eclipse
- WPS
- vim。vim输入法需要更新，否则上下左右键不可用，更新命令：
`sudo apt-get remove vim-common`，
`sudo apt-get install vim`。

### Ubuntu中文文件夹转英文
想到以前使用Ubuntu的痛苦经历，小编决定直接安装英文版。
安装好之后，在Language设置中选择中文，重启。启动后，系统会询问是否把Home文件夹中的文件夹转换为中文，选择否。如此，便可以保持中文界面，同时在使用命令的时候，不用中英文输入法来回切换。

### Win8比Ubuntu慢8小时
设置好Ubuntu，重启切换到Win8，发现Win8时间要比Ubuntu的时间慢8个小时！

原因：
Windows把系统硬件时间当作本地时间（local time），即操作系统中显示的时间跟BIOS中显示的时间是一样的。
Linux/Unix/Mac把硬件时间当作UTC，操作系统中显示的时间是硬件时间经过换算得来的，比如说北京时间是GMT+8，则系统中显示时间是硬件时间+8。

解决办法：
修改ubuntu时间如下：
`sudo gedit /etc/default/rcS`
找到这一行： **UTC=yes**
把 **yes** 改为 **no** 。

### 设置Win8为默认启动项
1、在Ubuntu下，进入/etc/grub.d
2、`sudo mv 30_os-prober 06_os_prober`
3、`sudo update-grub`


### 参考文档
win8和ubuntu13.10 双系统时间错误
http://blog.csdn.net/flywindmouse/article/details/13025017

------

将Ubuntu主文件夹里的中文文件夹名称改成英文
http://blog.csdn.net/l0605020112/article/details/20285239

------
Windows与Ubuntu双系统启动菜单管理和配置（补充win8） 
http://blog.chinaunix.net/uid-23586172-id-3075207.html