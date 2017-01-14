title: Fedora下几个命令
date: 2013-12-03 16:04:07
tags: 
- Linux
- Fedora
categories: 点滴发现
---

yum install gcc

yum install gcc-c++

yum install vim

rpm -ivh adobe-release-i386-1.0-1.noarch.rpm 

yum install flash-plugin

rpm -e clementine

rpm --rebuilddb

PS1='[\u@\h \W]\$'

pushd  popd

update-grub

chroot
<!--more-->

unzip *.zip


wget -c http://ftp.cross-lfs.org/pub/clfs/conglomeration/gcc/gcc-4.8.2-branch_update-1.patch


yum install p7zip*
7z x SOURCE\ 5.5.3.7z -o./source/

yum install lib* --setopt=protect_multilib=false --skip broken