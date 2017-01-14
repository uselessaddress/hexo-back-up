title: Oracle实验记录——存储过程和函数
date: 2015-05-19 19:05:53
tags: [数据库,Oracle]
categories: 设计开发
---

### 创建、编译并运行PL/SQL存储过程
在SQL Developer中，创建、编译并运行PL/SQL存储过程
1、右击过程，创建过程
2、输入过程名“emp_list”
3、添加参数

| Name | Type | Mode | Default Value |
| ------ | ------- | ------ | ------ | 
| v_empno | VARCHAR2 | IN |  |
| v_ename | VARCHAR2 | OUT |  |

4、显示指定参数的过程的框架
```
CREATE OR REPLACE
PROCEDURE EMP_LIST
( v_empno IN VARCHAR2
, v_ename OUT VARCHAR2
) AS
BEGIN
  NULL;
END EMP_LIST;
```
5、替换NULL为：
```
select ename into v_ename from emp where empno = v_empno;
```
单击工具栏的save按钮，编译PL/SQL子程序。

6、运行PL/SQL过程
单击编译图标，过程成功编译。右击emp_list，选择运行。
该操作将调用“运行PL/SQL”对话框。运行PL/SQL对话框允许选择要运行的目标过程或函数（对程序包有用），并显示所选目标的参数列表。PL/SQL块文本区域中包含的SQL Developer用来调用所选程序的生成代码。使用该区域填充要传送到程序单元的参数及处理复杂的返回类型。
将 “V_EMPNO:=NULL;”更改为“V_EMPNO:=7369;”，然后单击确定。
可以看到运行日志：
```
连接到数据库 scott。
V_ENAME = SMITH
进程已退出。
从数据库 scott 断开连接。
```
<!--more-->
### 无参数的存储过程
创建一个存储过程，使用游标实现，每输出dept表的一条记录（deptno、dname、loc）后，诉后输出该部门的员工记录（empno、ename、sal）。

```
create or replace procedure query_dept_emp is
  type sp_emp_cursor is ref cursor;
  test_cursor1 sp_emp_cursor;
  test_cursor2 sp_emp_cursor;
  v_deptno dept.Deptno%type;
  v_dname dept.dname%type;
  v_loc dept.loc%type;
  v_empno emp.empno%type;
  v_ename emp.ename%type;
  v_sal emp.sal%type;
begin
  open test_cursor1 for select deptno,dname,loc from dept;
  loop
    fetch test_cursor1 into v_deptno, v_dname, v_loc;
    exit when test_cursor1%notfound;
    dbms_output.put_line('部门编号：'||v_deptno||'部门名称：'||v_dname||'部门位置：'|| v_loc);
    dbms_output.put_line('-----------------------------------------------');
    open test_cursor2 for select empno,ename,sal from emp where deptno = v_deptno;
    loop
      fetch test_cursor2 into v_empno, v_ename, v_sal;
      exit when test_cursor2%notfound;
      dbms_output.put_line(v_empno||'          '|| v_ename||'         '|| v_sal);
    end loop;
  end loop;
  close test_cursor1;
  close test_cursor2;
end;

exec query_dept_emp;

```

### 带in参数的存储过程
创建一个PL/SQL块，根据输入的部门编号，用游标实现逐条输出emp表中该部门每位员工的编号（empno）、姓名（ename）和工资（sal）信息。

```
create or replace procedure query_by_deptno(v_deptno in emp.Deptno%type) is
  type sp_emp_cursor is ref cursor;
  test_cursor sp_emp_cursor;
  v_empno emp.empno%type;
  v_ename emp.ename%type;
  v_sal emp.sal%type;
begin
  open test_cursor for select empno,ename,sal from emp where deptno = v_deptno;
  dbms_output.put_line('员工编号    姓名    工资');
  loop
    fetch test_cursor into v_empno, v_ename, v_sal;
    exit when test_cursor%notfound;
    dbms_output.put_line(v_empno||'    '|| v_ename||'    '|| v_sal);
  end loop;
  close test_cursor;
end;


exec query_by_deptno(10);

```
### 带输入in、输出out参数的存储过程
查询emp中给定职工号的姓名、工资和佣金。
```
create or replace
procedure query_emp
  (v_emp_no in emp.empno%type, 
  v_emp_name out emp.ename%type,
  v_emp_sal out emp.sal%type,
  v_emp_comm out emp.comm%type
  )
is
  begin
    select ename,sal,comm
    into v_emp_name, v_emp_sal, v_emp_comm
    from emp 
    where empno = v_emp_no;
end query_emp;

variable emp_name varchar2(15);
variable emp_sal number;
variable emp_comm number;
execute query_emp(7369,:emp_name,:emp_sal,:emp_comm);
print emp_name;

```
### 使用隐式游标SQL%NOTFOUND的存储过程
解雇给定职工号的职工。如果职工号7654的职工不存在则出错。为了避免出错可使用隐式游标SQL%NOTFOUND语句。
```
create or replace procedure fire_emp(v_emp_no in emp.empno%type)
is
begin
  delete from emp where empno = v_emp_no;
  if sql%notfound then
    dbms_output.put_line('雇员号为：'||v_emp_no||'的员工不存在！');
  else 
    dbms_output.put_line('已删除雇员号为：'|| v_emp_no || '的员工。');
  end if;
end fire_emp;


execute fire_emp(7654);
```

### 自定义函数
用Function查询出EMP中给定职工号的工资。
```
create or replace function get_sal
  (v_emp_no in emp.empno%type)
  return number 
is 
  v_emp_sal emp.sal%type:=0;
begin
  select sal into v_emp_sal
  from emp where empno = v_emp_no;
  return (v_emp_sal);
end get_sal;

variable emp_sal number;
execute:emp_sal:=get_sal('7900');
print emp_sal;
```
### 小结
这个实验肯定是和PL/SQL程序设计一伙的！感觉在设计开发中基本用不到。
本篇是Oracle实验记录的的最后一篇，书上还有一些内容，什么分区表的创建和使用、使用RMAN工具、闪回技术等等，需要时再去学习。