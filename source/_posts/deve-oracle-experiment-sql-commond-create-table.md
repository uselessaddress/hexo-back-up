title: Oracle实验记录——利用SQL命令创建表
date: 2015-05-03 17:33:31
tags: 
- Oracle
- 数据库
categories: 设计开发
---

### 创建表
`create table userbase(username varchar2(8),passwd varchar2(16));`

### 插入一行
`insert into userbase values('voidking','voidking');`

### 查看数据
`select * from userbase;`

### 修改表
1、追加新的列
`alter table userbase add(realname varchar2(8));`
<!--more-->
2、修改现有的列、为新追加的列定义默认值
`alter table userbase modify(realname varchar2(8) default 'haojin');`
`insert into userbase(username, passwd) values('voidking','voidking');`
`select * from userbase;`

3、删除一个列
`alter table userbase drop column realname;`

4、禁用列
`alter table userbase set unused column passwd;`

5、重命名列
`alter table userbase rename column username to name;`

### 清空表
`truncate table userbase;`

### 删除表
`drop table userbase;`

### 添加注释
`comment on table userbase is '这张表用来测试';`

### 定义和管理数据完整性约束
1、非空约束
`create table userbase(username varchar2(8) not null,passwd varchar2(16));`

2、主键约束
`create table userbase(username varchar2(8) primary key,passwd varchar2(16));`
`alter table userbase add constraint primary_key primary key(username);`

3、唯一性约束
`create table userbase(username varchar2(8) primary key not null ,passwd varchar2(16) unique);`
`alter table userbase add constraint unique_value unique(passwd);`

4、外键约束
`create table userdetail(username varchar2(8) primary key not null ,realname varchar2(8));`
`alter table userbase add constraint fk_username foreign key(username) references userdetail(username);`

`insert into userdetail values('voidking','haojin');`
`insert into userbase values('voidking','voidking');`

`delete from userbase where username='voidking';`//不会删除userdetail的内容
`delete from userdetail where username='voidking';`//约束限制，无法删除

`alter table userbase add constraint fk_username foreign key(username) references userdetail(username) on delete cascade;`
`delete from userbase where username='voidking';`//不会删除userdetail的内容
`delete from userdetail where username='voidking';`//userdetail和userbase里的内容都被删除了

5、检查约束
`alter table userdetail add(sex varchar(4) default '男' check(sex='男' or sex='女'));`
`alter table userdetail add(sex varchar(4));`
`alter table userdetail modify(sex check(sex='男' or sex='女'));`

6、禁用和激活约束
`insert into userbase values('test','voidking');`//执行失败
`alter table userbase disable constraint fk_username;`
`insert into userbase values('test','voidking');`//执行成功
`alter table userbase enable constraint fk_username;`

7、删除约束
`alter table userdetail drop primary key;`//删除失败
`alter table userdetail drop primary key cascade;`//删除成功，同时删除了fk_username
`alter table userbase disable constraint fk_username;`

8、查看约束信息
`select constraint_name,constraint_type,status,validated from user_constraints where table_name='USERBASE';`
`select * from user_cons_columns where table_name='USERBASE';`

### 在sqldeveloper中创建表
开始，所有程序，Oracle-OraDb11g_home1，应用程序开发，SQL Developer。

### 在OEM中创建表
开始，所有程序，Oracle-OraDb11g_home1，Database Control-orcl。