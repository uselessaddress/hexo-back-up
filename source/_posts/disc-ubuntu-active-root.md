title: Ubuntu下激活root账号
date: 2013-07-16 20:24:11
tags: 
- Linux
- Ubuntu
- root
categories: 点滴发现
---

激活root帐号：
在安装系统时，root账户并没有被激活来供你使用，即root帐号被隐藏了，而是通过初始用户与sudo的结合使用来完成一些需要root权限的任务。这样做的好处是防止你不得不使用root来进行一些系统的初级管理，同时完全允许另一个账户来充当超级用户，也保护了你系统的安全方面的缺陷。
如果你需要使用root用户来完成一些工作的话，使用以下命令激活root用户：

法一：
在终端中输入：`sudo passwd root`
之后要求你输入两次root用户的密码，重启后就可以登陆root用户了。
退出root权限方法：`exit`
若想禁用 root 帐号： `sudo passwd -l root`

法二：
1、重启电脑，选择recovery模式
2、找到最下边的root选项
3、在recovery模式的root用户下创建一个root用户，输入：`passwd root`
4、出现****UNIX****提示，直接输入你要给root用户设置的密码
5、会再次出现******UNIX******的提示，是提示你再次输入密码
 重启后就可以进入root用户了。