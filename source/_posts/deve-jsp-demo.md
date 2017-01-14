title: JSP demo设计
date: 2014-02-19 20:10:24
tags:
- JSP
- JavaEE
categories: 设计开发
toc: true
---

### 需求分析
既然是留言系统，肯定要有用户登录，所有需要一个用户表（user）。字段包括：id、username和password。其中id设为自动增长的int型，并设为主键。username和password都设为varchar型。登录成功后要有个主界面，显示别人和自己的留言信息，那就应该有个留言表（message）。字段包括：id、userid、date、title、content。其中id设为自动增长的int型，并设为主键。userid是user表中的id，表明该条留言是该用户留的。date表示发布留言的时间，datetime型。title表示发布留言的标题，varchar型。content表示发布的内容，varchar型。

### 创建数据库和表
使用MySQL，scott用户。创建数据库“jsp”，创建表user、message。
<!--more-->
#### user表结构
| 字段名称 |  数据类型 |  主键  | 自增  | 允许为空 |   描述   |
| :----:   | :----:    | :----: | :----: | :----: | :----:   |
| id       |  int      | 是     |  增1   |        | ID号     |
| username | varchar(20) |      |        |        | 用户名   |
| password | varchar(20) |      |        |        | 密码     |

#### message表结构
| 字段名称 |  数据类型 |  主键  | 自增  | 允许为空 |   描述   |
| :----:   | :----:    | :----: | :----: | :----: | :----:   |
| id       | int       | 是     |  增1   |        | ID号     |
| userid   | int       |        |        |        | 用户ID号 |
| date     | datetime  |        |        |        | 发布时间 |
| title    | varchar(20) |      |        |        | 标题     |
| content  | varchar(500) |     |       |       | 留言内容 |

#### MySQL命令记录
root用户登录
```
mysql -u root -p
grant all privileges on *.* to scott@'localhost';
flush privileges;
exit
```
scott用户登录
```
create database jsp;
show databases;
use jsp;
create table user(id int,username varchar(20),password varchar(20));
create table message(id int,userid int ,date datetime,title varchar(20),content varchar(500));
alter table user modify int id primary key auto_increment;
alter table message modify id int primary key auto_increment;
alter table message add constraint fk_id foreign key(userid) references user(id);
desc user;
desc message;
```

### 创建项目
使用eclipse，新建Dynamic Web Project，命名为“jsp”。

### 创建表对应的JavaBean
新建包com.voidking.jsp.model，然后建立表对应的标准JavaBean：User和Message。
```
package com.voidking.jsp.model;

public class User {
	//Fields
	private Integer id;
	private String username;
	private String password;
	
	//Property accessors
	//属性 id 的 get/set 方法
	public Integer getId(){
		return this.id;
	}
	public void setId(Integer id){
		this.id=id;
	}
	//属性 username 的 get/set 方法
	public String getUsername(){
		return this.username;
	}
	public void setUsername(String username){
		this.username=username;
	}
	//属性 password 的 get/set 方法
	public String getPassword(){
		return this.password;
	}
	public void setPassword(String password){
		this.password=password;
	}

}


```

```
package com.voidking.jsp.model;

import java.sql.Date;
public class Message {
	//Fields
	private Integer id;
	private Integer userid;
	private Date date;
	private String title;
	private String content;

	
	//Property accessors
	//属性 id 的 get/set 方法
	public Integer getId(){
		return this.id;
		
	}
	public void setId(Integer id){
		this.id=id;
	}
	//属性 userId 的 get/set 方法
	public Integer getUserid(){
		return this.userid;
	}
	public void setUserid(Integer userid){
		this.userid=userid;
	}
	//属性 date 的 get/set 方法
	public Date getDate(){
		return this.date;
	}
	public void setDate(Date date){
		this.date=date;
	}
	//属性 title 的 get/set 方法
	public String getTitle(){
		return this.title;
	}
	public void setTitle(String title){
		this.title=title;
	}
	//属性 content 的 get/set 方法
	public String getContent(){
		return this.content;
	}
	public void setContent(String content){
		this.content=content;
	}
}


```
### 创建登录页面
在WebContent中新建login.jsp，代码如下：
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>简易留言板</title>
</head>
<body bgcolor="#E3E3E3">
<form action="mainServlet" method="post">
<table>
	<caption>用户登录</caption>
	<tr>
		<td>
			用户名：<input type="text" name="username" size="20"/>
		</td>
	</tr>
	<tr>
		<td>
			密&nbsp;&nbsp;码：<input type="password" name="password" size="21"/>
		</td>
	</tr>
	<tr>
		<td>
			<input type="submit" value="登录"/>
			<input type="reset" value="重置"/>
		</td>
	</tr>
</table>
</form>
如果没注册单击<a href="register.jsp">这里</a>注册！
</body>
</html>
```
### 创建DB类
新建包com.voidking.jsp.db，新建类DB，代码如下：
```
package com.voidking.jsp.db;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.ArrayList;

import com.voidking.jsp.model.Message;
import com.voidking.jsp.model.User;

public class DB {

	Connection ct;
	PreparedStatement pstmt;
	
	public DB()
	{
		try {
			Class.forName("com.mysql.jdbc.Driver");
			ct=DriverManager.getConnection("jdbc:mysql://localhost/jsp","scott","tiger");
		} catch (Exception e) {
			e.printStackTrace();
		}
		
	}
	
	public User checkUser(String username,String password)
	{
		try {
			pstmt = ct.prepareStatement("select * from user where username=? and password=?");
			pstmt.setString(1, username);
			pstmt.setString(2, password);
			
			ResultSet rs=pstmt.executeQuery();
			User user = new User();
			while (rs.next()) {
				user.setId(rs.getInt(1));
				user.setUsername(rs.getString(2));
				user.setPassword(rs.getString(3));
				return user;
				
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
		return null;	
	}
	
	public ArrayList findMessage()
	{
		
		try {
			ArrayList al = new ArrayList();
			pstmt= ct.prepareStatement("select * from message");
			ResultSet rs = pstmt.executeQuery();
			while(rs.next())
			{
				Message message = new Message();
				message.setId(rs.getInt(1));
				message.setUserid(rs.getInt(2));
				message.setDate(rs.getDate(3));
				message.setTitle(rs.getString(4));
				message.setContent(rs.getString(5));
				al.add(message);
				
			}
			return al;
		} catch (Exception e) {
			e.printStackTrace();
		}
		return null;	
		
	}
	
	public String getUsername(int id)
	{
		String username = null;
		try {
			pstmt = ct.prepareStatement("select username from user where id=?");
			pstmt.setInt(1,id);
			ResultSet rs = pstmt.executeQuery();
			while(rs.next())
			{
				username = rs.getString(1);			
				
			}
			return username;
		} catch (Exception e) {
			e.printStackTrace();
		}
		return username;
		
	}
	
	public boolean addMessage(Message message)
	{
		try {
			pstmt= ct.prepareStatement("insert into message(userid,date,title,content) values(?,?,?,?)");
			pstmt.setInt(1, message.getUserid());
			pstmt.setDate(2,message.getDate());
			pstmt.setString(3, message.getTitle());
			pstmt.setString(4, message.getContent());
			
			pstmt.executeUpdate();
			return true;
		} catch (Exception e) {
			e.printStackTrace();
		}
		
		return false;	
		
	}	
	
	public boolean insertUser(String username,String password)
	{
		try {
			pstmt = ct.prepareStatement("insert into user(username,password) values(?,?)");
			pstmt.setString(1, username);
			pstmt.setString(2, password);
			pstmt.executeUpdate();
			return true;
		} catch (Exception e) {
			e.printStackTrace();
		}
		return false;		
		
	}
}


```
### 创建MainServlet类
新建包com.voidking.jsp.servlet，新建类MainServlet，代码如下：
```
package com.voidking.jsp.servlet;

import java.io.IOException;
import java.util.ArrayList;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import com.voidking.jsp.db.DB;
import com.voidking.jsp.model.Message;
import com.voidking.jsp.model.User;

public class MainServlet extends HttpServlet {

	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		request.setCharacterEncoding("utf8");
		response.setContentType("utf8");
		String username = request.getParameter("username");
		String password = request.getParameter("password");

		DB db = new DB();

		HttpSession session = request.getSession();

		User user = (User) session.getAttribute("user");
		if (user == null) {
			user = db.checkUser(username, password);

		}

		session.setAttribute("user", user);

		if (user != null) {

			ArrayList al = db.findMessage();
			session.setAttribute("al", al);
			response.sendRedirect("main.jsp");

		} else {
			response.sendRedirect("login.jsp");
		}
	}

	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		doGet(request, response);
	}

}

```

### 创建mian.jsp
在WebContent中新建main.jsp，代码如下：
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="com.voidking.jsp.model.Message" %>
<%@ page import="com.voidking.jsp.model.User" %>
<%@ page import="com.voidking.jsp.db.DB"%>
<%@page import="java.util.Iterator"%>
<%@page import="java.util.ArrayList"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>留言板信息</title>
</head>
<body bgcolor="#E3E3E3">
<% User user = (User)session.getAttribute("user");	
%>
	当前用户为：<%=user.getUsername() %>
	<form action="liuyan.jsp" method="post">

		<table border="1">
			<caption>所有留言信息</caption>
			<tr>
				<th>留言人姓名</th><th>留言时间</th><th>留言标题</th><th>留言内容</th>
			</tr>
		<%
			ArrayList al = (ArrayList)session.getAttribute("al");	
			if(al != null)
			{
				Iterator iter = al.iterator();
				while(iter.hasNext())
				{
					Message message=(Message)iter.next();
		%>						
			<tr>
				<td><%=new DB().getUsername(message.getUserid())%></td>
				<td><%=message.getDate().toString()%></td>
				<td><%=message.getTitle()%></td>
				<td><%=message.getContent()%></td>
			</tr>
		<%
				}
			}
		%>
		</table>
		<input type="submit" value="留言"/>
	</form>
</body>
</html>
```
### 创建liuyan.jsp
在WebContent中新建liuyan.jsp，内容如下：
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>留言板</title>
</head>
<body bgcolor="#E3E3E3">
	<center>
		<form action = "addServlet" method="post">
			<table border="1">
				<caption>填写留言信息</caption>
				<tr>
					<td>留言标题</td>
					<td><input type="text" name="title"/></td>
				</tr>
				<tr>
					<td>留言内容</td>
					<td><textarea rows="5" cols="35" name="content"></textarea></td>
				</tr>
			</table>
			<input type="submit" value="提交"/>
			<input type="reset" value="重置" />
		</form>	
	</center>

</body>
</html>
```
### 创建AddServlet类
在包com.voidking.jsp.servlet中，新建类AddServlet，内容如下：
```
package com.voidking.jsp.servlet;

import java.io.IOException;
import java.sql.Date;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.voidking.jsp.db.DB;
import com.voidking.jsp.model.Message;
import com.voidking.jsp.model.User;

public class AddServlet extends HttpServlet {

	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		request.setCharacterEncoding("utf8");
		response.setCharacterEncoding("utf8");

		String title = request.getParameter("title");
		String content = request.getParameter("content");
		User user = (User) request.getSession().getAttribute("user");

		Message message = new Message();
		message.setUserid(user.getId());
		message.setDate(new Date(System.currentTimeMillis()));
		message.setTitle(title);
		message.setContent(content);

		if (new DB().addMessage(message)) {
			response.sendRedirect("success.jsp");

		}
	}

	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		doGet(request, response);
	}
}

```
### 创建成功页面
在WebContent中新建success.jsp，内容如下：
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>留言成功</title>
</head>
<body bgcolor="#E3E3E3">

	留言成功，单击<a href="mainServlet">这里</a>返回主界面。
</body>
</html>

```
### 配置web.xml
在WebContent中新建web.xml，内容如下：
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5" 
	xmlns="http://java.sun.com/xml/ns/javaee" 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
	http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
  <display-name></display-name>	
  <welcome-file-list>
    <welcome-file>login.jsp</welcome-file>
  </welcome-file-list>
  
  <servlet>
  	<servlet-name>mainServlet</servlet-name>
  	<servlet-class>com.voidking.jsp.servlet.MainServlet</servlet-class>
  </servlet>
  
  <servlet-mapping>
  	<servlet-name>mainServlet</servlet-name>
  	<url-pattern>/mainServlet</url-pattern>
  </servlet-mapping>
  
  <servlet>
  	<servlet-name>addServlet</servlet-name>
  	<servlet-class>com.voidking.jsp.servlet.AddServlet</servlet-class>
  </servlet>
  
  <servlet-mapping>
  	<servlet-name>addServlet</servlet-name>
  	<url-pattern>/addServlet</url-pattern>
  </servlet-mapping>
  
  <servlet>
  	<servlet-name>registerServlet</servlet-name>
  	<servlet-class>com.voidking.jsp.servlet.RegisterServlet</servlet-class>
  </servlet>
  
  <servlet-mapping>
  	<servlet-name>registerServlet</servlet-name>
  	<url-pattern>/registerServlet</url-pattern>
  </servlet-mapping>
</web-app>
```

### 创建注册页面
在WebContent中新建register.jsp，代码如下：
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>用户注册</title>
</head>
<body bgcolor="#E3E3E3">
	<form action="registerServlet" method="post">
		<table>
			<caption>用户注册</caption>
			<tr>
				<td>登录名</td>
				<td><input type="text" name="username"/></td>
			</tr>
			<tr>
				<td>密码：</td>
				<td><input type="password" name="password"/></td>
			</tr>
		</table>
		<input type="submit" value="注册" />
		<input type="reset" value="重置"/>
	</form>

</body>
</html>
```
### 创建RegisterServlet类
在包com.voidking.jsp.servlet下，新建类RegisterServlet，内容如下：
```
package com.voidking.jsp.servlet;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.voidking.jsp.db.DB;

public class RegisterServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		request.setCharacterEncoding("utf8");
		response.setCharacterEncoding("utf8");

		String username = request.getParameter("username");
		String password = request.getParameter("password");

		if (new DB().insertUser(username, password)) {
			response.sendRedirect("login.jsp");

		}
	}

	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		doGet(request, response);
	}
}

```
### 部署和运行
拷贝mysql-connector-java-*-bin.jar到WebContent/WEB-INF/lib。
发布工程，启动Tomcat服务器，访问地址：http://localhost:8080/jsp/login.jsp

### 编码问题
经过调试修改，登录、注册、留言，功能正常。但是，查看留言时，中文留言在界面上显示问号。
在数据库中查询，也显示问号，那么问题出在哪里呢？

#### 查看MySQL默认编码
```
status;
show variables like 'character%';
```
发现结果如下：

| Variable_name            | Value                                 |
| ------                   | ------                                |
| character_set_client     | gbk                                   |
| character_set_connection | gbk                                   |
| character_set_database   | latin1                                |
| character_set_filesystem | binary                                |
| character_set_results    | gbk                                   |
| character_set_server     | latin1                                |
| character_set_system     | utf8                                  |
| character_sets_dir       | d:\server\xampp\mysql\share\charsets\ |
推测是数据库编码问题，全部修改成utf8应该就可以了。


#### 修改无效
1、由于我的MySQL是XAMPP的一部分，比较精简，所以在bin文件夹下没有MySQLInstanceConfig.exe。没有办法利用这个工具重新配置。

2、修改配置文件。在my.ini中添加：
```
[mysqld]
default-character-set=utf8
character_set_server=utf8
init_connect='SET NAMES utf8'
```
重启MySQL，完全没有效果，靠！

3、通过命令配置
```
//create database jsp character set utf8;
alter database jsp character set utf8;//在没有table的情况下，此命令才有效。
set names utf8;  
```
经过测试，还是显示乱码。查看编码如下：

| Variable_name | Value |
| ------ | ------ |
| character_set_client| utf8 |
| character_set_connection | utf8 |
| character_set_database   | utf8 |
| character_set_filesystem | binary |
| character_set_results    | utf8 |
| character_set_server     | latin1 |
| character_set_system     | utf8 |
| character_sets_dir       | d:\server\xampp\mysql\share\charsets\ |
看来character_set_server的值是重点啊！

#### 重装
备份数据库。复制D:\Server\xampp\mysql\data路径下的数据库文件夹以及ibdata1文件，待会放到新的MySQL的data目录下。

卸载MySQL，使用绿色版安装，提示服务已存在，无法安装。最终，使用了windows安装版。注意，配置数据库的时候，一定要选择utf8编码。安装成功，拷贝数据库文件到新的data下，结果，MySQL无法启动。推测是因为新安装的32位，而原数据库是64位。

没办法，重新建立用户，重新建立数据库和表。经过测试，可以正常显示中文！重装治百病，很有道理！

### 源代码分享
https://github.com/voidking/jsp.git

### 小结
书上给的代码有几处错误，修改了之后，才最终跑起来。书本，确实只是用来参考就够了。
编程能力是写出来的！在一行行敲代码的过程中，会遇到很多问题，而解决这些问题的过程，培养出来的，就是编程能力！


### 参考文档
《Java EE基础实用教程》，郑阿奇主编
mysql添加外键：http://www.cnblogs.com/xiangxiaodong/archive/2013/05/05/3061049.html


