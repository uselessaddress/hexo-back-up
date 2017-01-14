title: Oracle实验记录——数据的导入和导出
date: 2015-04-07 16:36:31
tags: 
- Oracle
- 数据库
categories: 设计开发
---

以下命令，都是在操作系统命令行中使用。

### EXP数据导出
1、full方式：导出整个数据库
`exp system/Admin123 file=d:/test_1.dmp full=y`

2、owner方式：导出指定的用户模式
`exp scott/tiger file=d:/test_2.dmp owner=scott`

3、tables方式：导出指定的表
`exp scott/tiger file=d:/test_3.dmp tables=(emp,dept)`
<!--more-->
### IMP数据导入
1、full方式：导入整个数据库
`imp system/Admin123 file=d:/test_1.dmp full=y`

2、owner方式：导入指定的用户模式
`imp scott/tiger file=d:/test_2.dmp owner=scott`

3、tables方式：导入指定的表
`imp scott/tiger file=d:/test_3.dmp tables=emp,dept`

### EXPDP数据泵导出

#### 数据泵工具导出的步骤
1、启动sql plus，以system/manager 登陆
`conn system/manager as sysdba`

2、创建目录对象（Tips：手动在D盘下创建一个temp文件夹）
`create directory mypump as 'D:\temp';`

3、授权
`grant read,write on directory mypump to scott;`

4、在操作系统命令行中，执行导出
`expdp scott/tiger schemas=scott directory=mypump dumpfile=expdp_test1.dmp;`

#### 数据泵导出的各种模式
1、tables：指定表模式导出
`expdp scott/tiger tables=emp,dept directory=mypump dumpfile=expdp_test2.dmp`

2、按条件查询导出
`expdp scott/tiger tables=emp directory=mypump dumpfile=exopdp_test3.dmp query='"where sal < 2000"'`

3、tablespaces：指定要导出表空间列表
`expdp system/Admin123 directory=mypump dumpfile=expdp_tablespace.dmp tablespaces=users`

4、schemas：指定用户模式导出
`expdp scott/tiger schemas=scott directory=mypump dumpfile=expdp_test1.dmp`

5、full：指定整个数据库模式导出
`expdp system/Admin123 directory=mypump dumpfile=full.dmp full=y`

6、使用exclude，include导出数据
使用include选项，导出用户中包括指定类型的指定对象
导出scott用户下以B开头的所有表
`expdp scott/tiger directory=mypump dumpfile=include_1.dmp include=table:\"like\'B%\'\"`

导出scott用户下的所有存储过程
`expdp scott/tiger directory=mypump dumpfile=include_2.dmp schemas=scott include=procedure`

exclude导出用户中排除指定类型的指定对象
导出scott用户下除view类型以外的所有对象
`expdp scott/tiger directory=mypump dumpfile=exclude_1.dmp schemas=scott exclude=view`

### IMPDP数据泵导入
1、tables：指定表模式导入
`impdp scott/tiger tables=emp,dept directory=mypump dumpfile=expdp_test2.dmp`

2、schemas：指定用户模式导入
`impdp scott/tiger schemas=scott directory=mypump dumpfile=expdp_test1.dmp`