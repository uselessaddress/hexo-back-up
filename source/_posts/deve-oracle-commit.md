title: Oracle之commit
date: 2015-05-28 15:10:47
tags:
- 数据库
- Oracle
- 问题
categories: 设计开发
---

## Oracle之commit

### 前言
Oracle最后一次实验，小编打算移植SQL Server上的一个项目到Oracle。因为使用了Maven+JPA+Spring+Struts2+AngularJs，所以，虽然复杂了一点，但是可移植性非常好。修改一个database-conn.properties配置文件，在pom.xml中添加Oracle驱动即可。

database-conn.properties原文件：
```
database.driver=com.microsoft.sqlserver.jdbc.SQLServerDriver
database.uri=jdbc:sqlserver://localhost:1433;databaseName=design;integratedSecurity=false
database.username=sa
database.password=123


#hibernate
hibernate.dialect=org.hibernate.dialect.SQLServer2008Dialect
hibernate.show_sql=true
```

<!--more-->
database-conn.properties修改后文件：
```
database.name=Oracle
database.driver=oracle.jdbc.OracleDriver
database.uri=jdbc:oracle:thin:@localhost:1521:orcl
database.username=scott
database.password=tiger
 
#hibernate
hibernate.dialect=org.hibernate.dialect.OracleDialect
hibernate.show_sql=true
```

使用Maven把ojdbc14.jar安装到本地仓库，pom.xml添加：
```
<!-- Oracle驱动 -->
<dependency>
	<groupId>com.oracle</groupId>
	<artifactId>ojdbc14</artifactId>
	<version>10.2.0.3.0</version>
</dependency>
```
大功告成，clean install，jetty:run。

### 问题描述及解答
在Oracle的adminbase中插入一条数据：
```
insert into adminbase values(1,'voidking','voidking');
select * from adminbase;
```
两条命令都正常。然后在网页上使用账号密码登录，结果提示，用户名或密码错误。啊勒，怎么回事？莫非是因为Oracle的编码方式不对？修改完字符编码方式，问题没有解决。但是，我仍然倔强的认为，应该是编码方式的问题，因为以前在这方面吃的亏太多了。

上面的命令都是在sqldeveloper中执行的，换到sql plus试试。
```
select * from adminbase;
```
我靠，问题来了！结果显示`未选定行`！这是肿么回事？我好像知道为什么无法登录了，因为根本找不到数据哇！

经过查找资料，终于明白，在pl/sql developer插入数据后，需要commit（提交）。在SQL Plus中才能看到这种改变，因为二者处于两个不同的会话中。

在sqldeveloper中执行`commit`，再次登录，果然成功！

PS：字符编码查看命令：
```
select userenv('language') from dual;
select * from nls_database_parameters;
```

### 新的问题
不报错，但是不可以插入数据。比如添加书籍种类，输入信息后，点击“添加”，之后便没有反应了。理想的效果是，界面出现添加成功的信息。
控制台并没有报错信息，只是提示：
```
Hibernate: select hibernate_sequence.nextval from dual
Hibernate: insert into readerkind (enddate, name, notice, quantity, validity, id) values (?, ?, ?, ?, ?, ?)
```
不知道哪里出现了问题，也许，从SQLServer移植到Oracle，没有想象的那么简单。。。问题待解决。。。



### 参考文档
Oracle 字符集的查看和修改 
http://www.cnblogs.com/rootq/articles/2049324.html
http://blog.chinaunix.net/uid-25492475-id-3140218.html

------
使用pl/sqldeveloper和使用sql*plus得到的结果不同
http://www.educity.cn/wenda/410219.html

------

