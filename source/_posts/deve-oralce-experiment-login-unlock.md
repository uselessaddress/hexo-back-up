title: Oracle实验记录——登陆解锁
date: 2015-03-26 21:48:28
tags: 
- Oracle
- 数据库
categories: 设计开发
---
1、使用SYSTEM登陆SQL Plus

2、通过数据字典dba_users，查看Oracle账户的锁定状态
`select username,account_status from dba_users;`

3、为scott账户解锁
`alter user scott account unlock;`

4、为scott账户设置口令
`alter user scott identified by tiger;`

5、查看现在scott的状态
`select username,account_status from dba_users where username='SCOTT';`

6、在SQL Plus中使用scott账户连接数据库
`conn scott/tiger;`

7、显示当前用户
`show user;`

8、查看表信息
`select empno,ename,job,sal from emp where sal<2500;`

9、列出缓冲区内容
`list`
或者`l`

10、编辑当前行
`change /epno/empno`
<!--more-->
11、运行当前命令
`run`
或者`/`

12、增加一行
`input`
`order by sal`

13、在一行上增加内容
`append  desc`注意，两个单词之间有两个空格

14、删除一行
`del`

15、用系统编辑器编辑命令
`edit`

16、保存命令（sql文件）
`save d:\hello.sql`

17、运行命令（sql文件）
`start d:\hello.sql`
或者`@ d:\hello.sql`

18、清空缓冲区
`clear buffer`

19、列出表结构
`desc emp`

