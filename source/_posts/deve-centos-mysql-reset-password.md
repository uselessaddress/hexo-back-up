---
title: CentOS中MySQL重置密码
toc: false
date: 2016-11-20 11:24:57
tags:
- mysql
- centos
categories: 设计开发
---
# 问题描述
在centos中，安装mysql时没有设置mysql密码。登录mysql，`mysql -u root -p`，两次回车，结果提示，“ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password:NO)”。

<!--more-->

# 解决办法
1、停止mysql服务，`service mysqld stop`。   
2、进入/usr/local/mysql/bin文件夹，启用安全模式，`./mysqld_safe --skip-grant-tables &`。
3、使用root用户登录mysql，`mysql -u root`。
4、修改root用户密码：
```
mysql> use mysql;
Database changed
mysql> select * from  user;
Empty set (0.00 sec)
mysql> truncate table user;
Query OK, 0 rows affected (0.00 sec)
mysql> flush privileges;
Query OK, 0 rows affected (0.01 sec)
mysql> grant all privileges on *.* to root@localhost identified by 'voidking' with grant option;
Query OK, 0 rows affected (0.01 sec)
```

如果想设置密码为空，那么：
```
mysql> grant all privileges on *.* to root@localhost identified by '' with grant option;
Query OK, 0 rows affected (0.01 sec)*
mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)
```

确认结果：
```
mysql> select host, user from user;
+-----------+------+
| host      | user |
+-----------+------+
| localhost | root |
+-----------+------+
1 row in set (0.00 sec)
mysql> quit;
```

5、启动mysql服务：
```
ps -e | grep mysql
kill -KILL [PID of mysqld_safe]
kill -KILL [PID of mysqld]
service mysqld start
```

6、登录mysql，`mysql -u root -p`，回车后输入新设置的密码“voidking”即可。


# 书签
Access denied for user 'root@localhost' (using password:NO)
http://stackoverflow.com/questions/2995054/access-denied-for-user-rootlocalhost-using-passwordno