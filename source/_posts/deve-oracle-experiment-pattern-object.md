title: Oracle实验记录——模式对象
date: 2015-05-05 18:05:53
tags: [数据库,Oracle]
categories: 设计开发
---

### 表 
表示数据库中最常用的模式对象，用户的数据在数据库中是以表的形式存储的。表通常由一个或多个列组成，每个列表示一个属性，而表中的一行则表示一条记录。

在创建表时可以为表指定存储空间，如果不指定，Oracle会将该表存储到默认表空间中。根据需要可以将表从一个表空间移动到另一个表空间。
`create table userbase(username varchar2(16),passwd varchar2(16));`
`alter table userbase move tablespace user_data;`

重命名表
`alter table userbase rename to newuserbase;`
或者
`rename userbase to newuserbase;`
<!--more-->
### 索引
索引是数据库中用于存放表的每一条记录的位置的一种对象，其主要目的是为了加快数据的读取速度和完整性检查。不过，创建索引需要占用许多存储空间，而且在向表中添加和删除记录时，数据库需要花费额外的开销来更新索引。
创建索引：
`create index index_username on userbase(username) tablespace user_data;`
删除索引：
`drop index index_username;`
创建唯一索引：
`create unique index unique_index_username on userbase(username) tablespace user_data;`
创建组合索引：
`create index complex_index_username on userbase(username,passwd) tablespace user_data;`
创建反向键索引：
`create index reverse_index_username on userbase(username) reverse tablespace user_data;`

### 视图
视图是一个虚拟表，它并不存储真实的数据，它的行和列的数据来自于定义视图的子查询语句中所引用的表，这些表通常也称为视图的基表。
视图可以建立在一个或多个表（或其他视图）上，它不占用实际的存储空间，只是在数据字典中保存它的定义信息。

`create table userdetail(username varchar2(16),realname varchar2(16));`
创建视图语法如下：
```
create or replace view view_user(v_username,v_passwd,v_realname) 
as select b.username,b.passwd,d.realname
from userbase b,userdetail d
where b.username=d.username;

```
以上命令如果提示没有创建视图权限，请使用system登陆，执行
`grant create view to scott;`


### 序列
在Oracle中，可以使用序列自动生成一个整数序列，主要用来自动为表中的数据类型的主键列提供有序的唯一值，这样就可以避免在向表中添加数据时，手工指定主键值。序列和表没有关系。

而且使用手工指定主键值这种方式时，由于主键值不允许重复，因此它要求操作人员在指定主键值时自己判断新添加的值是否已经存在，这显然是不可取的。

创建序列语法如下：
```
create sequence userbase_sequence
minvalue 1
maxvalue 1024
start with 1
increment by 1
cache 10;

```
删除序列：
`drop sequence userbase_sequence;`

添加id列：
`alter table add id int;`

插入数据：
`insert into userbase values('voidking','voidking',userbase_sequence.nextval);`

### 同义词
Oracle支持为表、索引或视图等模式对象定义别名，也就是为这些对象创建同义词。Oracle中的同义词主要分为如下两类。
1、公有同义词：在数据库中所有用户都可以使用。
2、私有同义词：由创建它的用户私人拥有。不过，用户可以控制其他用户是否有权使用自己的同义词。

创建同义词语法：
`create public synonym synonym_userbase for userbase;`

权限不够，使用system用户登录，执行
`grant create synonym to scott;`
