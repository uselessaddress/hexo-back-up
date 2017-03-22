title: mysql设置root用户密码
toc: true
date: 2015-04-30 09:47:04
tags: [数据库,mysql,root]
categories: 设计开发
---

# 设置mysql初始密码
一般mysql的root默认密码为空，如果之前没有设置过root密码，我们可以使用mysqladmin命令来修改root密码。
`net start mysql`
`mysqladmin -u root -p password 123456`

<!--more-->

# phpmyadmin修改密码
安装配置phpmyadmin，方法自行百度。

# mysql忘记密码
1、KILL掉系统里的MySQL进程；
2、用以下命令启动MySQL，以不检查权限的方式启动；
`mysqld_safe -skip-grant-tables &`
3、然后用空密码方式使用root用户登录 MySQL；
`mysql -u root`
4、修改root用户的密码；
`mysql> update mysql.user set password=PASSWORD('voidking') where User='root';`
`mysql> flush privileges;`
`mysql> quit;`
5、重新启动MySQL，就可以使用新密码登录了

# 允许远程访问
登录mysql控制台，然后依次输入：
`mysql> use mysql;`
`mysql> grant all privileges on *.* to 'root'@'%' identified by 'voidking' with grant option;`

# 参考文档
Mysql数据库中设置root密码的命令及方法
http://www.cnblogs.com/ylan2009/articles/2549713.html

