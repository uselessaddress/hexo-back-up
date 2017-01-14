title: samble设置
date: 2013-07-19 17:30:28
tags: 
- Linux
- samble
categories: 精华转载
---

在安装samba增加用户，并且使用smbpasswd生成密码的时候出现如下错误： 
Failed to find entry for user test.
Failed to modify password entry for user test
说明是没有该用户，请使用 -a 参数
OPTIONS
       -a
           This option specifies that the username following should be added to the local smbpasswd file, with the new
           password typed (type <Enter> for the old password). This option is ignored if the username following
           already exists in the smbpasswd file and it is treated like a regular change password command. Note that
           the default passdb backends require the user to already exist in the system password file (usually
           /etc/passwd), else the request to add the user will fail.
           This option is only available when running smbpasswd as root.
解决方法：
加参数‘-a’：
`# smbpasswd  -a 用户`即可
<!--more-->
查看Samba 服务器的端口及防火墙；
查看这个有何用呢？有时你的防火墙可能会把smbd服务器的端口封掉，所以我们应该smbd服务器所占用的端口；下面查看中，我们知道smbd所占用的端口是139和445 ；
[root@localhost ~]# netstat -tlnp |grep smb
tcp        0      0 0.0.0.0:139                 0.0.0.0:*                   LISTEN      10639/smbd
tcp        0      0 0.0.0.0:445                 0.0.0.0:*                   LISTEN      10639/smbd
如果有防火墙，一定要把这两个端口打开。如果不知道怎么打开,把防火墙规则清掉也行；
[root@localhost ~]# iptables -F
或
[root@localhost ~]# /sbin/iptables -F



显示某台提供samba服务的服务器上的共享资源
`#smbclient -L //127.0.0.1 -U haojin`
使用共享资源
`#smbclient //127.0.0.1/haojin -U haojin`


smbclient命令说明

![shell command]  执行所用的SHELL命令，或让用户进入 SHELL提示符

cd [目录]	切换到服务器端的指定目录，如未指定，则 smbclient 返回当前本地目录

lcd [目录]	切换到客户端指定的目录；

dir 或ls  	列出当前目录下的文件；

exit 或quit 	退出smbclient	

get file1  file2  	从服务器上下载file1，并以文件名file2存在本地机上；如果不想改名，可以把file2省略

mget file1 file2 file3  filen 	 从服务器上下载多个文件；

md或mkdir 目录	在服务器上创建目录

rd或rmdir 目录		删除服务器上的目录

put file1 [file2]	向服务器上传一个文件file1,传到服务器上改名为file2；

mput file1 file2 filen     向服务器上传多个文件



更多设置编辑文件 /etc/samba/smb.conf



快速地将本地计算机中所有的网络映射连接断开
net use * /del

windows下切换samba用户
control userpasswords2




