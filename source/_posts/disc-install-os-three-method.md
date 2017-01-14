title: 安装系统之三种方式
date: 2015-01-30 12:16:49
tags: [系统,操作系统]
categories: 点滴发现
---
### 前言

安装系统的教程有很多，最常用三种方式：硬盘安装，U盘安装，光盘安装。

### 硬盘安装
从硬盘安装系统，是最简单的一种方式。这种方式，前提是原系统可以正常开机。

#### 步骤一：下载镜像
我们以安装win8为例。
1、打开浏览器，然后在地址栏输入一个搜索引擎网址，比如百度。
2、在搜索框输入“win8镜像”，单击第一个搜索结果。
![](http://voidking.qiniudn.com/@/imgs/os/01.jpg)
3、选择一个镜像，进入下载页
![](http://voidking.qiniudn.com/@/imgs/os/02.jpg)
4、找到下载链接，下载镜像，大小一般在2g到4g，文件名称为*.iso。
![](http://voidking.qiniudn.com/@/imgs/os/03.jpg)
5、解压下载好的镜像。
![](http://voidking.qiniudn.com/@/imgs/os/04.jpg)
<!--more-->
#### 步骤二：安装系统
1、双击autorun.exe，选择打开ghost安装器。
![](http://voidking.qiniudn.com/@/imgs/os/05.jpg)
2、在ghost安装器界面，选中需要安装系统的盘符，以及刚才解压出来的win8.gho映像。单击“执行”，安装系统就开始了。
![](http://voidking.qiniudn.com/@/imgs/os/07.jpg)
3、系统重启几次后，系统安装完成。这个过程，10到30分钟。
![](http://voidking.qiniudn.com/@/imgs/os/08.jpg)
![](http://voidking.qiniudn.com/@/imgs/os/09.jpg)

### U盘安装
从U盘安装系统，是最常用的一种方式。如果你的系统彻底崩溃了，或者你想分区后再重装系统，这种方式是很好的选择。

#### 步骤一：下载镜像
这个步骤的操作方法和硬盘安装步骤一相同。

#### 步骤二：制作启动盘
1、将U盘插入USB接口。
2、下载启动盘制作工具，我们以U大师为例。进入[U大师官网](http://www.udashi.com/)，然后下载U大师。
![](http://voidking.qiniudn.com/@/imgs/os/10.jpg)

3、安装U大师，并启动。
![](http://voidking.qiniudn.com/@/imgs/os/11.png)
4、制作启动盘。
![](http://voidking.qiniudn.com/@/imgs/os/12.jpg)
![](http://voidking.qiniudn.com/@/imgs/os/13.png)
![](http://voidking.qiniudn.com/@/imgs/os/14.png)

#### 步骤三：拷贝映像
步骤一中，我们解压出的文件中有一个win8.gho，复制win8.gho映像到U盘。

#### 步骤四：从U盘启动
1、将做好的启动盘插入USB接口。
2、点完开机键，狂按F12（一般的品牌机，选择启动项的键都是F12）。出现启动项选择界面，一般可供选择的有光驱、硬盘、网络、可移动磁盘（U盘）。这里我们选择从U盘启动。
![](http://voidking.qiniudn.com/@/imgs/os/15.jpg)
![](http://voidking.qiniudn.com/@/imgs/os/16.jpg)
3、如果按F12没有出现启动项选择界面，请参考下列启动按键。
![](http://voidking.qiniudn.com/@/imgs/os/17.png)
4、如果还是没有出现启动项选择界面，或者出现了启动项选择界面，但是没有从U盘启动这个选项。这时，就需要设置BIOS。不同机器的BIOS差别较大，不展开讲了，给出两个参考，分别来自[百度经验](http://jingyan.baidu.com/article/a65957f4a89d8e24e67f9b90.html)和[U大师](http://www.udashi.com/jc/2.html)。

#### 步骤五：进入winpe
1、假定步骤四我们已经成功完成，这时候，我们会看到下面的界面。
![](http://voidking.qiniudn.com/@/imgs/os/18.jpg)

2、有时间的话，这上面的选项你可以挨个试试。这里我们选择【01】进入。
![](http://voidking.qiniudn.com/@/imgs/os/19.jpg)


#### 步骤六：安装系统
1、winpe系统会自动弹出一个软件——U大师智能快速装机。安装源我们选择U盘里的win8.gho。
![](http://voidking.qiniudn.com/@/imgs/os/20.jpg)

2、单击上图中的开始，安装系统开始。
![](http://voidking.qiniudn.com/@/imgs/os/21.jpg)

3、系统重启几次后，系统安装完成。这个过程，10到30分钟。


### 光盘安装
从光盘安装系统，是最传统的一种方式。这种方式的前提是，你的电脑要有光驱。

如果，你有系统盘（无论是自己做的，还是从商店买的），那么直接插入光驱，然后从步骤三开始往下看。我们这里假设你没有系统盘，有一张4g的空白光盘。

#### 步骤一：下载镜像
这个步骤的操作方法和硬盘安装步骤一相同，需要注意的是，这里不需要解压。

#### 步骤二：制作系统盘
1、将空白盘插入光驱。
2、下载安装光盘刻录软件，这里小编使用的是UltraISO。
3、打开UltraISO，将下载的镜像文件*.iso刻录到空白光盘。
![](http://voidking.qiniudn.com/@/imgs/os/22.jpg)
4、刻录完成后，系统盘也就制作完成了。

#### 步骤三：从光盘启动
1、将系统盘插入光驱。
2、点完开机键，狂按F12。出现启动项选择界面，我们选择从CD-ROM启动。

#### 步骤四：安装系统
选择“安装win8系统到C盘”，开始安装系统。系统自动重启几次，安装完成。在这个界面，我们也可以进入winpe，然后分区或者安装系统。
![](http://voidking.qiniudn.com/@/imgs/os/23.jpg)


### 小结
所谓的不同方式，说白了，不过是安装源的存储介质的区别。

还有其他的安装方式，下面再给出两个：
1、移动硬盘
这种安装方式和从U盘安装类似，甚至，你可以把系统安装在移动硬盘，随身携带。

2、网络
在安装linux的时候，可以通过网络获取最新版。


