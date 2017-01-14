title: Oracle实验记录——创建和管理表空间
date: 2015-05-03 19:59:00
tags: [数据库,Oracle]
categories: 设计开发
---

### 创建临时表空间
```
create temporary tablespace user_temp  
tempfile 'D:\oracle\oradata\user_temp.dbf' 
size 10m  
autoextend on  
next 5m maxsize 50m  
extent management local;  
```
PS：临时表空间主要用于存储用户在执行order by等语句进行排序或汇总时产生的临时数据，它是所有用户公有的。默认情况下，所有用户都使用temp作为临时表空间。但是也允许使用其他表空间作为临时表空间，这需要在创建用户时进行指定。
<!--more-->
### 创建数据表空间
```
create tablespace user_data  
datafile 'D:\oracle\oradata\user_data.dbf' 
logging 
size 10m  
autoextend on  
next 5m maxsize 50m  
extent management local; 
```

### 创建用户并指定表空间 
```
create user voidking identified by voidking
default tablespace user_data  
temporary tablespace user_temp; 
``` 

### 修改用户的默认表空间
`alter user voidking default tablespace user_data;`

### 给用户授予权限
`grant connect,resource,dba to voidking;`


### 重命名表空间
`alter tablespace user_data rename to new_user_data;`

### 增加新的数据文件
```
alter tablespace user_data
add datafile 'D:\oracle\oradata\user_data2.dbf'
size 2m 
autoextend on 
next 1m 
maxsize unlimited;
```

### 删除表空间的数据文件
`alter tablespace user_data drop datafile 'D:\oracle\oradata\user_data2.dbf';`

### 设置数据文件的状态
数据文件主要有三种状态：online、offline、offline drop。
`alter database datafile 'D:\oracle\oradata\user_data.dbf' online;`

### 删除表空间
`drop tablespace user_data including contents and datafiles;`//删除表空间
`drop tablespace user_temp including contents and datafiles;`//无法删除，表空间被占用
`select username,temporary_tablespace from dba_users;`//查看user_temp是不是某些用户的默认临时表空间
`alter user voidking temporary tablespace user_temp2;`
`alter database default temporary tablespace user_temp2;`
`drop tablespace user_temp including contents and datafiles;`//成功删除

### 设置默认表空间
`alter database default temporary tablespace user_temp;`//设置默认临时表空间
`alter database default tablespace user_data;`//设置默认永久性表空间

### 查看表空间
1、查看表空间的名称和大小
```
SELECT t.tablespace_name, round(SUM(bytes / (1024 * 1024)), 0) ts_size 
FROM dba_tablespaces t, dba_data_files d 
WHERE t.tablespace_name = d.tablespace_name 
GROUP BY t.tablespace_name; 
```
或者
```
select tablespace_name ,sum(bytes) / 1024 / 1024 as MB　
from dba_data_files 
group by tablespace_name;
```


2、查看表空间物理文件的名称及大小 
```
SELECT tablespace_name, 
file_id, 
file_name, 
round(bytes / (1024 * 1024), 0) total_space 
FROM dba_data_files 
ORDER BY tablespace_name; 
```

3、查看所有表空间对应的文件

`select tablespace_name,file_name from dba_data_files;`

4、查看回滚段名称和大小
```
SELECT segment_name, 
tablespace_name, 
r.status, 
(initial_extent / 1024) initialextent, 
(next_extent / 1024) nextextent, 
max_extents, 
v.curext curextent 
FROM dba_rollback_segs r, v$rollstat v 
WHERE r.segment_id = v.usn(+) 
ORDER BY segment_name; 
```

5、查看控制文件
`SELECT NAME FROM v$controlfile; `

6、查看日志文件
`SELECT MEMBER FROM v$logfile; `

7、查看表空间的使用情况 
```
SELECT SUM(bytes) / (1024 * 1024) AS free_space, tablespace_name 
FROM dba_free_space 
GROUP BY tablespace_name; 
SELECT a.tablespace_name, 
a.bytes total, 
b.bytes used, 
c.bytes free, 
(b.bytes * 100) / a.bytes "% USED ", 
(c.bytes * 100) / a.bytes "% FREE " 
FROM sys.sm$ts_avail a, sys.sm$ts_used b, sys.sm$ts_free c 
WHERE a.tablespace_name = b.tablespace_name 
AND a.tablespace_name = c.tablespace_name; 
```

8、查看数据库库对象
```
SELECT owner, object_type, status, COUNT(*) count# 
FROM all_objects 
GROUP BY owner, object_type, status; 
```

9、查看数据库的版本
```
SELECT version 
FROM product_component_version 
WHERE substr(product, 1, 6) = 'Oracle'; 
```

10、查看数据库的创建日期和归档方式 
`SELECT created, log_mode, log_mode FROM v$database; `


### 修改数据文件大小
`alter database datafile 'D:\oracle\oradata\user_data.dbf'  resize 20m;`

### 使用OEM管理表空间
开始，所有程序，Oracle-OraDb11g_home1，Database Control-orcl。使用sys登陆，连接身份sysdba。服务器，存储，表空间。