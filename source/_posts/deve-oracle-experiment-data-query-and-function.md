title: Oracle实验记录——数据查询和函数的使用
date: 2015-05-05 14:13:50
tags: 
categories: 设计开发
---

我们使用scott用户的几张表来演示。

### 查看表结构
`desc emp;`

### 查询所有列
`select * from dept;`

### 查询指定列
`select ename, sal, job, deptno from emp;`
<!--more-->
### 取消重复行
`select distinct deptno, job from emp; `

### 统计行数
`select count(*) from emp;`

### 条件查询
1、查询SMITH所在部门，工作，薪水
`select deptno,job,sal from emp where ename = 'SMITH'; `
2、查询薪水大于3000的员工信息
`select * from emp where sal > 3000;`

### 显示每个雇员的年工资
`select sal*13+nvl(comm, 0)*13 "年薪" , ename, comm from emp;`

### 显示列别名
`select ename "姓名", sal*12 as "年收入" from emp; `

### ||
`select ename || ' is a ' || job from emp;`

### like
%表示0到多个字符，_表示任意单个字符。
1、显示首字符为S的员工姓名和工资？
`select ename,sal from emp where ename like 'S%';`

2、显示第三个字符为大写O的所有员工的姓名和工资
`select ename,sal from emp where ename like '__O%';`

### in
`select * from emp where empno in (7844, 7839,123,456);`

### is null
`select * from emp where mgr is null; `

### 逻辑操作符
查询工资高于500或者是岗位为 MANAGER 的雇员，同时还要满足他们的姓名首字母为大写的J。
`select * from emp where (sal >500 or job = 'MANAGER') and ename like 'J%';`

### order by
1、按照工资的从低到高的顺序显示雇员的信息
`select * from emp order by sal;`
2、按照部门号升序而雇员的工资降序排列 
`select * from emp order by deptno,sal desc;`
3、按年薪排序
`select ename, (sal+nvl(comm,0))*12 "年薪" from emp order by "年薪" asc;`

### 分组函数
1、显示所有员工中的最高工资和最低工资
`select max(sal),min(sal) from emp;`

2、最高工资那个人
`select ename,sal from emp where sal=(select max(sal) from emp);`

3、显示所有员工的平均工资和工资总和
`select avg(sal),sum(sal) from emp;`

4、计算总共有多少员工
`select count(*) from emp;`

### group by
1、显示每个部门的平均工资和最高工资
`select avg(sal),max(sal),deptno from emp group by deptno;`

2、显示每个部门的每种岗位的平均工资和最低工资
`select min(sal), avg(sal), deptno, job from emp group by deptno, job;`

### having
显示平均工资低于 2000的部门号和它的平均工资
`select avg(sal), max(sal), deptno from emp group by deptno having avg(sal) < 2000;`

### 多表查询
1、显示雇员名，雇员工资及所在部门的名字
`select e.ename, e.sal, d.dname from emp e, dept d where e.deptno = d.deptno;`

2、显示部门号为 10 的部门名、员工名和工资
`select d.dname, e.ename, e.sal from emp e, dept d where e.deptno = d.deptno and e.deptno = 10;`

3、显示各个员工的姓名，工资及工资的级别
`select e.ename, e.sal, s.grade from emp e, salgrade s where e.sal between s.losal and s.hisal;`

4、显示雇员名，雇员工资及所在部门的名字，并按部门排序
`select e.ename, e.sal, d.dname from emp e, dept d where e.deptno = d.deptno order by e.deptno;`

### 自连接
显示某个员工的上级领导的姓名
`select worker.ename, boss.ename from emp worker,emp boss where worker.mgr = boss.empno and worker.ename = 'FORD';`

### 单行子查询
1、查询出SMITH的部门号
`select deptno from emp where ename = 'SMITH';`

2、显示与SMITH同部门的所有员工
`select * from emp where deptno = (select deptno from emp where ename = 'SMITH');`

### 多行子查询
查询和部门10的工作相同的雇员的名字、岗位、工资、部门号
`select distinct job from emp where deptno = 10; `
`select * from emp where job in (select distinct job from emp where deptno = 10);`


### all
显示工资比部门 30的所有员工的工资高的员工的姓名、工资和部门号
`select ename, sal, deptno from emp where sal > all (select sal from emp where deptno = 30);`
`select ename, sal, deptno from emp where sal > (select max(sal) from emp where deptno = 30);`

### any
显示工资比部门30的任意一个员工的工资高的员工姓名、工资和部门号
`select ename, sal, deptno from emp where sal > any (select sal from emp where deptno = 30);`
`select ename, sal, deptno from emp where sal > (select min(sal) from emp where deptno = 30);`


### 多列子查询
查询与 SMITH 的部门和岗位完全相同的所有雇员
`select deptno, job from emp where ename = 'SMITH';`
`select * from emp where (deptno, job) = (select deptno, job from emp where ename = 'SMITH');`

### from子句中使用子查询
显示高于自己部门平均工资的员工的信息
`select deptno, avg(sal) mysal from emp group by deptno;`
```
select e.ename, e.deptno, e.sal, ds.mysal 
from emp e, (select deptno, avg(sal) mysal from emp group by deptno) ds 
where e.deptno = ds.deptno and e.sal > ds.mysal;
```