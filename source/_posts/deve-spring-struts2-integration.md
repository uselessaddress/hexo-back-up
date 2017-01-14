title: Spring与Struts2整合应用
date: 2014-02-22 16:10:24
tags: 
- Spring
- Struts2
- JavaEE
categories: 设计开发
toc: true
---

### 新建项目
新建Dynamic Web Project，命名为struts2_spring。

### 添加struts2框架
添加struts2五个核心jar包到lib文件夹。


### web.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app id="WebApp_9" version="2.4" xmlns="http://java.sun.com/xml/ns/j2ee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
	<welcome-file-list>
		<welcome-file>/login.jsp</welcome-file>
	</welcome-file-list>
    <filter>
        <filter-name>struts2</filter-name>
        <filter-class>org.apache.struts2.dispatcher.FilterDispatcher</filter-class>
    </filter>

    <filter-mapping>
        <filter-name>struts2</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

</web-app>

```

<!--more-->
### login.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="/struts-tags" prefix="s" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>登录界面</title>
</head>
<body>
	<s:form action="login.action" method="post">
		<s:textfield name="xh" label="学号"></s:textfield>
		<s:password name="kl" label="口令"></s:password>
	</s:form>
</body>
</html>
```

### LoginAction.java
```
package com.voidking.struts2_spring.action;

import com.opensymphony.xwork2.ActionSupport;

public class LoginAction extends ActionSupport{
	private String xh;
	private String kl;
	public String getXh() {
		return xh;
	}
	public void setXh(String xh) {
		this.xh = xh;
	}
	public String getKl() {
		return kl;
	}
	public void setKl(String kl) {
		this.kl = kl;
	}
	
	public String execute() throws Exception{
		return SUCCESS;
	}
}

```

### struts.xml
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
    "http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
	<include file="struts-default.xml"></include>
	<package name="default" extends="struts-default">
		<action name="login" class="com.voidking.struts2_spring.action.LoginAction">
			<result name="success">/login_success.jsp</result>
		</action>
	</package>
</struts>

```

### login_success.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="/struts-tags" prefix="s" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>登录成功</title>
</head>
<body>
	<h2>您好！<s:property value="xh"/> 欢迎您登录成功</h2>
</body>
</html>
```

### 部署运行
在登录框和密码框输入任意输入，单击登录按钮，转向登录成功界面，并输出登录名。

### 添加Spring框架
拷贝Spring的核心jar包到lib文件夹。

### 添加Spring支持包
拷贝struts2-spring-plugin.jar到lib文件夹，该包位于Struts2的lib目录下。

### 修改web.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app id="WebApp_9" version="2.4" xmlns="http://java.sun.com/xml/ns/j2ee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
	<welcome-file-list>
		<welcome-file>/login.jsp</welcome-file>
	</welcome-file-list>
    <filter>
        <filter-name>struts2</filter-name>
        <filter-class>org.apache.struts2.dispatcher.FilterDispatcher</filter-class>
    </filter>

    <filter-mapping>
        <filter-name>struts2</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

	<listener>
		<listener-class>
			org.springframework.web.context.ContextLoaderListener
		</listener-class>
	</listener>
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>
			/WEB-INF/classes/applicationContext.xml
		</param-value>
	</context-param>
</web-app>

```

### struts.properties
```
struts.objectFactory=spring
```
该文件位于src文件夹下，运行过程中会被Struts2框架加载，是的Struts2的类对象的生成交给Spring完成。

### applicationContext.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<!--
  - Middle tier application context definition for the image database.
  -->
<beans xmlns="http://www.springframework.org/schema/beans"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xmlns:context="http://www.springframework.org/schema/context"
		xmlns:tx="http://www.springframework.org/schema/tx"
		xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
				http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-2.5.xsd
				http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-2.5.xsd">
	<bean id="loginAction" class="com.voidking.struts2_spring.action.LoginAction"></bean>

</beans>

```

### 修改struts.xml
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
    "http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
	<include file="struts-default.xml"></include>
	<package name="default" extends="struts-default">
		<!-- 使用Spring生成的类对象 -->
		<action name="login" class="loginAction">
			<result name="success">/login_success.jsp</result>
		</action>
	</package>
</struts>

```

### 再次部署运行
我们可以得到和第一次部署运行同样的结果。

### 源代码分享

https://github.com/voidking/struts2_spring.git

### 参考文档
《Java EE基础实用教程》，郑阿奇主编

