title: 红旗Linux安装Grub2
date: 2013-11-01 09:23:46
tags: 
- Linux
categories: 精华转载
---

我使用红旗7-0630系统，默认KDE桌面，内核版本更新到2.6.31，分区为：swap=/dev/sda8，/=/dev/sda9。

（一）安装grub2
1、下载：ftp://alpha.gnu.org/gnu/grub/（我下载其中的grub-1.96.tar.gz)
2、备份/boot（备份是个好习惯，就算安装错误，也可以恢复）
3、cd /下载目录（我的下载目录是/tmp，因此命令为：cd /tmp）
4、tar xvf grub-1.96.tar.gz
5、cd /tmp/grub-1.96
6、./configure
7、make && make install
8、grub-install --root-directory=/ /dev/sda   ＃引导记录写入MBR
9、update-grub              ＃它也是一个脚本，将根据 /usr/local/etc/grub.d/ 里的文件自动创建 /boot/grub/grub.cfg
10、提示：<!--more-->
Updating /boot/grub/grub.cfg ...
Found linux image: /boot/vmlinuz-2.6.31
Found initrd image: /boot/initrd-2.6.31.img
Found linux image: /boot/vmlinuz-2.6.29.4-167.dt7.i586
Found initrd image: /boot/initrd-2.6.29.4-167.dt7.i586.img
done
11、reboot       ＃重启，OK终于成功安装Grub2。

（二）grub2配置
配置文件为：/boot/grub/grub.cfg和上个版本的配置文件不同/boot/grub/grub.conf
我贴出自己当前的配置文件，如下：
```
#
# DO NOT EDIT THIS FILE
#
# It is automatically generated by /usr/local/sbin/update-grub using templates
# from /usr/local/etc/grub.d and settings from /usr/local/etc/default/grub
#
### BEGIN /usr/local/etc/grub.d/00_header ###
set default=0
set timeout=5
set root=(hd0,9)
terminal console
### END /usr/local/etc/grub.d/00_header ###
### BEGIN /usr/local/etc/grub.d/10_hurd ###
### END /usr/local/etc/grub.d/10_hurd ###
### BEGIN /usr/local/etc/grub.d/10_linux ###
menuentry "GNU/Linux, linux 2.6.31" {
linux (hd0,9)/boot/vmlinuz-2.6.31 root=/dev/sda9 ro 
initrd (hd0,9)/boot/initrd-2.6.31.img
}
menuentry "GNU/Linux, linux 2.6.31 (single-user mode)" {
linux (hd0,9)/boot/vmlinuz-2.6.31 root=/dev/sda9 ro single 
initrd (hd0,9)/boot/initrd-2.6.31.img
}
menuentry "GNU/Linux, linux 2.6.29.4-167.dt7.i586" {
linux (hd0,9)/boot/vmlinuz-2.6.29.4-167.dt7.i586 root=/dev/sda9 ro 
initrd (hd0,9)/boot/initrd-2.6.29.4-167.dt7.i586.img
}
menuentry "GNU/Linux, linux 2.6.29.4-167.dt7.i586 (single-user mode)" {
linux (hd0,9)/boot/vmlinuz-2.6.29.4-167.dt7.i586 root=/dev/sda9 ro single 
initrd (hd0,9)/boot/initrd-2.6.29.4-167.dt7.i586.img
}
menuentry "Windows XP" {
set root=(hd0,1)
chainloader +1
}
### END /usr/local/etc/grub.d/10_linux ###
```

其中：
```
menuentry "Windows XP" {
set root=(hd0,1)
chainloader +1
}
```
是我自行添加的，因为我是双系统，安装Grub2的时候不能引导Windows XP。因为我更新过内核因此就有2个版本的linux，每个版本又分单用户模式和普通模式，再加上Windows XP系统，所有就有5个选项。
/boot/grub/grub.cfg是只读文件，在修改前，需要更改权限，加入写入权限，命令如下：chmod +w /boot/grub/grub.cfg
注意：grub2中的分区是从(hd0,1)开始的。但是上个版本grub的分区是从（hd0,0）开始。因此这个要特别注意。