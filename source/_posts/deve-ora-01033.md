title: ORA-01033
date: 2013-09-09 11:15:55
tags: [数据库,Oracle,错误]
categories: 设计开发
---
转载自[百度百科](http://baike.baidu.com/link?url=1WUrctXEk6JCGhL4YzXURaVjKvwvfVqyagyG8Pp8SJscZs_2m9st0Jd4PuQaMArcBQeSm448O1p2WW-0HV2O0_)

1、进入CMD，执行set ORACLE_SID=ORCL，确保连接到正确的SID；
2、命令窗口运行sqlplus "/as sysdba" 启动窗口之后显示如下信息

SQL*Plus: Release 11.1.0.7.0 - Production on 星期三 3月 6 17:17:53 2013
Copyright (c) 1982, 2008, Oracle. All rights reserved.

连接到:
Oracle Database 11g Enterprise Edition Release 11.1.0.7.0 - Production
With the Partitioning, OLAP, Data Mining and Real Application Testing options
SQL>
3、停止服务 shutdown immediate
SQL> shutdown immediate
ORA-01109: 数据库未打开
<!--more-->
已经卸载数据库。
ORACLE 例程已经关闭。
4、 启动服务 startup 观察启动时有无数据文件加载报错，并记住出错数据文件标号
SQL> startup
ORACLE 例程已经启动。
Total System Global Area 535662592 bytes
Fixed Size 1348508 bytes
Variable Size 272632932 bytes
Database Buffers 255852544 bytes
Redo Buffers 5828608 bytes
数据库装载完毕。
ORA-16038: 日志 2 sequence# 59 无法归档
ORA-19809: 超出了恢复文件数的限制
ORA-00312: 联机日志 2 线程 1: 'D:\APP\EN\ORADATA\ORCL\REDO02.LOG'

5、检查出错日志所在的组
SQL> select group#,sequence#,archived,status from v$log;
GROUP# SEQUENCE# ARC STATUS
---------- ---------- --- ----------------
1 61 NO CURRENT
3 60 NO INACTIVE
2 59 NO INACTIVE
6、修复出错的组日志信息
SQL> alter database clear unarchived logfile group 2;
数据库已更改。
7、打开数据库
SQL> alter database open;
数据库已更改。
SQL>