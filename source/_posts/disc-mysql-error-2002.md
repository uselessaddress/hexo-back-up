---
title: "连接MySQL报错ERROR 2002 (HY000)"
toc: true
date: 2017-04-02 14:00:00
tags:
- mysql
categories: 点滴发现
---
# 问题描述
`mysql -u root -p`，然后输入密码，登录mysql，报错如下：

```
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)
```

<!--more-->

# 解决办法
1、查看mysql.sock文件位置
`find / -name mysql.sock`，结果为：

```
/data/db/3306/mysql.sock
```
和`/tmp/mysql.sock`不一致。

在`/tmp`下没有mysql.sock文件，依次执行`touch mysql.sock`，`chmod 666 mysql.sock`。

重启mysql，`service mysqld restart`，再次登录mysql，报错如下：
```
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (111)
```

2、找到mysql配置文件
`ps aux | grep mysql`，在结果中找到：
```
--defaults-file=/opt/mysql/my.cnf
```

3、查看mysql.sock配置
`more /opt/mysql/my.cnf | grep sock`，结果如下：
```
socket=/data/db/3306/mysql.sock
socket = /data/db/3306/mysql.sock
```

4、修改my.cnf
`vim /opt/mysql/my.cnf`，修改两个socket如下：
```
socket = /tmp/mysql.sock
socket = /tmp/mysql.sock
```

重启mysql，再次登录，成功！

# 书签
MySQL错误ERROR 2002 (HY000)
http://www.jb51.net/article/56952.htm