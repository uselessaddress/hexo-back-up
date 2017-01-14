title: Oracle实验记录——数据库的安全性
date: 2015-05-04 14:42:56
tags: [数据库,Oracle]
categories: 设计开发
---

### 创建数据库表空间（永久表空间）
```
create tablespace test 
datafile 'D:\oracle\oradata\test.dbf'
size 100m
autoextend on next 100m
maxsize unlimited;

```
<!--more-->
### 创建临时表空间
```
create temporary tablespace test_temp 
tempfile 'D:\oracle\oradata\test_temp.dbf'
size 100m
autoextend on next 32m
maxsize 1024m;

```


### 创建用户
```
create user user1 
identified by user1
default tablespace test quota 10m  
on users account lock;
```
PS:创建用户时，如果没有指定默认表空间和临时表空间，则该用户将使用系统自带的users表空间作为默认表空间，自带的temp表空间作为临时表空间。

### 解锁用户
`alter user user1 account unlock;`

### 授予系统权限
`grant create session to user1;`
`grant create table to user1;`
`grant unlimited tablespace to user1;`

### 授予对象权限
`conn scott/tiger;`
`grant select on emp to user1;`
`grant update(ename) on emp to user1;`

### 创建角色
Oracle三个标准角色：
1、Connect：拥有CONNECT角色的用户，可以与服务器建立连接会话，不可以创建表。
2、Resource：RESOURCE角色允许用户创建表、过程、触发器、序列等权限。
3、DBA：DBA角色拥有所有的系统权限——包括无限制的空间限额和给其他用户授予各种权限的能力。
PS：如果新创建的用户需要登录到OEM，则必须给用户授予select_catalog_role角色后才可以登录，否则只有管理员身份的用户才可以登录到OEM。

`conn system/voidking`
`create role myrole;`
`grant create session to myrole;`
`grant create table to myrole;`
`grant myrole to user1;`

PS：有些系统权限比较特殊，是不能放到角色中的，如：unlimited tablaspace；这些权限很高、很特殊，必须直接赋予给用户。


### 回收用户的权限
`revoke create table from user1;`

### 删除用户
`drop user user1;`
`drop user user1 cascade;`//用户如果有其他对象，加上cascade连同该用户的所有对象一并删除。