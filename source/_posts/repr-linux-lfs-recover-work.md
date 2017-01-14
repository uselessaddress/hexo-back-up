title: LFS恢复工作状态
date: 2013-10-19 14:36:19
tags: 
- Linux
- LFS
categories: 精华转载
---

LFS(Linux From Scratch)

方法一：
1.重新启动计算机，并从LiveCD启动
2.加载分区
export LFS=/mnt/lfs
mkdir -pv $LFS
mount /dev/sda2 $LFS
3.加载交换分区（如果不想用交换分区或者没有交换分区可跳过此步骤）
swapon /dev/sda1
4.建立工具链的链接
ln -sv $LFS/tools /
5.创建lfs用户
groupadd lfs
useradd -s /bin/bash -g lfs -m -k /dev/null lfs
passwd lfs
chown -v lfs $LFS/tools
chown -v lfs $LFS/sources
su - lfs
6.建立lfs用户的环境
cat > ~/.bash_profile  ~/.bashrc 
<!--more-->
方法二：
1.重新启动计算机，并从LiveCD启动
2.加载分区
export LFS=/mnt/lfs
mkdir -pv $LFS
mount /dev/hda2 $LFS
3.加载交换分区（如果不想用交换分区或者没有交换分区可跳过此步骤）
swapon /dev/hda1
4.加载必要的文件系统
mount -v --bind /dev $LFS/dev
mount -vt devpts devpts $LFS/dev/pts
mount -vt tmpfs shm $LFS/dev/shm
mount -vt proc proc $LFS/proc
mount -vt sysfs sysfs $LFS/sys
5.Chroot到目标系统下
chroot "$LFS" /tools/bin/env -i \
HOME=/root TERM="$TERM" PS1='\u:\w\$ ' \
PATH=/bin:/usr/bin:/sbin:/usr/sbin:/tools/bin \
/bin/bash --login +h
6.进入编译目录
cd /sources
export LFS=/sources

