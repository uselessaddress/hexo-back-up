title: Oracle实验记录——PL/SQL程序设计
date: 2015-05-19 18:05:53
tags: [数据库,Oracle]
categories: 设计开发
toc: true
---

### PL/SQL块
创建一个PL/SQL块，利用替换变量输入职工号，查询该职工的工资，如果工资小于300元，那么把工资更改为加200元。如果没有该员工，则显示“没有该员工！”
```
select empno from emp;
```
记住一些empno，方便接下来的输入。

```
undefine 员工号;
declare
  v_sal emp.sal%type;
begin
  select sal into v_sal from emp where empno = &&员工号;
  if v_sal < 3000 then
  update emp set sal = sal + 200 where empno = &&员工号;
  end if;
  dbms_output.put_line('工资为：'||v_sal);
  exception when no_data_found then
    dbms_output.put_line('没有该员工！');
end;
```
运行上述代码，输入一个empno，比如7369，就可以在DBMS输出中看到结果，注意，需要先启用DBMS输出。
![](http://voidking.qiniudn.com/@/imgs/oracle/dbms.jpg)
<!--more-->
### 使用游标
为了处理select语句返回的多行数据，开发人员可以使用显式游标。使用显式游标表哭定义游标、打开游标、提取游标和关闭游标。

1、游标的使用
```
declare
  cursor emp_cursor is select ename,sal from emp where deptno=10;
  v_ename emp.ename%type;
  v_sal emp.sal%type;
begin
  open emp_cursor;
  loop
    fetch emp_cursor into v_ename,v_sal;
    exit when emp_cursor%notfound;
    dbms_output.put_line('姓名：'||v_ename||'，工资：'||v_sal);
  end loop;
  close emp_cursor;
end;
```

2、在游标中，使用fetch...bulk collect into语句提取所有数据
```
declare
  cursor emp_cursor is
    select ename from emp where deptno=10;
  type ename_table_type is table of emp.ename%type;
  ename_table ename_table_type;
begin
  open emp_cursor;
  fetch emp_cursor bulk collect into ename_table;
  for i in 1..ename_table.count loop
    dbms_output.put_line(ename_table(i));
  end loop;
  close emp_cursor;
end;
```

3、使用游标属性
```
declare 
  cursor emp_cursor is
    select ename from emp where deptno=10;
  type ename_table_type is table of emp.ename%type;
  ename_table ename_table_type;
begin
  if not emp_cursor%isopen then
    open emp_cursor;
  end if;
  fetch emp_cursor bulk collect into ename_table;
  dbms_output.put_line('提取的总行数：'||emp_cursor%rowcount);
  close emp_cursor;
end;
```
4、基于游标定义记录变量
5、带参数的游标
6、使用游标更新数据
7、使用游标删除数据
8、使用游标for循环
9、若类型REF游标
10、强类型REF游标

### 小结
PL/SQL程序设计有什么用处？不知道。只是感觉很复杂，也没有学习的欲望，等到确实有需求了再来学习吧！