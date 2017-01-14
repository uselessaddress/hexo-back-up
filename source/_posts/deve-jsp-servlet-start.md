title: JSP和servlet概述
date: 2014-02-19 19:10:24
toc: true
tags:
- JSP
- servlet
- JavaEE
categories: 设计开发
---

## JSP概述

### 名词解释
JSP全名为Java Server Pages，中文名叫java服务器页面，其根本是一个简化的Servlet设计。

JSP文件类似于HTML文件，但又不完全相同，其实JSP是由HTML、Java片段和JSP标记组成的。

### Servlet
Java的运行方式是通过Java虚拟机把\*.java的文件编译成\*.class文件，但JSP文件却是后缀名为.jsp的文件，怎么执行呢？
当\*.jsp文件被送到服务器后，先由服务器翻译成Servlet文件，而Servlet文件就是\*.java文件，然后\*.java文件又被编译成\*.class文件，再由Java虚拟机解释执行。
<!--more-->
Servlet和Java Applet一样，它们都不是独立的应用程序，都没有main()方法，而是生存在容器中，由容器来管理。编写一个Servlet文件，需要实现javax.servlet.Servlet接口。

### Servlet demo设计

新建Dynamic Web Project，命名为servlet。新建包com.voidking.servlet，新建类HelloWorld，实现Servlet接口，内容如下：
```
package com.voidking.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.Servlet;
import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public class HelloWorld implements Servlet {

	@Override
	public void destroy() {
		// TODO Auto-generated method stub

	}

	@Override
	public ServletConfig getServletConfig() {
		// TODO Auto-generated method stub
		return null;
	}

	@Override
	public String getServletInfo() {
		// TODO Auto-generated method stub
		return null;
	}

	@Override
	public void init(ServletConfig arg0) throws ServletException {
		// TODO Auto-generated method stub

	}

	@Override
	public void service(ServletRequest req, ServletResponse res)
			throws ServletException, IOException {
		// TODO Auto-generated method stub

		PrintWriter pw = res.getWriter();
		pw.println("HelloWorld");
	}

}

```

在WebContent/WEB-INF下新建web.xml，内容如下：
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5" 
	xmlns="http://java.sun.com/xml/ns/javaee" 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
	http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
  <display-name></display-name>	
  <servlet>
  	<servlet-name>HelloWorld</servlet-name>
  	<servlet-class>com.voidking.servlet.HelloWorld</servlet-class>
  </servlet>
  
  <servlet-mapping>
  	<servlet-name>HelloWorld</servlet-name>
  	<url-pattern>/helloWorld</url-pattern>
  </servlet-mapping>
</web-app>

```

发布工程，访问http://localhost:8080/servlet/helloWorld ，即可在界面上看到“HelloWorld”。

### 原理分析
init()：在Servlet实例化之后，Servlet容器会调用init()方法，来初始化该对象。该方法主要是为了让Servlet对象在处理客户请求前可以完成一些初始化工作，例如，建立数据库的连接，获取配置信息等。对于每个Servlet实例，init()方法只能被调用一次。init()方法有一个类型为ServletConfig的参数，Servlet容器通过这个参数向Servlet传递配置信息。Servlet使用该对象从Web应用程序的配置信息中获取初始化参数。另外，在Servlet中，还可以通过ServletConfig对象获取描述Servlet运行环境的ServletContext对象，使用该对象，Servlet可以和它的Servlet容器进行通信。

service()：容器调用service()方法来处理客户端的请求。在init()方法正确完成后，service()方法被容器调用。在service()方法中有用于接受客户端请求信息的请求对象（类型为ServletRequest）和用于对客户端进行响应的响应对象（类型为ServletResponse）的参数。Servlet对象通过ServletRequest对象得到客户端的相关信息和请求信息，在对请求进行处理后，调用ServletResponse对象的方法设置响应信息。

destroy()：当容器检测到一个Servlet对象应该从服务中被移除时，容器会调用该对象的destroy()方法，来释放Servlet对象所使用的资源，保存数据到持久存储设备中。例如，将内存中的数据保存到数据库中，关闭数据库连接等。在Servlet容器调用destroy()方法前，如果还有其他线程正在service()方法中执行，容器会等待这些线程执行完毕或等待服务器设定的最大时间到达。一旦Servlet对象的destroy()方法被调用，容器不会再把其他的请求发送给该对象。如果需要该Servlet对象再次为客户端服务，容器将会重新生成一个Servlet对象来处理客户端的请求。在destroy()方法调用之后，容器会释放这个Servlet对象，在随后的时间内，该对象会被Java的垃圾收集器回收。

getServletConfig()：放回容器调用init()方法时传递给Servlet对象的ServletConfig对象，ServletConfig对象包含了Servlet的初始化参数。

getServletInfo()：放回一个String类型的字符串，其中包括关于Servlet的信息，例如，作者、版本和版权。


web.xml文件中包含配置信息。web.xml文件的第一行是对xml文件的生命，接着就是xml的根元素<web-app>，其属性中声明了版本等信息。
<servlet>和</servlet>之间配置的是<servlet-name>和<servlet-class>，其中<servlet-name>的值“HelloWorld”是为Servlet起的一个名字，这个可以随便起名，只要符合Java的命名规则就行；而<servlet-class>的值则是自己写的Servlet类的类名，这个必须配置正确，如果有包，还要在前面加上包名。这个类名不带.java，也不带.class。

<servlet-mapping>与</servlet-mapping>之间配置的是<servlet-name>和<url-pattern>，其中<servlet-name>的值就是上面刚刚配置的<servlet-name>的值；而<url-pattern>的值也可以随便起名，但其前面必须加“/”，如上面例子中的“/helloWorld”。

访问地址：http://localhost:8080/servlet/helloWorld  ，其中，`http://localhost:8080` 是服务器URL，而后面的`servlet`是项目名，最后面的`helloWorld`是在web.xml中配置的<url-pattern>的值。

### 优化
#### GenericServlet类
上面的例子中，采用实现Servlet接口的方法，需要实现5个方法。为了简化Servlet的编写，在javax.servlet包中提供了一个抽象的类GenericServlet。它给出了出service()外其他4个方法的简单实现。GenericServlet实现了Servlet接口和ServletConfig接口。
HelloWorld.java可以改写为：
```
package com.voidking.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.GenericServlet;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public class HelloWorld extends GenericServlet {

	@Override
	public void service(ServletRequest arg0, ServletResponse arg1)
			throws ServletException, IOException {
		// TODO Auto-generated method stub
		PrintWriter pw = arg1.getWriter();
		pw.println("HelloWorld");

	}

}

```

#### HttpServlet类
在大部分网络中，都是客户端通过HTTP协议来访问服务端的资源。为了快速开发应用于HTTP协议的Servlet类，在javax.servlet.http包中提供了一个抽象HttpServlet。它继承了GenericServlet类。

当容器接到一个针对HttpServlet对象请求时，该对象就会调用public的service()方法，首先将参数类型转换为HttpServletRequest和HttpServletResponse，然后调用protected的service()方法将参数传进去。

接着调用HttpServletRequest对象的getMethod()方法获取请求方法名来调用相应的doXxx()方法。所以一个Servlet类在继承HttpServlet时，不用覆盖它的service()方法，只需要覆盖相应的doXxx()方法就行了。通常情况下，都是覆盖其doGet()和doPost()方法。然后在其中的一个方法中调用另一个方法，这样就可以做到合二为一。

HelloWorld.java可以改写为：
```
package com.voidking.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class HelloWorld extends HttpServlet {
	protected void doGet(HttpServletRequest req,HttpServletResponse res) throws ServletException,IOException
	{
		PrintWriter pw = res.getWriter();
		pw.println("HelloWorld");
	}
	
	protected void doPost(HttpServletRequest req,HttpServletResponse res) throws ServletException,IOException
	{
		doGet(req, res);
	}

}

```

### Servlet进阶
在一个HTML文件中建立一个表单，里面有一个输入框，当客户输入内容后，提交到一个Servlet类，这个Servlet类取出客户输入的信息，并在HTML页面上显示该内容。

右击servlet项目下的WebContent，New，HTML File，命名为index.html，内容如下：
```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Servlet进阶</title>
</head>
<body>
	<form action="inputServlet" method="post">
		请输入你想显示的内容：<input type="text" name="input"/></br>
		<input type="submit" value="提交" />
		<input type="reset" value="重置" />
	</form>
</body>
</html>
```

在com.voidking.servlet包下新建InputServlet类，继承HttpServlet，内容如下：
```
package com.voidking.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class InputServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public InputServlet() {
        super();
    }

	protected void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
		// TODO Auto-generated method stub
		res.setCharacterEncoding("utf8");
		req.setCharacterEncoding("utf8");
		
		String input = req.getParameter("input");
		
		PrintWriter pw = res.getWriter();
		pw.println("<html><head><meta charset=\"UTF-8\"><title>");
		pw.println("显示输入内容");
		pw.println("</title><body>");
		pw.println(input);
		pw.println("</body></html>");
	}

	protected void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(req, res);
	}

}

```

web.xml修改如下：
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5" xmlns="http://java.sun.com/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
	http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
	<display-name></display-name>
	<servlet>
		<servlet-name>HelloWorld</servlet-name>
		<servlet-class>com.voidking.servlet.HelloWorld</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>HelloWorld</servlet-name>
		<url-pattern>/helloWorld</url-pattern>
	</servlet-mapping>

	<servlet>
		<servlet-name>InputServlet</servlet-name>
		<servlet-class>com.voidking.servlet.InputServlet</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>InputServlet</servlet-name>
		<url-pattern>/inputServlet</url-pattern>
	</servlet-mapping>
</web-app>

```

发布工程，访问：http://localhost:8080/servlet/

### 源码分享
https://github.com/voidking/servlet.git

### JSP语法
#### 数据定义
`<%! 变量声明 %>`，相当于Servlet类里面的变量声明。

#### 程序块
`<% 程序块 %>`，相当于在Servlet类里面service()函数里。

#### 表达式
`<%=Java表达式 %>`，相当于Servlet类里面service()函数里，PrintWriter调用println()函数。

#### 注释
`<%-- 注释内容[<%=表达式%>]-->`，输出注释。
`<%-- 注释内容--%>`，隐藏注释。

#### 指令
JSP指令用来提供整个JSP页面的相关信息和设定JSP页面的相关属性，如设定网页的编码方式、脚本语言及导入需要用到的包等。语法格式如下：
`<%@ 指令名 属性名="属性值"%>`

1、`page`指令
2、`include`指令
3、`taglib`指令

#### 动作
1、`<jsp:param>`
2、`<jsp:include>`
3、`<jsp:useBean>`
4、`<jsp:setProperty>`
5、`<jsp:getProperty>`
6、`<jsp:forward>`
7、`<jsp:plugin>`

#### 内置对象
1、`page`
2、`config`
3、`out`
4、`response`
5、`request`
6、`session`
7、`application`
8、`pageContext`
9、`exception`


### 参考文档
《Java EE基础实用教程》，郑阿奇主编


