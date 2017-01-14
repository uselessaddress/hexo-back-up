title: Linux复习整理
toc: true
date: 2015-07-03 10:51:42
tags:
- linux
categories: 设计开发
---
# 题型

1、填空（10\*1')
2、选择（10\*1'）
3、简单操作题（写命令）(5\*2')
4、简答题（6\*5'）
5、综合题（2\*20'）（程序或者流程图）

# 考纲

## 命令题
### 定时任务如何设计
小明是个网管，老板要求他每天早晨3点起来看看磁盘空间满不满，请问你如何用学过的知识为小明解决这一痛苦的问题。
<!--more-->

#### 编写脚本文件
假设在/root目录下：
`vim diskfree.sh`，内容如下：

```
#!/bin/bash
#取得每个分区的使用百分比（不要百分号）
percent=`df -h | grep -v Filesystem| awk '{print int($5)}'`

#循环判断分区使用率是否超过90%
for each_one in $percent
do
		#判断使用率是否超过90%
        if [ $each_one -ge 90 ];then
				#如果超过90 则把使用情况发给mail_address
                df | mail -s "Disk Critical" mail_address
        fi
done

```
稍微解释一下代码：
df，检查文件系统的磁盘空间占用情况；-h，以方便阅读方式展示，这个参数可不加。
|，管道命令，左侧命令的处理结果传递给右侧。
grep -v，忽略含有Filesystem的这一行。
awk '{print int($5)}'，逐行读入文件流，以空格或TAB为默认分隔符将每行切片，切开的部分再进行各种分析处理。这里实现的效果是，把第5个域（Use%）的数据转换为int类型。
-ge，大于或等于，可以换成-gt，表示大于。
mail -s，之后跟的是标题和收件人邮箱，管道之前的是内容。


#### 添加执行权限
`chmod +x diskfree.sh`

#### 添加自动执行
`vim /etc/crontab`，追加如下一句：
```
0 3 * * * root /root/diskfree.sh > /dev/null 2>&1
```
上面的代码依次对应：

m（分）：1～59 每分钟用\*或者 \*/1表示
h（时）：1～23（0表示0点）
dom（日）：1～31
mon（月）：1～12 
dow（周）：0～6（0表示星期天） 
user（用户）：用户
command（命令）：命令


下面是一些时间例子：
1、每天早上6点10分 
2、每两个小时 
3、晚上11点到早上8点之间每两个小时，早上8点 
4、每个月的4号和每个礼拜的礼拜一到礼拜三的早上11点 
5、1月份日早上4点 
```
10 6 * * * 
0 */2 * * * 
0 23-7/2，8 * * * 
0 11 4 * mon-wed 
0 4 1 jan * 
```

> \> /dev/null 2>&1，这一句可以省略。如果不加这一句，当程序在你所指定的时间执行后，系统会发一份邮件到你的mail里面(/usr/spool/mail/用户名)，显示该程序执行的内容。


### 命令使用

#### mv
某领导要求小刚把bin目录下后缀名为“.tx.htm”的文件重命名为“.html”
假设当前目录为bin，则：
1、单个文件重命名
`mv test.tx.htm test.html`
2、批量重命名
`rename 's/\.tx\.htm$/\.html/' *.tx.htm`
PS：移动文件
`mv test.tx.htm ..`，把test.tx.htm移动到上一层目录。

#### mount
把/dev/sdb1挂载到/mnt/sdb1。
`mkdir /mnt/sdb1`，新建文件夹sdb1。
`mount /dev/sdb1 /mnt/sdb1`，挂载。
`umount /mnt/sdb1`，卸载。

#### cp 
登录到电信机房，发现木有显示屏，请问如何把can目录底下所有后缀名为“txt”的文件拷贝到U盘中，假设U盘的目录是/udsk，当前所在目录为can。
`cp *.txt /udsk`

#### passwd
给新员工小明分配一个账户并设置默认密码“12345678”。
`useradd xiaoming`
`passwd xiaoming`，之后两次输入密码12345678

#### ifconfig
小明发现网络不通，他想看看第二块网卡是否分正常分配了IP地址，他如何为第二块网卡分配192.168.1.1。
`ifconfig eth1`
`ifconfig eth1 192.168.1.1 netmask 255.255.255.0`

#### netstat
某web服务器部署在linux上，当远程登录时发现该应用不通。请问如何检查该应用是否起动，假设该应用的服务端口号是“8181”。
`netstat -anp | grep 8181`

- a ：all，表示列出所有的连接，服务监听，Socket资料
- t ：tcp，列出tcp协议的服务
- u ：udp，列出udp协议的服务
- n ：port number， 用端口号来显示
- l ：listening，列出当前监听服务
- p ：program，列出服务程序的PID

#### pwd 
如何知道工作目录？
`pwd`

-P：如果当前的工作路径是链接的话，显示链接的原始路径，也就是实际路径。

#### who
当前终端登录的用户是谁？
`who am i`

#### ls
查看当前文件夹下所有文件，包含隐藏文件和属性
`ls -la`

## 简答题
### Linux有哪些运行级别？
Linux系统有7个运行级别(runlevel)。
运行级别0：系统停机状态，系统默认运行级别不能设为0，否则不能正常启动
运行级别1：单用户工作状态，root权限，用于系统维护，禁止远程登陆
运行级别2：多用户状态(没有NFS)
运行级别3：完全的多用户状态(有NFS)，登陆后进入控制台命令行模式
运行级别4：系统未使用，保留
运行级别5：X11控制台，登陆后进入图形GUI模式
运行级别6：系统正常关闭并重启，默认运行级别不能设为6，否则不能正常启动

PS：
1、查看当前运行级别
`runlevel`
2、切换运行级别
`init N`，N的值为0到6，其中，0为关机，6为重启。



### 如何创建一个用户？
1、`su`，切换到root用户。
2、`useradd testuser`，创建一个名为testuser的用户。
- c： comment，指定一段注释性描述。
- d：目录，指定用户主目录，如果此目录不存在，则同时使用-m选项，可以创建主目录。
- g：用户组，指定用户所属的用户组。

3、`passwd testuser`，之后回车，为testuser设置密码。



### 文件系统
linux有哪些常见的文件系统？linux管理Windows下的文件系统？

linux常见的文件系统有ext、ext2、ext3、ext4、JFS、XFS、ReiserFS等。
假设linux下有一个盘hda1的文件系统为fat32，那么：
`mkdir /mnt/win1`
`mount -t vfat /dev/hda1 /mnt/win1`
假设linux下有一个盘hda2的文件系统为ntfs，如果内核支持ntfs，那么：
`mkdir /mnt/win2`
`mount -t ntfs /dev/hda2 /mnt/win2`
假设linux下有一个盘hda2的文件系统为ntfs，如果内核不支持ntfs，那么：
`apt-get intall ntfs-3g`，此命令适合Ubuntu系统
`mkdir /mnt/win2`
`ntfs-3g /dev/hda2 /mnt/win2`


### gcc/g++
gcc编译的步骤有哪些？会生成哪些文件？

gcc/g++在执行编译工作的时候，总共需要4步。
1、预处理，生成.i的文件。（预处理器cpp）
2、将预处理后的文件转换成汇编代码，生成文件.s。（编译器egcs） 
3、将汇编代码变为目标代码(机器代码)生成.o的文件。（汇编器as）
4、连接目标代码，生成可执行程序。（链接器ld）

假设hello.c为最初的源代码：
`gcc -E hello.c -o hello.i`，生成经过预处理的代码；
`gcc -S hello.i -o hello.s`，生成汇编处理后的汇编代码；
`gcc -c hello.s -o hello.o`，生成编译后的目标文件，含有最终编译出的机器码，但它里面所引用的其他文件中函数的内存位置尚未定义；
`gcc hello.o -o hello`，可执行程序。
上面的四个命令，可以合成一个`gcc hello.c -o hello`。

### vi的使用
vi有哪些使用方法有哪些模式？模式下有怎样的编辑？如何为vi当中增加行号？

三种模式：

- 命令行模式（command mode）：
控制屏幕光标的移动，字符、字或行的删除，移动复制某区段等。
按下“i”，进入插入模式；按下“:”，进入底行模式。

- 插入模式（Insert mode）：
只有在Insert mode下，才可以做文字输入，按「ESC」键可回到命令行模式。

- 底行模式（last line mode）：
将文件保存或退出vi，也可以设置编辑环境，如寻找字符串、列出行号……等。

增加行号：
在底行模式中，输入`set number`

基本操作：
1、进入vi
`vi filename`
进入vi之后，是处于命令行模式。输入“i”，切换到插入模式。 

2、在插入模式编辑文件  
编辑文件，编辑完成点击「ESC」，回到命令行模式。
 
3、退出vi及保存文件 
在命令行模式下，按一下“:”进入底行模式： 
- wq (存盘并退出vi) 
- q! (不存盘强制退出vi) 

## 综合题
### shell编程
接收用户从键盘输入的十个整数，然后求出其总和、最大值、最小值，从小到大排序。
```
#!/bin/sh
for i in `seq 10`
do
  read  var
  echo $var >> tempfile.tmp
done 
echo "min number is :"`sort -n  tempfile.tmp |head -n1 `
echo "max number is :"`sort -rn  tempfile.tmp |head -n1 `
echo "sum of all number:"`awk '{ a+=$0}END{ print a}' tempfile.tmp `
echo "the order is:"`sort -n tempfile.tmp`
rm tempfile.tmp
```

### 操作系统最后五个实验
Liunx当中的ls，mao，pwd，硬盘引导，内核编译。重点关注内核引导，内核编译，pwd。


# 考纲升级版

## 基本概念
### 登陆提示符
超级用户和普通用户的登录提示符是什么？
超级用户root的提示符是#，普通用户的提示符是$。

### 桌面环境
Linux系统下经常使用的两种桌面环境是什么？
（1）GNOME，最常见
（2）KDE

### Linux系统如何标识硬盘
Linux下硬盘分区的标识在Linux下用hda、hdb等来标识不同的硬盘；用hda1、hda2、hda5、hda6 来标识不同的分区。

前两个字母：hdx（x为a-d）代表IDE硬盘，sdx（x为a-z）代表SCSI、SATA、USB硬盘。
第三个字母：a\b\c……代表的是1、2、3……的意思。
hda代表第一块IDE硬盘，sdb代表第二块SCSI硬盘。

第四位数字：可以理解为Windows盘下的C\D\E盘符。
hda1代表第一个IDE硬盘的第一个分区，sdb2代表第二个SCSI硬盘的第二个分区。


### Linux在I386体系结构中的分页支持几级？
i386采用二级分页，其线性地址的结构如下：
Dir有10位，表示页表目录项的下标，指向一个页表；Page有10位，表示一个具体页表中的目录项的下标，指向一个物理页面；Offset有12位，表示在物理页面中的偏移量（单位为字节）。



### 什么时候需要编译内核？
1、尝鲜，linux内核发行了新版本，想在第一时间使用新功能。

2、使用一些工具，需要使用包含debuginfo的内核，而一般的发行版本不包含debuginfo。

3、修改了内核代码，比如添加了系统调用。

4、做arm的嵌入式开发，将其烧制到主板上。

## 命令操作
### 执行定时任务
1、执行一次
语法：
```
at [参数] [时间]
at> 执行的指令
```
退出at命令 ctrl+d。
`atq`，查询当前的等待任务，被执行之后就不会显示。
`atrm 任务的工作号`，删除系统中由at建立的正在等待被执行的任务。

at命令的参数：
-m ：当指定的任务被完成之后，将给用户发送邮件，即使没有标准输出
-I ：atq的别名
-d ：atrm的别名
-v ：显示任务将被执行的时间
-c ：打印任务的内容到标准输出
-V ：显示版本信息
-q ：后面加<列队> 使用指定的列队
-f ：后面加<文件> 从指定文件读入任务而不是从标准输入读入
-t ：后面<时间参数> 以时间参数的形式提交要运行的任务 

例子：
明天17:20，输出时间到指定文件内
```
at 17:20 tomorrow
at> date > /home/voidking/date.txt
at> <EOT>
```


2、定期执行
详情请见：小明是个网管，老板要求他每天早晨3点起来看看磁盘空间满不满。。。

### 重命名文件
1、单个文件重命名
`mv test.tx.htm test.html`
2、批量重命名
`rename 's/\.tx\.htm$/\.html/' *.tx.htm`

### 加载光驱
`ls -l /dev | grep cdrom`，假设看到光驱全名为cdrom1。
`mount /dev/cdrom1 /mnt/`，把光盘挂载到/mnt目录下。

### 如何修改密码
1、修改自己的密码
`passwd`，输入当前密码，输入新密码。
2、root用户修改其他用户的密码
`passwd username`，输入新密码。

### 如何看本机网址
1、查看本机IP地址
`ifconfig`
2、设置IP地址
`ifconfig eth1 192.168.1.1 netmask 255.255.255.0`

PS：查看主机名
`hostname`

### 如何合并文件
`cat file1.txt file2.txt > file.txt`，file1.txt和file2.txt合成file.txt。
`cat file1.txt >> file.txt`，file1.txt中的内容添加到file.txt的最后。

### 如何创建用户
1、`su`，切换到root用户。
2、`useradd testuser`，创建一个名为testuser的用户。
3、`passwd testuser`，之后回车，为testuser设置密码。

### 如何删除文件
`rm filename`
-f, --force    忽略不存在的文件，从不给出提示。
-i, --interactive 进行交互式删除
-r, -R, --recursive   指示rm将参数中列出的全部目录和子目录均递归地删除。
-v, --verbose    详细显示进行的步骤

### 如何查看当前路径
`pwd`
-P：如果当前的工作路径是链接的话，显示链接的原始路径，也就是实际路径。

## 综合
### linux的运行级别
Linux系统有7个运行级别(runlevel)。
运行级别0：系统停机状态，系统默认运行级别不能设为0，否则不能正常启动
运行级别1：单用户工作状态，root权限，用于系统维护，禁止远程登陆
运行级别2：多用户状态(没有NFS)
运行级别3：完全的多用户状态(有NFS)，登陆后进入控制台命令行模式
运行级别4：系统未使用，保留
运行级别5：X11控制台，登陆后进入图形GUI模式
运行级别6：系统正常关闭并重启，默认运行级别不能设为6，否则不能正常启动

PS：
1、查看当前运行级别
`runlevel`
2、切换运行级别
`init N`，N的值为0到6，其中，0为关机，6为重启。


### linux的文件权限
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/linux/mode.jpg)
如图，`-rwxrwxr-x`，这10位表示文件的权限，第2～10个字符中每3个为一组。

左边三个字符（rwx）表示所有者权限，中间3个字符（rwx）表示与所有者同一组的用户的权限，右边3个字符（r-x）是其他用户的权限。

r(Read，读取)：对文件而言，具有读取文件内容的权限；对目录来说，具有浏览目录的权
w(Write,写入)：对文件而言，具有新增、修改文件内容的权限；对目录来说，具有删除、移动目录内文件的权限。
x(eXecute，执行)：对文件而言，具有执行文件的权限；对目录了来说该用户具有进入目录的权限。

改变文件权限：
`chmod [options] [who][opcode]mode files`

options：

- -R，--recursive
可递归遍历子目录，把修改应到目录下所有文件和子目录

who：

- a，默认值，所有用户
- u，拥有者
- g，同组用户
- o，其他用户。

opcode:

-  +，增加权限
-  -，删除权限
- =，重新分配权限

mode：

- r=4，读
- w=2，写
- x=1，执行

还可设置第四位，它位于三位权限序列的前面：

- 4，执行时设置用户ID，用于授权给基于文件属主的进程，而不是给创建此进程的用户。
- 2，执行时设置用户组ID，用于授权给基于文件所在组的进程，而不是基于创建此进程的用户。
- 1，设置粘贴位。


实例：
```
chmod u+x file	给属主增加执行权限
chmod 751 file	给属主所有权限，给组分配读和执行权限，给其他用户执行权限
chmod u=rwx,g=rx,o=x file	上例的另一种形式
chmod =r file	为所有用户分配读权限
chmod 444 file	同上例
chmod a-wx,a+r file	同上例
chmod -R u+r directory	directory目录下所有文件和子目录分配读的权限
chmod 4755	设置用ID，给属主分配读、写和执行权限，给组和其他用户分配读、执行的权限。
```

### 如何创建文件系统
假设新添加了一块硬盘sdb，想要把这块硬盘的所有空间分给第一个分区，而且该分区文件系统为ext4。
1、`cd /dev`
2、`fdisk sdb`
3、命令p：查看当前新盘状态。
4、命令n：创建一个新的分区。
5、两个选项e（扩展分区）和p（主分区），选择p。
6、连续两次回车，使用默认的起始和结束sector。
7、创建了一个sdb1，大小为整块虚拟硬盘。
8、命令w：保存退出。
9、`mkfs -t ext4 sdb1`，格式化为ext4文件系统。


### 如何用GCC编译文件？
gcc编译的步骤有哪些？会生成哪些文件？

gcc/g++在执行编译工作的时候，总共需要4步。
1、预处理，生成.i的文件。（预处理器cpp）
2、将预处理后的文件转换成汇编代码，生成文件.s。（编译器egcs） 
3、将汇编代码变为目标代码(机器代码)生成.o的文件。（汇编器as）
4、连接目标代码，生成可执行程序。（链接器ld）

假设hello.c为最初的源代码：
`gcc -E hello.c -o hello.i`，生成经过预处理的代码；
`gcc -S hello.i -o hello.s`，生成汇编处理后的汇编代码；
`gcc -c hello.s -o hello.o`，生成编译后的目标文件，含有最终编译出的机器码，但它里面所引用的其他文件中函数的内存位置尚未定义；
`gcc hello.o -o hello`，可执行程序。
上面的四个命令，可以合成一个`gcc hello.c -o hello`。


### vi的3种模式
三种模式：

- 命令行模式（command mode）：
控制屏幕光标的移动，字符、字或行的删除，移动复制某区段等。
按下“i”，进入插入模式；按下“:”，进入底行模式。

- 插入模式（Insert mode）：
只有在Insert mode下，才可以做文字输入，按「ESC」键可回到命令行模式。

- 底行模式（last line mode）：
将文件保存或退出vi，也可以设置编辑环境，如寻找字符串、列出行号……等。

增加行号：
在底行模式中，输入`set number`

基本操作：
1、进入vi
`vi filename`
进入vi之后，是处于命令行模式。输入“i”，切换到插入模式。 

2、在插入模式编辑文件  
编辑文件，编辑完成点击「ESC」，回到命令行模式。
 
3、退出vi及保存文件 
在命令行模式下，按一下“:”进入底行模式： 
- wq (存盘并退出vi) 
- q! (不存盘强制退出vi) 

### 使用shell完成高斯求和
所谓高斯求和，就是等差数列求和，这里分别输入开始值、结束值、等差：
```
#!/bin/sh

read -p "Input value of start: " start
read -p "Input value of end: " end
read -p "Input value of diff: " diff
count=$(($(($(($end-$start))/$diff))+1))
sum=$(($(($end+$start))*$count/2))
echo "SUM is $sum"
```


### 用汇编语言实现操作系统引导
#### 第一种思路
```
CODES	SEGMENT
		ORG 07C00H		; 将此段加载到内存0x0000:7C00处
start:
		MOV AX, CODES
		MOV ES, AX
		MOV AX, OFFSET msg		; 将字符串拷贝到ax
		MOV BP, AX		; es:bp = 串地址

		MOV CX, OFFSET strend              
		MOV DX, OFFSET msg
		SUB CX, DX		; cl= 串长度 
		MOV len, CX

next:	INC color
		AND color, 0FH
		MOV AX, 1301H		; ah = 13 TELTYPE 显示字符串, al = 00h 
		MOV BH, 00H		; 页号为0（bh = 0） 
		MOV BL, color		;黑底红字（bl = 0ch，高亮）
		MOV CX, len
		MOV DX, 0815H	;第0h行15h列（dh = 0 dl = 15h）
		INT 10H		; 10h号中断
		JMP next
over:	RET		

		color	DB 00H
		len		DW 0000H 
		msg		DB "Hello World!"
		strend	DB '$'
		ORG 07C00H+200H-2H	;把结尾标志加载到(07c00h+200h-2h)处     
		DW 0AA55H (07c00h+512d-2d)
CODES	ENDS    
		END	start

```
参考文档：
简单OS开发前奏(三)
http://blog.csdn.net/otishiono/article/details/5906119
用汇编语言编写一个Boot Sector
http://blog.csdn.net/misskissc/article/details/8702337

感觉有点难，考试时还是画流程图吧！╮(╯▽╰)╭

#### 第二种思路
第一种思路有问题，虽然实现了题目要求，但是和Linux这门课没啥关系，提供第二种思路如下。

首先说一下操作系统启动过程：
POST->BIOS->Bootloader->Linux kernel->init->system ready。

我们要完成的，就是Bootloader中的启动代码。Bootloader的工作有：
1) 初始化RAM 
2) 初始化串口  
3) 检测处理器类型 
4) 设置Linux启动参数 
5) 调用Linux内核映像

没有找到合适的参考代码，小伙伴们找到了记得在群里分享一下。

参考文档：
linux bootloader 
http://blog.chinaunix.net/uid-28440799-id-3484616.html
Bootloader分析
http://blog.csdn.net/xiaomt_rush/article/details/6582337
Boot Linux
http://www.almesberger.net/cv/papers/ols2k-9.pdf
Linux 引导过程内幕
http://www.ibm.com/developerworks/cn/linux/l-linuxboot/
引导加载程序之争：了解 LILO 和 GRUB
http://www.ibm.com/developerworks/cn/linux/l-bootload.html
Linux系统引导过程（BIOS和Bootloader部分）
http://blog.csdn.net/keminlau/article/details/4523973

# 老师赠言
大题目都给大家了，小题目看上课的听讲情况了，尽量不要空着试卷，考试不会难为大家。 

# 后记
不保证正确性，仅供参考。导出的pdf文档格式不友好，给出本文链接：
http://www.voidking.com/2015/07/03/deve-linux-review/
