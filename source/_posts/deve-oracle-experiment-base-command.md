title: Oracle实验记录——基本命令
date: 2015-04-07 15:49:38
tags: 
- Oracle
- 数据库
categories: 设计开发
---

1、显示员工信息表emp的内容
`select * from emp;`

2、将sql命令存入emp.sql文件
`save d:\emp.sql`

3、清空缓存区
`del`

4、调入emp.sql文件
`get d:\emp.sql`

5、再次执行同样的sql命令
`@ d:\emp.sql`
或者`start d:\emp.sql`
<!--more-->
6、利用替换变量接受雇员的姓名
`select * from emp where ename=&cont_name;`

7、利用替换变量接受雇员的雇佣日期（格式：03-12月-81）
`select * from emp where hiredate='&d';`

8、定义替换变量，在sql语句里使用该替换变量，使用完后将该变量删除
`define salary=1800;`
`select * from emp where sal>&salary;`
`undefine salary`

9、将查询结果假脱机输出到文件d:\spool_temp.txt中
`spool d:\spool_temp.txt`
`select * from emp;`
`select * from dept;`
`spool off`

10、为列deptno设置标题为“部门代码”
`column deptno heading '部门代码'`
`select * from emp;`

11、为sal列定制格式。要求在每个值前加$符号作为前缀，并保留一个小数位
`column sal format $99,999.0`
`select * from emp;`

12、用“/”替换所有空值
`column comm null '/'`
`select * from emp;`

13、显示所有列的当前设置
`column`

14、清除所有列的设置
`clear column`

15、限制重复行
`select * from emp;`
`select * from emp order by deptno;`


可能有许多雇员属于同一个部门，因此在DepartmentID列将有重复值出现
`break on deptno;`
`select * from emp order by deptno;`

显示break的设置
`break`

清除break的设置
`clear break`

