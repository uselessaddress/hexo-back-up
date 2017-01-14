title: tar报错Not found in archive
date: 2013-11-12 20:35:44
tags: 
- Linux
- eclipse
- 错误
categories: 点滴发现
---

下载了一个eclipse，想把它解压到/usr目录
[root@localhost Downloads]# tar -zxvf eclipse-jee-indigo-SR2-linux-gtk-x86_64.tar.gz /usr
tar: /usr: Not found in archive
tar: Exiting with failure status due to previous errors

原因是因为压缩文件使用的相对路径 在当前目录下找不到 /usr目录，通过使用-C指定解压目录可解决此问题
tar -zxvf eclipse-jee-indigo-SR2-linux-gtk-x86_64.tar.gz -C /usr
tar -cvf - /Novel | tar -xvf - -C ~