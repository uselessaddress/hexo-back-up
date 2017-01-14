title: "vsftpd下错误之:500 OOPS"
date: 2013-11-28 20:22:02
tags: 
- Linux
- vsftpd
categories: 点滴发现
---

### 前言
vsftpd下错误之：500 OOPS。vsftpd是在Linux发行版中最推崇的一种FTP服务器程序，vsftpd的特点：小巧轻快、安全易用等。 这里主要讲的是如何解决vsftpd下错误之：500 OOPS　

### 详细问题：
我在用ftp IP 地址登录FTP服务器时，系统提示我输入用户名和密码，可是仍然提示：500 OOPS: child died.
Connection closed by remote host.

### 解决办法：
1、 查看 SELinux 的状态： sestatus -b | grep ftp
2、 在出现的结果中可以看到
ftp_home_dir off
tftpd_disable_trans off
之类。我们现在只要把其中之一设置为on就可以了。
3、 setsebool -P ftpd_disable_trans on 或者 setsebool -P ftp_home_dir on
4、 重启vsftpd： service vsftpd restart

### 再次登录
登录成功了。
试着上传一些文件来进行测试，看看是否有日志记录
默认的日志在/var/log/目录下面。
