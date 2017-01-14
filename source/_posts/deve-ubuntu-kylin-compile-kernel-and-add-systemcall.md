title: "Ubuntu Kylin编译内核和添加系统调用"
toc: true
date: 2015-06-29 11:01:47
tags:
- Ubuntu
- Linux
categories:
---

# 下载内核
## 查看当前内核版本
`uname -a`
![](http://7oxjrx.com1.z0.glb.clouddn.com/imgs/linux/compile/kernalversion.jpg)
可以看到，当前内核版本为3.16.0。
<!--more-->

## 获取当前版本内核源码
`sudo apt-get install linux-source`
内核源码默认下载到/usr/src文件夹下，这里，我们就使用这套源码。

## 官方下载地址
https://www.kernel.org/
这个网站，你可以找到最新的内核版本，如果要想要给内核升级，可以来这里下载。

# 解压内核
`cd /usr/src/`
`tar -jvf linux-source-3.16.0.tar.bz2`
解压失败：
```
tar: You must specify one of the '-Acdtrux', '--delete' or '--test-label' options
Try 'tar --help' or 'tar --usage' for more information.
```

添加一个参数x：
`tar -xjvf linux-source-3.16.0.tar.bz2`
再次失败，各种Permission denied。

`su`
`tar -xjvf linux-source-3.16.0.tar.bz2`
虽然速度很慢（3分钟左右），但是好歹解压成功了。

# 添加系统调用
## 修改sys.c
`cd linux-source-3.16.0/kernel/`
`gedit sys.c`
添加如下代码：
```
/*
 *new systemcall added by voidking
 */
asmlinkage int sys_callvoidking(void)
{
	printk("You are handsome,VoidKing!");
	return 0;
}
```
![](http://7oxjrx.com1.z0.glb.clouddn.com/imgs/linux/compile/sys.jpg)

## 修改syscall_32.tbl
`cd /usr/src/linux-source-3.16.0/arch/x86/syscalls`
`gedit syscall_32.tbl`
在最后一行插入
```
355	i386	callvoidking		sys_callvoidking
```
![](http://7oxjrx.com1.z0.glb.clouddn.com/imgs/linux/compile/syscall_32.jpg)

## 修改syscalls.h
`cd /usr/src/linux-source-3.16.0/include/linux/`
`gedit syscalls.h`
在`#endif`前添加：
```
asmlinkage int sys_callvoidking(void);
```
![](http://7oxjrx.com1.z0.glb.clouddn.com/imgs/linux/compile/syscalls.jpg)

# 编译准备
## libncurses5-dev
安装nucurses库，为了make menuconfig调用。
`apt-get install libncurses5-dev`

## 配置文件
把原先系统的配置文件拷贝到当前源码目录。
`cp /boot/config-\`uname -r\` .config`

## 加载配置文件
`make menuconfig`，打开编译配置界面，load，ok，save，exit。
![](http://7oxjrx.com1.z0.glb.clouddn.com/imgs/linux/compile/makemenuconfig.jpg)
![](http://7oxjrx.com1.z0.glb.clouddn.com/imgs/linux/compile/load.jpg)
![](http://7oxjrx.com1.z0.glb.clouddn.com/imgs/linux/compile/save.jpg)

# 编译命令
## 编译内核
`make -j8`或者`make bzImage`，前者更快一点。
![](http://7oxjrx.com1.z0.glb.clouddn.com/imgs/linux/compile/kernelcompile.jpg)

有些同学要两个小时才能编译完成，小编使用VirtualBox，只用了20分钟，电脑配置之牛逼可见一斑！哇哈哈哈！

## 编译模块
`make modules`，编译模块时，小编自己添加的系统调用出现了警告，不过不影响接下来的编译。
一个小时之后。。。噗！要吐血了，之前分给虚拟机8G存储空间，没想到编译过程中，空间不足了，没法继续编译。。。

## 扩展Ubuntu空间
1、首先关闭虚拟机，在设置中给Ubuntu再添加一块虚拟硬盘。
2、打开虚拟机，`cd /dev`
3、`fdisk sdb`
4、命令p：查看当前新盘状态。
5、命令n：创建一个新的分区。
6、两个选项e（扩展分区）和p（主分区），选择p。
7、连续两次回车，使用默认的起始和结束sector。
8、创建了一个sdb1，大小为整块虚拟硬盘。
9、命令w：保存退出。
10、`mkfs -t ext4 sdb1`，格式化。
11、`cd /mnt`，`mkdir sdb1`，`mount /dev/sdb1 sdb1`
12、`cd sdb1`，`cp -rv /usr/src/linux-source-3.16.0 .`
13、`umount sdb1`，`mount /dev/sdb1 /usr/src`

## 重新编译模块
`cd /usr/src/linux-source-3.16.0`，`make modules`
等。。。再等。。。继续等。。。四个小时之后，提示空间不足。。。我靠，你丫到底需要多大空间？8G的空间不够你编译程序用的！！！哭了。。。

## 加速编译
寻找加速编译内核的办法，发现，似乎`make -j8`等于`make bzImage`加`make modules`。
去掉编译模块这个步骤试试：
`make mrproper`，`make clean`，`make -j8`，然后`make modules_install`。报错：
```
  INSTALL arch/x86/crypto/aes-x86_64.ko
cp: cannot stat ‘arch/x86/crypto/aes-x86_64.ko’: No such file or directory
Can't read private key
scripts/Makefile.modinst:30: recipe for target 'arch/x86/crypto/aes-x86_64.ko' failed
make[1]: *** [arch/x86/crypto/aes-x86_64.ko] Error 2
Makefile:1089: recipe for target '_modinst_' failed
make: *** [_modinst_] Error 2
```
![](http://7oxjrx.com1.z0.glb.clouddn.com/imgs/linux/compile/modulesinstallerror.jpg)
猜测是配置文件.config文件有问题，换成/usr/src/linux-headers-3.16.0-23-generic目录下的.config文件。重新编译，安装，还是报同样错误。那就再加上编译模块这个步骤吧！

## 第三次编译模块
`make -j8 modules`，大概四个小时之后，终于编译完成。
![](http://7oxjrx.com1.z0.glb.clouddn.com/imgs/linux/compile/modulesbuildsuccess.jpg)
安装模块报错，和`make -j8`编译后直接安装模块报错相同。给跪了，能不能愉快地玩耍了！

## 找到错误
后来，仔细检查了前面的步骤。发现修改syscalls.h的时候，声明类型写错了，本该是int，却写成了long类型。修改成int，一路顺利。实践证明，`make -j8`等于`make bzImage`加`make modules`。

## 安装模块
`make modules_install`
![](http://7oxjrx.com1.z0.glb.clouddn.com/imgs/linux/compile/modulesinstallsuccess.jpg)

## 建立映像文件
建立要载入ramdisk的映像文件
`mkinitramfs -o /boot/initrd-mylinux.img`
`cd /boot`，`ll`
![](http://7oxjrx.com1.z0.glb.clouddn.com/imgs/linux/compile/initrd-mylinux.img.jpg)

## 安装内核
`make install`
![](http://7oxjrx.com1.z0.glb.clouddn.com/imgs/linux/compile/kernelinstallsuccess.jpg)

# 更新grub引导
`update-grub`
![](http://7oxjrx.com1.z0.glb.clouddn.com/imgs/linux/compile/update-grub.jpg)

# 测试
## 选择新内核
开机时，选择新内核引导系统。

## voidking.c
新建文件voidking.c，内容如下：
```
#include <unistd.h>
#include <stdio.h>

#define callvoidking 355

int main()
{
	syscall(callvoidking);
	return 0;
}
```
## 编译执行
`gcc -o voidking voidking.c`
`./voidking`
最终什么都没有显示出来，不知道错在哪里。。。也许是sys.c中的函数没有写好，不重做了，整个流程耗时太久，领会精神就够了。

# 小结
为Linux添加新的系统调用，主要包括有4个步骤：编写系统调用服务例程；添加系统调用号；修改系统调用表；重新编译内核并测试新添加的系统调用。

虽然这次实验最终失败了，但是，我掌握了添加系统调用和编译内核的流程，培养了耐心以及分析解决问题的能力。也算是收获颇丰，吼吼！


# 参考文档
Ubuntu 14.04 TLS 内核升级和添加系统调用
http://yunpan.cn/cQtsTfkbeTPvG  访问密码 adce

------
如何实现一个新的系统调用
http://book.51cto.com/art/201007/213651.htm

------
ubuntu下重新编译内核
http://m.blog.csdn.net/blog/chen_jianjian/42168449

------
Ubuntu Linux内核编译步骤
http://www.linuxidc.com/Linux/2012-03/57303.htm

------
Add a system call to the linux kernel in Ubuntu
http://www.franksthinktank.com/howto/addsyscall/

------
Ubuntu添加系统调用  
http://gnodsy.blog.163.com/blog/static/17754713320112135352447/

------
Ubuntu内核的重新编译安装
http://www.linuxidc.com/Linux/2009-12/23721.htm