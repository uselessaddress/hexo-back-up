title: Strust2国际化应用
date: 2014-02-20 20:10:24
tags: 
- Struts2
- JavaEE
categories: 设计开发
toc: true
---


### 前言
有的时候，一个项目不仅要求只支持一种语言。如用中文开发的项目，只有懂中文的用户能用，而别的国家由于不使用中文将难以使用。若再重新开发一套功能相同但只是语言不同的项目，显然是不可取的。所以对于一个项目，国际化的应用是必要的。

### 建立资源文件
在Struts2概述中的实例的基础上，在src文件夹下新建一个文件struts.properties，内容如下：
```
struts.custom.resources=messageResource
```
或者直接在struts.xml中添加：
```
<constant name="struts.custom.i18n.resources" value="messageResource"></constant>
```
<!--more-->
在src文件夹下新建两个文件，messageResource_en_US.properties和messageResource_zh_CN.properties，内容分别如下：
```
username=DLM
password=KL
login=login
```


```
username=登录名
password=口令
login=登录
```
PS：在输入中文时，eclipse会帮助我们自动转成ASCII字符，所以，输入完成后，我们看到的结果如下：
```
username=\u767B\u5F55\u540D
password=\u53E3\u4EE4
login=\u767B\u5F55
```

### 建立login.jsp文件
为了让程序可以显示国际化信息，则需要在JSP页面输出key，而不是直接输出字符常量。
Struts2访问国际化消息主要有以下三种方式：
1、在JSP页面输出国际化消息，可以使用Struts2的<s:text.../>标签，该标签可以指定name属性，该属性指定国际化资源文件中的key。
2、在Action中访问国际化消息，可以使用ActionSupport类的getText()方法，该方法可以接收一个参数，该参数指定了国际化资源文件中key。
3、在表单元素的label属性里输出国际化信息，可以为该表单标签指定一个key属性，该属性指定了国际化资源文件中的key。

下面是login.jsp文件代码：
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="/struts-tags" prefix="s" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>登录</title>
</head>
<body>
	<s:i18n name="messageResource">
		<s:form action="login.action" method="post">
			<s:textfield name="XH" key="username" size="20"></s:textfield>
			<s:textfield name="KL" key="password" size="21"></s:textfield>
			<s:submit value="%{getText('login')}"></s:submit>
		</s:form>
	</s:i18n>
</body>
</html>

```

### 部署运行
部署项目，启动Tomcat，使用IE访问：http://localhost:8080/struts2/login.jsp ，可以看到中文界面。
工具，Internet选项，常规，语言，添加，英语（美国）[en-US]，上移。
刷新页面，就可以看到英文界面了。


### eclipse下显示classes文件夹

eclipse，Window，Show View，Navigator。


### 参考文档
《Java EE基础实用教程》，郑阿奇主编