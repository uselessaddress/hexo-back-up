title: Strust2概述
date: 2014-02-20 19:10:24
tags:
- Struts2
- JavaEE
categories: 设计开发
toc: true
---

### 名词解释
Struts：它通过采用 Java Servlet/JSP 技术，实现了基于JavaEE Web应用的MVC设计模式的应用框架，是MVC经典设计模式中的一个经典产品。

Struts2：它是Struts的下一代产品，是在Struts和WebWork的技术基础上进行了合并的全新的Struts2框架。其全新的Struts2的体系结构与Struts的体系结构差别巨大。Struts2以WebWork为核心，采用拦截器的机制来处理用户的请求，这样的设计也使得业务逻辑控制器能够与ServletAPI完全脱离开，所以Struts2可以理解为WebWork的更新产品。

MVC模式的提出改变了程序设计的思路，但代码的规范性还是很差，而Struts框架则具有组件的模块化、灵活性和重用性的优点，同时也简化了基于MVC的Web应用程序的开发，从应用的角度来说，Struts有三大块：Struts核心类、Struts配置文件及Struts标签库。

Struts本身就实现了MVC模式，就Struts的发展来说，从以前的Struts1到现在的Struts2，其目的是为了给程序员一个好的框架来开发应用软件。
<!--more-->
### 官网
http://struts.apache.org/index.html

### 下载
http://struts.apache.org/download

下载的下载 struts-\*-all.zip 解压后，有四个文件夹：
1、apps。包含基于Struts2的示例应用，是学习Struts2非常有用的资料。
2、docs。包含Struts2的相关文档，如Struts2快速入门、Struts2文档、API文档等资料。
3、lib。包含Struts2框架的核心类库，以及Struts2的第三方插件类库。
其中有5个是必须的：struts2-core-\*.jar、xwork-\*.jar、 ognl-\*.jar、commons-logging-\*.jar、freemarker-\*.jar。
还有3个包也要注意导入，不导入运行Tomcat时候可能会出现异常。分别是：commons-io-\*.jar、commons-fileupload-\*.jar和javassist-\*.ga.jar。
整合Spring与Struts时，在Struts的lib目录中找到struts2-spring-plugin-\*.jar，引入到工程中。 
4、src。包含Struts2框架的全部源代码。



### MVC简介
MVC包含三个基础部分：Model、View和Controller，这三个部分以最小的耦合协同工作，以增加程序的可扩展性和可维护性。在JSP demo设计中，首先JSP页面作为View，Servlet作为Controller，而JavaBean作为Model。具体来说，MVC具有以下优点：
1、多个视图可以对应一个模型。按MVC设计模式，一个模型对应多个视图，可以减少代码的复制及代码的维护量，一旦模型发生改变，也易于维护。

2、模型返回的数据与现实逻辑分离。模型数据可以应用任何显示技术，例如，使用JSP页面、Velocity模板或者直接产生Excel文档等。

3、应用被分为三层，降低了各层之间的耦合，提供了应用的可扩展性。

4、控制层的概念也很有效，由于它把不同的模型和不同的视图组合在一起，完成不同的请求，因此控制层可以说是包含了用户请求权限的概念。

5、MVC更符合软件工程化管理的精神。不同的层各司其职，每一层的组件具有相同的特征，有利于通过工程化和工具化产生管理程序代码。

### Struts2体系结构
Struts2的基本流程如下：
1、Web浏览器请求一个资源。
2、过滤器Dispatcher查找请求，确定适当的Action。
3、拦截器自动对请求应用通用功能，如验证和文件上传等操作。
4、Action的execute方法通常用来存储和重新获得信息（通过数据库）。
5、结果被返回到浏览器。可能是HTML、图片、PDF或其他。

其实，Struts2框架的应用着重在控制上。简单的流程是：页面->控制器->页面。最重要的控制器的取数据与处理后传数据的问题。

### Struts2实例开发

#### 建立一个Web项目
新建Dynamic Web Project，命名为“struts2”。

#### 加载Struts2基本类库
右击项目名，Properties，Java Build Path，Libraries，Add Library...，User Library，Next，User Libraries...，New...，输入“struts2”，OK，Add External JARs...，选中5个必须的jar文件，打开，OK。

右击项目名，Properties，Java Build Path，Libraries，Add Library...，User Library，Next，选中“struts2”，Finish。

#### 配置web.xml文件
在WebContent/WEB-INF中新建文件web.xml，内容如下：
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app id="WebApp_9" version="2.4" xmlns="http://java.sun.com/xml/ns/j2ee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
	<welcome-file-list>
		<welcome-file>/hello.jsp</welcome-file>
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
#### 创建hello.jsp
在WebContent中新建hello.jsp，内容如下：
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Struts2应用</title>
</head>
<body>
	<form action="struts.action" method="post">
		请输入姓名：<input type="text" name="name" /><br/>
		<input type="submit" value="提交"/>
	</form>
</body>
</html>
```
#### Action实现类
新建包com.voidking.struts2.action，新建类StrutsAction，代码如下：
```
package com.voidking.struts2.action;

import java.util.Map;

import com.opensymphony.xwork2.ActionContext;
import com.opensymphony.xwork2.ActionSupport;

public class StrutsAction extends ActionSupport{
	private String name;
	
	public String getName()
	{
		return name;		
	}
	
	public void setName(String name)
	{
		this.name=name;
	}
	
	public String execute() throws Exception
	{
		if(!name.equals("HelloWorld"))
		{
			Map request = (Map)ActionContext.getContext().get("request");
			request.put("name", getName());
			return "success";
		}else {
			return "error";
		}
	}
}

```
在写代码的时候，小编发现，当输入类ActionSupport时，不会出现代码提示。于是，小编直接把刚才的Struts的5个主要类库拷贝到了WebContent/lib下，果然出现了代码提示。估计是因为eclipse在编辑时不会扫描通过Java Build Path添加的类库，而会扫描lib中的类库。

#### 创建并配置struts.xml文件
在src中新建文件struts.xml，内容如下：
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
    "http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
    <package name="default" namespace="/" extends="struts-default">
        <default-action-ref name="index" />
        <action name="struts" class="com.voidking.struts2.action.StrutsAction">
            <result name="success">/welcome.jsp</result>
            <result name="error">/hello.jsp</result>
        </action>
    </package>
</struts>

```
#### 创建welcome.jsp
在WebContent中新建文件welcome.jsp，代码如下：
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="/struts-tags" prefix="s" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>欢迎</title>
</head>
<body>
	hello<s:property value="#request.name"/>!
</body>
</html>
```
#### 部署和运行
发布，启动Tomcat，访问http://localhost:8080/struts2/ 或者 http://localhost:8080/struts2/hello.jsp 

PS：在调试过程中，如果修改了项目中的.java文件或者配置文件，就必须重新启动Tomcat服务器，而修改了JSP文件则只需要刷新页面即可。

### Struts2实例分析
#### 工作流程
用户发送一个请求后，web.xml中配置的FilterDispatcher就会过滤该请求。如果请求是以.action结尾，该请求就会被转入Struts2框架处理。Struts2框架接收到*.action请求后，将根据*.action请求前面的“*”来决定调用哪个业务。

Struts2框架中的配置文件struts.xml会起映射作用，它会根据“*”来决定调用用户定义的哪个Action类。例如在上面项目中，请求为struts.action，所以，在struts.xml中有个Action类的name为“struts”，这表示该请求与这个Action来匹配，就会调用该Action中class属性指定的Action类。

但是，在Struts2中，用户定义的Action类并不是业务拦截器，而是Action代理，其并没有和Servlet耦合。所以Struts2框架提供了一系列拦截器，它负责将HttpServletRequest请求中的请求参数解析出来，传入到用户定义的Action类中。然后再调用其execute()方法处理用户请求，处理结束后，会返回一个值，这时，Struts2框架的struts.xml文件又起到映射作用，会根据其返回的值来决定跳转到哪一个页面。如在上面项目中，如果返回的是SUCCESS，就会跳到welcome.jsp页面；如果是ERROR，就会回到原页面。

#### 各文件详解
##### web.xml
web.xml中定义了一个过滤器，那么，过滤器是什么？

过滤器是用户请求和处理程序之间的一层处理程序。它可以对用户请求和处理程序响应的内容进行处理，通常用于权限控制、编码转换等场合。
它先于与之相关的servlet或JSP页面运行在服务器上。过滤器可附加到一个或多个servlet或JSP页面上，并且可以检查进入这些资源的请求信息。在这之后，过滤器可以作如下的选择：
1、以常规的方式调用资源（即，调用servlet或JSP页面）。
2、利用修改过的请求信息调用资源。
3、调用资源，但在发送响应到客户机前对其进行修改。
4、阻止该资源调用，代之以转到其他的资源，返回一个特定的状态代码或生成替换输出。

在Servlet作为过滤器使用时，它可以对客户的请求进行处理。处理完成后，它会交给下一个过滤器处理，这样，客户的请求在过滤链里逐个处理，直到请求发送到目标为止。
例如，某网站里有提交“修改的注册信息”的网页，当用户填写完修改信息并提交后，服务器在进行处理时需要做两项工作：判断客户端的会话是否有效；对提交的数据进行统一编码。这两项工作可以在由两个过滤器组成的过滤链里进行处理。当过滤器处理成功后，把提交的数据发送到最终目标；如果过滤器处理不成功，将把视图派发到指定的错误页面。

Servlet过滤器是在Java Servlet规范中定义的，它能够对过滤器关联的URL请求和响应进行相关检查和修改。Servlet过滤器能够在Servlet被调用之后检查response对象，修改response的Header对象和response内容。Servlet过滤器过滤的URL资源可以是Servlet、JSP、HTML文件，或者是整个路径下的任何资源。多个过滤器可以构成一个过滤器链，当请求过滤器关联的URL时，过滤器就会逐个发生作用。

所有过滤器必须实现java.Servlet.Filter接口，这个接口中含有3个过滤器类必须实现的方法：
1、init(FilterConfig)：Servlet过滤器的初始化方法，Servlet容器创建Servlet过滤器实例后将调用这个方法。
2、doFilter(ServletRequest,ServletResponse,FilterChain)：完成实际的过滤操作，当用户请求与过滤器关联的URL时，Servlet容器将先调用过滤器doFilter方法，返回响应之前也会调用此方法。FilterChain参数用于访问过滤器链上的下一个过滤器。
3、destroy()：Servlet容器在销毁过滤器实例前调用该方法，这个方法可以释放Servlet过滤器占用的资源。

过滤器类编写完成后，必须要在web.xml中进行配置，格式如下：
```
<filter>
	<!--自定义的名称-->
	<filter-name>过滤器名</filter-name>
	<!--自定义的过滤器类，注意，如果写在包下，要加包名-->
	<filter-class>过滤器对应类</filter-class>
	<init-param>
		<!--类中参数名称-->
		<param-name>参数名称</param-name>
		<!--对应参数的值-->
		<param-value>参数值</param-value>
	</init-param>
</filter>
```
过滤器必须和特定的URL关联才能发挥作用，过滤器的关联方式有3种：与一个URL关联、与一个URL目录下的所有资源关联、与一个Servelet关联。

```
<filter-mapping>
	<!--这里与上面配置的名称要相同-->
	<filter-name>过滤器名</filter-name>
	<!--与一个URL资源关联-->
	<url-pattern>xxx.jsp</url-pattern>
</filter-mapping>
```
```
<filter-mapping>
	<!--这里与上面配置的名称要相同-->
	<filter-name>过滤器名</filter-name>
	<!--与一个URL目录下所有资源关联-->
	<url-pattern>/*</url-pattern>
</filter-mapping>
```
```
<filter-mapping>
	<!--这里与上面配置的名称要相同-->
	<filter-name>过滤器名</filter-name>
	<!--与一个Servlet关联-->
	<url-pattern>Servlet名称</url-pattern>
</filter-mapping>
```

##### struts.xml
struts.xml是Struts2框架的核心配置文件，主要用于配置开发人员编写的action。struts.xml文件通常放在Web应用程序的WEB-INF/classes目录下（src目录下也可以，src目录下编译生成的文件默认放在WEB-INF/classes目录下），该目录下的struts.xml将被Struts2框架自动加载。

struts.xml是一个XML文件，文件前面是XML的头文件，然后是<struts>标签，位于Struts2配置的最外层，其他标签都是包含在它里面的。

##### package元素
Struts2的包类似于Java中的包，将action、result、result类型、拦截器和拦截器栈组织为一个逻辑单元，从而简化了维护工作，提高了重用性。

与Java中的包不同的是，Struts2中的包可以扩展另外的包，从而“继承”原有包的所有定义，并可以添加自己包的特有配置，以及修改原有包的部分配置。从这一点上看，Struts2中的包更像Java中的类。

package有以下几个常用属性：
1、name：该属性是必选的，指定包的名字，这个名字将作为引用该包的键。注意，包的名字必须是唯一的，在一个struts.xml文件中不能出现两个同名的包。
2、extends：该属性是可选的，允许一个包继承一个或多个先前定义的包。
3、abstract：该属性是可选的，将其设置为true，可以把一个包定义为抽象的。抽象包不能有action定义，它只能作为“父”包，被其他包所继承。注意，以为Struts2的配置文件是从上到下处理的，所以父包应该在子包前面定义。
4、namespace：该属性是可选的，将保存的action配置为不同的名称空间。
如果接收到一个请求为/space/main.action，框架将首先查找/space名称空间，如果找到了，则执行main.action；如果没有找到，则到默认的名称空间（name="default"）中继续查找。

如果接收到一个请求为/main.action，框架将首先查找“/”名称空间，如果找到了，则执行main.action；如果没有找到，则到默认的名称空间（name="default"）中继续查找。

##### Action元素
Struts2的核心功能是Action。对于开发人员来说，使用Struts2框架，主要的编码工作就是编写Action类。而开发好Action类后，就需要配置Action映射，以告诉Struts2框架，针对某个URL的请求应该交由哪个Action进行处理。

当一个请求匹配到某个Action名字时，框架就使用这个映射来确定如何处理请求。
```
<action name="struts" class="com.voidking.struts2.action.StrutsAction">
	<result name="success">/welcome.jsp</result>
	<result name="error">/hello.jsp</result>
</action>
```
在上面代码中，如果一个请求映射到struts时，就会执行该Action配置的class属性对应的Action类。然后根据Action类的返回值决定跳转的方向。其实一个Action类中不一定只能有execute()方法。如果一个请求要调用Action类中的其他方法，就需要在Action配置中加以配置。例如，如果在com.voidking.struts2.action.StrutsAction中有另外一个方法为：
```
public String find() throws Exception{return SUCCESS;}
```
如果想要调用这个方法，就必须在Action中配置method属性，其配置方法为：
```
<action name="find" class="com.voidking.struts2.action.StrutsAction" method="find">
	<result name="success">/welcome.jsp</result>
	<result name="error">/hello.jsp</result>
</action>
```


##### result元素
一个result元素代表一个可能的输出。当Action类中的方法执行完成时，返回一个字符串型的结果代码，框架根据这个结果代码选择对应的result，向用户输出。
```
<result name="逻辑视图名" type="视图结果类型" >
	<param name="参数名">参数值</param>
</result>
```
param中的name属性有两个值：
1、location：指定逻辑视图。
2、parse：是否允许在实际视图名中使用OGNL表达式，默认参数为true。
实际上通常不需要写这个param标签，而是直接在<result></result>中指定物理视图位置。

result中的name属性有如下值：
1、success：表示请求处理成功，该值也是默认值。
2、error：表示请求处理失败。
3、none：表示请求处理完成后不跳转到任何界面。
4、input：表示输入时如果验证失败应该跳转到什么地方。
5、login：表示登录失败后跳转的目标。

type（非默认类型）属性支持的结果类型有以下几种：
1、chain：用来处理Action链。
2、chart：用来整合JFreeChart的结果类型。
3、`dispatcher`：用来转向页面，通常处理JSP，该类型也为默认类型。
4、freemarker：处理FreMarker模板。
5、httpheader：控制特殊HTTP行为的结果类型。
6、jsf：JSF整合的结果类型。
7、redirect：重定向到一个URL。
8、`redirect-action`：重定向到一个action。
9、stream：想浏览器发送InputStream对象，通常用来处理文件下载，还可用于返回Ajax数据。
10、tiles：与Tiles整合的结果类型。
11、velocity：处理Velocity模板。
12、xslt：处理XML/XSML模板。
13、plaintext：显示原始文本文件，如文件源代码。

##### ActionSupport类
在Struts2中，Action与容器已经做到完全解耦，不再继承某个类或实现某个接口，也就是说，上面的实例中，StrutsAction完全可以不继承ActionSupport类。
但是，在特殊情况下，为了降低编程的工作难度，充分利用Struts2提供的功能，定义Action时会继承ActionSupport类，该类位于xwork2提供的包com.opensymphony.xwork2中。
下面是ActionSupport实现的接口：
```
public class ActionSupport implements Action,Validateable,ValidationAware,TextProvider,LocaleProvider,Serializable{
}
```
Action接口同样位于com.opensymphony.xwork2包，定义了一些常量和一个execute()方法。
```
public interface Action{
	public static final String SUCCESS = "success";
	public static final String NONE = "none";
	public static final String ERROR = "error";
	public static final String INPUT = "input";
	public static final String LOGIN = "login";
	public String execute() throws Exception;
}
```
接口ValidationAware的实现类ValidationAwareSupport定义了三个集合成员，这些集合用户存储运行时的错误或者消息。ValidationAware的众多方法主要完成对这些成员的存储操作和判断集合中是否有元素的操作，ActionSupport仅仅实现对这些方法的简单调用。

### Struts2数据验证及验证框架
#### 数据验证
上面的实例中，即使用户输入空的name，服务器也会处理用户请求。但是，如果是注册时，用户注册了空的用户名和密码，并且保存到数据库中，如果后面要根据用户输入的用户名和密码来查询数据，这些空的输入就可能会引起异常。（PS：可以使用JavaScript在客户端验证）
Action类继承了ActionSupport类，该类实现的接口中有Validateable，定义了validate()方法。所以只要在用户自定义的Action类中重写该方法就可以实现验证功能，修改ActionSupport如下：
```
package com.voidking.struts2.action;

import java.util.Map;

import com.opensymphony.xwork2.ActionContext;
import com.opensymphony.xwork2.ActionSupport;

public class StrutsAction extends ActionSupport{
	private String name;
	
	public String getName()
	{
		return name;		
	}
	
	public void setName(String name)
	{
		this.name=name;
	}
	
	public String execute() throws Exception
	{
		if(!name.equals("HelloWorld"))
		{
			Map request = (Map)ActionContext.getContext().get("request");
			request.put("name", getName());
			return SUCCESS;
		}else {
			return ERROR;
		}
	}
	
	public void validate()
	{
		//如果姓名为空，则把错误信息添加到Action类的fieldErrors
		if(this.getName()==null || this.getName().trim().equals(""))
		{
			addFieldError("name", "姓名是必须的！");
		}
		
	}
}

```
validate()方法会在execute()方法之前执行，执行该方法之后，Action类的fieldErrors中已经包含了数据校验错误信息，将把请求转发到input逻辑视图出，所以要在配置中加入以下代码：
```
<action name="struts" class="com.voidking.struts2.action.StrutsAction">
	<result name="success">/welcome.jsp</result>
	<result name="error">/hello.jsp</result>
	<result name="input">/hello.jsp</result>
</action>
```

经过测试，我们发现，当输入为空时，界面上并不会有任何提示！废话，根本就没有在界面上设置显示错误的区域好不来！修改hello.jsp如下：
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@taglib prefix="s" uri="/struts-tags" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Struts2应用</title>
</head>
<body>
	<form action="struts.action" method="post">
		请输入姓名：<input type="text" name="name" /><br/>
		<input type="submit" value="提交"/>
		<s:fielderror fieldName="name"/>
	</form>
</body>
</html>
```


#### 验证框架
上面的校验通过重写validate方法实现，这种方法虽然可以达到预期效果，但是如果不是一个输入框，而是2、3个甚至更多，那就要在validate方法中做出很多判断，而且这些判断的语句基本相同。所以Struts2提供了校验框架，只需要增加一个校验配置文件，就可以完成对数据的校验。Struts2提供了大量的数据校验框架，包括表单域校验器和非表单域校验器两种。

##### 必填字符串校验器
在包com.voidking.struts2.action中，新建文件StrutsAction-validation.xml，内容如下：
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE validators PUBLIC
        "-//OpenSymphony Group//XWork Validator Config 1.0//EN"
        "http://www.opensymphony.com/xwork/xwork-validator-config-1.0.dtd">
<validators>
<!--需要校验的字段的字段名-->
	<field name="name">
		<field-validator type="requiredstring">
			<!--去空格-->
			<param name="trim">true</param>
			<!--错误提示信息-->
			<message>姓名是必须的</message>
		</field-validator>
	</field>
</validators>
```
如果想对StrutsAction中find()方法进行验证，命名应该为StrutsAction-find-validation.xml。

经过测试，完全没有效果，而且会报出警告！java.io.FileNotFoundException: http://www.opensymphony.com/xwork/xwork-validator-1.0.dtd

经过查找资料，原来是这个url已经过时了，opensymphony这个组织貌似已经停止运营了，但其主要的开源项目，也都基本找到了新东家，比如struts交由Apache来运营了，改成下面这个写法就没问题：
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE validators PUBLIC
        "-//OpenSymphony Group//XWork Validator Config 1.0//EN"
        "http://struts.apache.org/dtds/xwork-validator-1.0.dtd">
<validators>
<!--需要校验的字段的字段名-->
	<field name="name">
		<field-validator type="requiredstring">
			<!--去空格-->
			<param name="trim">true</param>
			<!--错误提示信息-->
			<message>姓名是必须的</message>
		</field-validator>
	</field>
</validators>
```
关闭网络连接，再次测试，果然又不可以了！晕，还非得联网？难道不可以使用本地的文件吗！
1、解压xwork-core-*.jar包，找到xwork-validator-1.0.dtd。

2、eclipse，Window，Preferences，XML，XML Catelog，Add，File System...，选中刚才解压的xwork-validator-1.0.dtd，打开，

3、Location已经选好`D:\jar\struts2\xwork-validator-1.0.dtd`，Key type选择`Public ID`，Key填`-//OpenSymphony Group//XWork Validator Config 1.0//EN`，Alternative web address填`http://struts.apache.org/dtds/xwork-validator-1.0.dtd`。（本地dtd不存在时回去web上去找dtd）

4、StrutsAction-validation.xml需要修改如下：
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE validators PUBLIC
        "-//OpenSymphony Group//XWork Validator Config 1.0//EN"
        "D:\jar\struts2\xwork-validator-1.0.dtd">
<validators>
<!--需要校验的字段的字段名-->
	<field name="name">
		<field-validator type="requiredstring">
			<!--去空格-->
			<param name="trim">true</param>
			<!--错误提示信息-->
			<message>姓名是必须的</message>
		</field-validator>
	</field>
</validators>
```

##### 必填校验器
```
<validators>
<!--需要校验的字段的字段名-->
	<field name="name">
		<field-validator type="required">
			<!--去空格-->
			<param name="trim">true</param>
			<!--错误提示信息-->
			<message>姓名是必须的</message>
		</field-validator>
	</field>
</validators>
```

##### 整数校验器
```
<validators>
<!--需要校验的字段的字段名-->
	<field name="age">
		<field-validator type="int">
			<param name="min">18</param>
			<param name="max">100</param>
			<!--错误提示信息-->
			<message>年龄必须在18至100之间</message>
		</field-validator>
	</field>
</validators>
```

##### 日期校验器
```
<validators>
<!--需要校验的字段的字段名-->
	<field name="date">
		<field-validator type="date">
			<param name="min">1980-01-01</param>
			<param name="max">2016-01-01</param>
			<!--错误提示信息-->
			<message>日期必须在1980-01-01到2016-01-01之间</message>
		</field-validator>
	</field>
</validators>
```

##### 邮件地址校验器
```
<validators>
<!--需要校验的字段的字段名-->
	<field name="email">
		<field-validator type="email">
			<!--错误提示信息-->
			<message>必须输入有效的电子邮件地址</message>
		</field-validator>
	</field>
</validators>
```

##### 网址校验器
```
<validators>
<!--需要校验的字段的字段名-->
	<field name="url">
		<field-validator type="url">
			<!--错误提示信息-->
			<message>必须输入有效的网址</message>
		</field-validator>
	</field>
</validators>
```

##### 字符串长度校验器
```
<validators>
<!--需要校验的字段的字段名-->
	<field name="password">
		<field-validator type="stringlength">
			<param name="minLength">8</param>
			<param name="maxLength">16</param>
			<!--错误提示信息-->
			<message>密码长度必须在8到16位之间</message>
		</field-validator>
	</field>
</validators>
```

##### 正则表达式校验器
```
<validators>
<!--需要校验的字段的字段名-->
	<field name="xh">
		<field-validator type="regex">
			<param name="expression"><![CDATA[(\d{6})]]></param>
			<!--错误提示信息-->
			<message>学号必须是6位数字</message>
		</field-validator>
	</field>
</validators>
```

### Struts2标签库
Struts2标签库，大大简化了JSP页面输出逻辑的实现。借助Struts2标签库，完全可以避免在JSP页面中使用Java脚本代码。虽然Struts2把所有的标签都定义在URI为/struts-tags的命名空间下，但依然可以对Struts2标签进行简单的分类。
1、UI标签。用于生成HTML元素，分为表单标签和非表单标签。
2、非UI标签。用于数据访问和逻辑控制。
3、Ajax标签。用于Ajax支持。

#### OGNL表达式
OGNL是Object Graphic Navigation Language（对象图导航语言）的缩写，是一个开源项目。
OGNL是一种功能强大的EL（Expression Language，表达式语言），可以通过简单的表达式来访问Java对象中的属性。

标准的OGNL会设定一个根对象（root对象）。假设使用标准OGNL表达式来求值（不是Struts2 OGNL），如果OGNL上下文有两个对象foo对象和bar对象，同时foo对象被设置为根对象，则利用下面的OGNL表达式求值。
```
#foo.blah	//返回foo.getBlah()
#bar.blah	//返回bar.getBlah()
blah		//返回foo.getBlah()，因为foo是根对象
```

在Struts2框架中，值栈（Value Stack）就是OGNL的根对象。假设值栈中存在两个对象实例Man和Animal，这两个对象实例都有一个name属性，Animal有一个species属性，Man有一个salary属性。假设Animal在值栈的顶部，Man在Animal后面，下面的代码片段能够更好地理解OGNL表达式。
```
species		//调用animal.getSpecies()
salary		//调用man.getSalary()
name		//调用animal.getName()
```
如果要获得Man的name值，需要如下代码：
```
man.name
```
Struts2允许在值栈中使用索引，实例代码如下：
```
[0].name	//调用animal.getName()
[1].name	//调用man.getName()
```
Struts2中的OGNL Context是ActionContext，Context Map包括：application、session、值栈（root）、request、parameters、attr。

由于值栈是Struts2中ONGL的根对象。如果用户需要访问值栈中的对象，则可以通过如下代码访问值栈中的属性：
```
${foo}	//获得值栈中foo的属性
```
如果访问其他Context中的对象，由于不是根对象，在访问时需要加#前缀。
1、application对象：用来访问ServletContext，如#application.userName或者#application["userName"]，相当于调用Servlet的getAttribute("userName")。
2、session对象：用来访问HttpSession，如#session.userName或者#session["userName"]，相当于调用session.getAttribute("userName")。
3、request对象：用来访问HttpServletRequest属性的Map，如#request.userName或者#request["userName"]，相当于调用request.getAttribute("userName")。
StrutsAction中有如下代码：
```
Map request = (Map)ActionContext.getContext().get("request");
request.put("name",getName);
```
这就是先得到的request对象，然后把值放进去，该例的welcome.jsp中有：
```
<s:property value="#request.name"/>
```
其中#request.name相当于调用了request.getAttribute("name")。

如果需要一个集合元素时，可以使用OGNL中集合相关的表达式。使用如下代码生成一个List对象：
```
{e1,e2,e3,...}
```
使用如下代码生成一个Map对象：
```
#{key:value1,key2:value2,...}
```
对于集合类型，OGNL表达式可以使用in和not in两个元素符号。其中，in表达式用来判断某个元素是否在指定的集合对象中；not in判断某个元素是否不在指定的集合对象中，代码如下所示：
```
<s:if test="'foo' in {'foo','bar'}">
	...
<s:/if>
```
或
```
<s: if test="'foo' not in {'foo','bar'}">
	...
<s:/if>
```
除了in和not in之外，OGNL还允许使用某个规则获得集合对象的自己，常用的有以下3个相关操作符。
- ?:获得所有符合逻辑的元素。
- ^:获得符合逻辑的第一个元素。
- $:获得符合逻辑的最后一个元素。
```
Person.relatives.{?#this.gender=='male'}
```
该代码可以获得Person的所有性别为male的relatives集合。

#### 数据标签
数据标签属于非UI标签，主要用于提供各种数据访问相关的功能。
- property：用于输出某个值。
- set：用于设置一个新变量。
- param：用于设置参数，通常用于bean标签和action标签的子标签。
- bean：用于创建一个JavaBean实例。如果指定id属性，则可以将创建的JavaBean实例放入Stack Context中。
- action：用于JSP页面直接调用一个Action。
- date：用于格式化输出一个日期。
- debug：用于在页面上生成一个调试链接，当单击该链接时，可以看到当前值栈和Stack Context中的内容。
- il8n：用于指定国际化资源文件的baseName。
- include：用于在JSP页面中包含其他的JSP或Servlet资源。
- push：用于将某个值放入值栈的栈顶。
- text：用于输出国际化。
- url：用于生成一个URL地址。

#### 控制标签
控制标签也属于非UI标签，主要用于完成流程的控制，以及对值栈的控制。
- if、elseif、else：用于控制选择输出的标签。
- append：用于将多个集合拼接成一个新的集合。
- generator：用于将一个字符串按指定的分隔符分隔成多个字符串，临时生成的多个字符串可以使用iterator标签来迭代输出。
- iterator：用于将ihelloworld迭代输出。
- merge：用于将多个集合拼接成一个新的集合，但与append的拼接方式不同。
- sort：用于对集合进行排序。
- subset：用于截取集合的部分元素，形成新的子集合。


#### 表单标签
大部分表单标签和HTML表单元素是一一对应的关系，表单元素中的name属性值会映射到程序员定义的Action类中。

在上面的实例中，属性name的值“name”在StrutsAction的成员变量中就有setName()和getName()的定义，这样Struts2框架就可以把它们关联起来。
实际上，表单元素的名字封装着一个请求参数，而请求参数被封装到Action类中，根据其set方法赋值，再根据其get方法取值。

如果有这样一个JavaBean类，类名为User，该类有两个属性：一个是username，一个是password。并分别生成它们的getter和setter方法，在JSP页面的表单中可以这样为表单元素命名：
```
<s:textfield name="user.username" label="用户名"/>
<s:textfield name="user.password" label="密码">
```
这时可以在Action类中直接定义user对象user属性，并生成其getter和setter方法，这样就可以用user.getUsername()和user.getPassword()方法访问表单提交的username和password的值。

下面介绍和HTML表单元素不是一一对应的几个重要的表单标签：
- checkboxlist：可以一次创建多个复选框，相当于HTML标签的多个<input type="checkbox" .../>，它根据list属性指定的集合来申请多个复选框。因此，该标签需要指定一个list属性。

- combobox：生成一个单行文本框和下拉列表框的组合。两个表单元素只能对应一个请求参数，只有单行文本框里的值才会才包含请求参数，下拉列表框只是用于辅助输入，并没有name属性，故不会产生请求参数。

- datetimepicker：用于生成一个日期、时间下拉列表框。当使用该日期、时间列表框选择某个日期、时间时，系统会自动将选中日期、时间输出指定文本框中。在使用该标签时，要在HTML的head部分加入<s:head/>，因为datetimepicker标签中有一个日历小控件，其中包含JavaScript代码。

- select：用于生成一个下拉列表框，通过为该元素指定list属性的值，来生成下拉列表框的选项。

- radio：用法与checkboxlist用法很相似，唯一的区别就是checkboxlist生成的是复选框，而radio生成的是单选框。

- head：用于生成HTML页面的head部分。
如果需要在页面中使用Ajax组件，就需要在head标签中加入theme="ajax"属性。这样就可以将标准Ajax的头信息包含到页面中。

#### 非表单标签
非表单标签主要用于在界面中生成一些非表单的可视化元素。这些标签不经常用到：
- a：生成超链接。
- actionerror：输出Action实例的getActionMessage()方法返回的消息。
- component：生成一个自定义组件。
- div：生成一个div片段。
- fielderror：输出表单域的类型转化错误、校验错误提示。
- tablePanel：生成HTML页面的Tab页。
- tree：生成一个树形结构。
- treenode：生成树形结构的节点。

### Struts2拦截器
Struts2框架的绝大部分功能是通过拦截器来完成的。当FilterDispatcher拦截到用户请求后，大量拦截器将会对用户请求进行处理，然后才调用用户自定义的Action类中的方法来处理请求。可见，拦截器是Struts2的核心所在。当需要扩展Struts2功能时，只需要提供相应的拦截器，并将它配置在Struts2容器即可。反之，如果不需要某个功能，也只需要取消该拦截器即可。

Struts2内建的大量拦截器都是以name-class对的形式配置在struts-default.xml文件中，其中name是拦截器的名称，class指定该拦截器的实现类。从前面的实例中可以看出，配置struts.xm时，继承了strut-default包，这样就可以应用里面定义的拦截器。否则，就必须自己定义这些拦截器。

#### 拦截器配置
拦截器在struts.xml中配置，格式为：
```
<interceptor name="拦截器名" class="拦截器实现类"></interceptor>
```
有些时候，在拦截器实现类中会定义一些参数，那么在配置拦截器时就需要为其传入拦截器参数。
```
<interceptor name="myInterceptor" class="com.voidking.struts2.tool.MyInterceptor">
	<param name="参数名">参数值</param>
</interceptor>
```
通常情况下，往往多个拦截器一起使用来进行过滤，这时就会把需要的拦截器组成一个拦截器栈。

```
<interceptor-stack name="拦截器栈名">
	<interceptor-ref name="拦截器一"></interceptor-ref>
	<interceptor-ref name="拦截器二"></interceptor-ref>
	<interceptor-ref name="拦截器三"></interceptor-ref>
</interceptor-stack>
```
在配置拦截器栈时，用到的拦截器必须是已经存在的拦截器，即已经配置好的拦截器。拦截器栈也可以引用拦截器栈，实质上就是把引用的拦截器栈中的拦截器包含到了该拦截器栈中。

当在struts.xml中配置一个包时，可以为其指定默认的拦截器，如果为包指定了某个拦截器，则该拦截器会对每个Action起作用，但是如果显式地为某个Action配置了拦截器，则默认的拦截器将不会起作用。默认拦截器用<default-interceptor-ref name=""/>元素来定义。

```
<package name="包名">
	<interceptors>
		<interceptor name="拦截器一" class="拦截器实现类"></interceptor>
		<interceptor name="拦截器二" class="拦截器实现类"></interceptor>
		<interceptor-stack name="拦截器栈名">
			<interceptor-ref name="拦截器一"></interceptor-ref>
			<interceptor-ref name="拦截器二"></interceptor-ref>
		</interceptor-stack>
	</interceptors>
	<default-interceptor-ref name="拦截器名或拦截器栈名"></default-interceptor-ref>
</package>
```

#### 拦截器实现类
Struts2框架提供了很多拦截器，但总有一些功能需要程序员自定义拦截器来完成，比如权限控制等。
Struts2提供了一些接口或类供程序员自定义拦截器。如Struts2提供了com.opensymphony.xwork2.interceptor.Interceptor接口，程序员只要实现该接口就可以完成拦截器实现类。该接口代码如下：
```
import java.io.Serializable;
import com.opensymphony.xwork2.ActionInvocation;
public interface Interceptor extends Serializable{
	void init();
	String intercept(ActionInvocation invocation) throws Exception;
	void destroy();
}
```
- init()：该方法在拦截器被实例化之后、拦截器被执行之前调用。该方法只被执行一次，主要用于初始化资源。
- intercept(ActionInvocation invocation)：该方法用于实现拦截的动作。该方法有个参数，该参数调用其invoke方法，将控制权交给下一拦截器，或者交给Action类的方法。
- destroy()：该方法与init()方法对应，拦截器实例被销毁之前调用，用于销毁在init方法中打开的资源。

除了Intercept接口外，Struts2还提供了AbstractInterceptor类，该类提供了init方法和destroy方法的空实现。在一般的拦截器中，都会继承该类，因为一般实现的拦截器是不需要打开资源的，故无需实现这两个方法，继承该类会更简洁。

#### 实例应用
在上面的实例中，添加功能：在输入框输入“hello”，不能通过，返回当前页面。
新建包com.voidking.struts2.tool，新建类MyInterceptor，代码如下：
```
package com.voidking.struts2.tool;

import com.opensymphony.xwork2.Action;
import com.opensymphony.xwork2.ActionInvocation;
import com.opensymphony.xwork2.interceptor.AbstractInterceptor;
import com.voidking.struts2.action.StrutsAction;

public class MyInterceptor extends AbstractInterceptor{

	@Override
	public String intercept(ActionInvocation arg0) throws Exception {
		// 得到StrutsAction类对象
		StrutsAction action=(StrutsAction)arg0.getAction();
		// 如果Action类的name属性值为“hello”，则返回错误页面
		if(action.getName().equals("hello"))
		{
			return Action.ERROR;
		}
		return arg0.invoke();
	}

}

```
struts.xml修改如下：
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
    "http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
    <package name="default" namespace="/" extends="struts-default">
    
    	<interceptors>
    		<interceptor name="myInterceptor" class="com.voidking.struts2.tool.MyInterceptor"></interceptor>
    	</interceptors>
    	<default-interceptor-ref name=""></default-interceptor-ref>
    	
        <default-action-ref name="index" />
        <action name="struts" class="com.voidking.struts2.action.StrutsAction">
            <result name="success">/welcome.jsp</result>
            <result name="error">/hello.jsp</result>
            <result name="input">/hello.jsp</result>
            
            <!-- 拦截配置在result后面 -->
            <!-- 使用系统默认拦截器栈 -->
            <interceptor-ref name="defaultStack"></interceptor-ref>
            <!-- 配置拦截器 -->
            <interceptor-ref name="myInterceptor"></interceptor-ref>
            
        </action>
    </package>
</struts>

```
Struts框架提供了
### XML代码提示
eclipse，Window，Preferences，XML，XML Files，Editor，Content Assist。
Auto activation delay(ms):，设置为200。
Prompt when these characters are inserted:，添加`"-abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ`。

### 小结
校验错误有提示fielderror，那么校验成功的提示该怎么搞？

### 参考文档
《Java EE基础实用教程》，郑阿奇主编
Servlet过滤器介绍之原理分析：http://zhangjunhd.blog.51cto.com/113473/20629/
struts2和servlet区别：http://blog.csdn.net/qiluluwawa/article/details/8619568
