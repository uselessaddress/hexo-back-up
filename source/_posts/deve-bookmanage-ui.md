title: "图书管理系统之ui"
toc: true
date: 2015-08-03 22:00:12
tags:
- JavaEE
- Struts2
categories: 设计开发
---
# 前言
这三篇博文，是一个整体，单独看也许艰涩难懂。但是，合起来看，很多疑问就可以迎刃而解。正如马哲所说，整体功能可以大于部分功能之和。

# 新建工程
1、打开Eclipse，File，New，Maven Project，勾选Create a simple project，Next。

2、填写Group Id和Artifact Id，Packaging选择war。

<!--more-->

相当于命令：
```
mvn archetype:create -DgroupId=com.voidking.book -DartifactId=book-ui
-DarchetypeArtifactId=maven-archetype-webapp
```

# pom.xml

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
		<groupId>com.voidking.book</groupId>
		<artifactId>book-parent</artifactId>
		<version>0.0.1-SNAPSHOT</version>
		<relativePath>../book-parent/pom.xml</relativePath>
	</parent>
  <artifactId>book-ui</artifactId>
  
  <packaging>war</packaging>  
  <dependencies>
		<!-- struts2框架 -->
		<dependency>
			<groupId>org.apache.struts</groupId>
			<artifactId>struts2-core</artifactId>
		</dependency>
		<dependency>
			<groupId>org.apache.struts</groupId>
			<artifactId>struts2-spring-plugin</artifactId>
		</dependency>
		<dependency>
			<groupId>org.apache.struts</groupId>
			<artifactId>struts2-json-plugin</artifactId>
		</dependency>
		<dependency>
			<groupId>org.apache.struts</groupId>
			<artifactId>struts2-convention-plugin</artifactId>
		</dependency>
		<!-- Spring 核心库 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-beans</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-orm</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aop</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-tx</artifactId>
		</dependency>
		<!-- Spring MVC 库 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
		</dependency>
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjweaver</artifactId>
		</dependency>

		<!-- servlet api -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
		</dependency>

		<!-- 引用业务层的jar包 -->
		<dependency>
			<groupId>com.voidking.book</groupId>
			<artifactId>book-logic</artifactId>
			<version>0.0.1-SNAPSHOT</version>
			<exclusions>
				<exclusion>
					<groupId>org.hibernate</groupId>
					<artifactId>hibernate-entitymanager</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-entitymanager</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
		</dependency>
	</dependencies>
	<build>
		<plugins>
			<plugin>
				<groupId>org.mortbay.jetty</groupId>
				<artifactId>maven-jetty-plugin</artifactId>
				<configuration>
					<scanIntervalSeconds>10</scanIntervalSeconds>
					<connectors>
						<connector implementation="org.mortbay.jetty.nio.SelectChannelConnector">
							<port>8099</port>
							<maxIdleTime>60000</maxIdleTime>
						</connector>
					</connectors>
				</configuration>
			</plugin>
		</plugins>
	</build>
</project>

```
值得一提的是，maven的web工程中，习惯用jetty而不是tomcat。jetty发布的工程，在当前工程下的target/相应版本号下。比如本工程，发布后在target/book-ui-0.0.1-SNAPSHOT下。

# 新建包
新建包com.voidking.book.admin.action、com.voidking.book.bookbase.action、com.voidking.book.bookkind.action、com.voidking.book.readerbase.action、com.voidking.book.readerkind.action、com.voidking.book.borrowinfo.action、com.voidking.book.search.action、com.voidking.book.statistics.action。

# admin.action包
在admin.action包，新建LoginAction.java，内容如下。
```
package com.voidking.book.admin.action;

import org.apache.struts2.convention.annotation.Action;
import org.apache.struts2.convention.annotation.Namespace;
import org.apache.struts2.convention.annotation.ParentPackage;
import org.apache.struts2.convention.annotation.Result;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;

import com.opensymphony.xwork2.ActionContext;
import com.opensymphony.xwork2.ActionSupport;
import com.voidking.book.entity.Admin;
import com.voidking.book.logic.AdminLogicService;

@Controller
@Namespace("/admin")
@ParentPackage("custom-default")
public class LoginAction extends ActionSupport {

	private static final long serialVersionUID = 2936224923889056993L;

	@Autowired
	private AdminLogicService adminLogicService;

	private Admin admin;
	private String info;

	public Admin getAdmin() {
		return admin;
	}

	public void setAdmin(Admin admin) {
		this.admin = admin;
	}

	public String getInfo() {
		return info;
	}

	public void setInfo(String info) {
		this.info = info;
	}

	@Action(value = "login", results = { @Result(name = "success", type = "json") })
	public String login() throws Exception {

		this.info = this.adminLogicService.login(admin);

		if (admin.getId() != null)// 登录成功
		{
			ActionContext.getContext().getSession().put("admin", admin);
		}
		return SUCCESS;
	}
}

```
1、@Controller，Spring的注解。在SpringMVC中提供了一个非常简便的定义Controller的方法，你无需继承特定的类或实现特定的接口，只需使用@Controller标记一个类是Controller，然后使用@RequestMapping和@RequestParam等一些注解用以定义URL请求和Controller方法之间的映射，这样的Controller就能被外界访问到。此外Controller不会直接依赖于HttpServletRequest和HttpServletResponse等HttpServlet对象，它们可以通过Controller 的方法参数灵活的获取到。

2、我们知道通常情况下，Struts2是通过struts.xml配置的。但是随着系统规模的加大我们需要配置的文件会比较大，虽然我们可以根据不同的系统功能将不同模块的配置文件单独书写，然后通过include节点将不同的配置文件引入到最终的struts.xml文件中，但是毕竟还是要维护和管理这些文件，因此也会给维护工作带来很大的困扰。为了解决这个问题，可以考虑使用struts2的注解。实际上struts2中最主要的概念就是package、action以及Interceptor等等概念，所以只要明白这些注解就可以了。

3、@Namespace，Struts2的注解。命名空间，也就是xml文件中package的namespace属性。如果没有@Namespace("/admin")注解，按照Convention Plugin的约定，会将此包作为根包，对应Action URL的命名空间为“/”。

4、@ParentPackage，Struts2的注解。这个注解对应了xml文件中的package节点，它只有一个属性叫value，其实就是package的name属性；

5、@Action，Struts2的注解。这个注解对应action节点。这个注解可以应用于 action 类上，也可以应用于方法上。这个注解中有几个属性：
- value，表示action的URL，也就是action节点中的name属性；
- results，表示action的多个result，这个属性是一个数组属性，因此可以定义多个Result；
- interceptorRefs，表示action的多个拦截器，这个属性也是一个数组属性，因此可以定义多个拦截器；
- params，这是一个String类型的数组，它按照name/value的形式组织，是传给action的参数；
- exceptionMappings，这是异常属性，它是一个ExceptionMapping的数组属性，表示action的异常，在使用时必须引用相应的拦截器；

6、@Result ，Struts2的注解。这个注解对应了result节点。这个注解只能应用于 action 类上。这个注解中也有几个属性：
- name，表示action方法的返回值，也就是result节点的name属性，默认情况下是success；
- location，表示view层文件的位置，可以是相对路径，也可以是绝对路径；
- type，是action的类型，比如redirect；
- params，是一个String数组。也是以name/value形式传送给result的参数；

# struts.xml
在src/main/resources文件下，新建struts.xml，内容如下。
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC "-//Apache Software Foundation//DTD Struts Configuration 2.1//EN" "http://struts.apache.org/dtds/struts-2.1.dtd">
<struts>
	<constant name="struts.multipart.maxSize" value="99999999999" />
	<constant name="struts.allowed.action.names" value="[a-zA-Z0-9._!/\-]*"></constant>
	<constant name="struts.multipart.saveDir" value="/tmp" />
	<constant name="struts.devMode" value="true" />
	<constant name="struts.i18n.encoding" value="UTF-8" />
	<package name="custom-default" extends="json-default">
		<interceptors>
			<!--定义拦截器 name:拦截器名称 class:拦截器类路径-->
			<!-- 定义拦截器栈 -->
			<interceptor-stack name="myDefaultStack">
				<interceptor-ref name="json" />
				<interceptor-ref name="defaultStack" />
			</interceptor-stack>
		</interceptors>
		<default-interceptor-ref name="myDefaultStack" />
	</package>
</struts>    

```
1、struts.properties文件的内容均可在struts.xml中以`<constant name="" value=""></constant>`加载。

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.3//EN"
    "http://struts.apache.org/dtds/struts-2.3.dtd">

<struts>

    <!-- 把它设置为开发模式，发布时要设置为false -->
    <constant name="struts.devMode" value="true" />
    <!-- 设置在class被修改时是否热加载，发布时要设置为false -->
    <constant name="struts.convention.classes.reload" value="true"/>
    <!-- 自动动态方法的调用，使用这个设置后可以这样调用：action!method -->
    <constant name="struts.enable.DynamicMethodInvocation" value="true" />
    <!-- 指定jsp文件所在的目录地址 -->
    <constant name="struts.convention.result.path" value="/WEB-INF/content/" />
    <!-- 使用struts-default默认的转换器，如果是rest的使用：rest-default，rest需要rest的jar插件 -->
    <constant name="struts.convention.default.parent.package" value="struts-default"/>
    <!-- 用于配置包名后缀。默认为action、actions、struts-->
    <constant name="struts.convention.package.locators" value="actions" />
    <!-- 用于配置类名后缀，默认为Action，设置后，Struts2只会去找这种后缀名的类做映射 -->
    <constant name="struts.convention.action.suffix" value="Action"/>
    <!-- 设置即使没有@Action注释，依然创建Action映射。默认值是false。因为Convention-Plugin是约定优于配置的风格，
        可以不通过注解根据预先的定义就能访问相应Action中的方法 -->
    <constant name="struts.convention.action.mapAllMatches" value="true"/>
    <!-- 自定义jsp文件命名的分隔符 -->
    <constant name="struts.convention.action.name.separator" value="-" />
    <!-- 国际化资源文件名称 -->
    <constant name="struts.custom.i18n.resources" value="i18n" />
    <!-- 是否自动加载国际化资源文件  -->
    <constant name="struts.i18n.reload" value="true" />
    <!-- 浏览器是否缓存静态内容 -->
    <constant name="struts.serve.static.browserCache" value="false" />
     <!-- 上传文件大小限制设置 -->
    <constant name="struts.multipart.maxSize" value="-1" />
    <!-- 主题，将值设置为simple，即不使用UI模板。这将不会生成额外的html标签 -->
    <constant name="struts.ui.theme" value="simple" />
    <!-- 编码格式 -->
    <constant name="struts.i18n.encoding" value="UTF-8" />

</struts>
```

2、package有以下几个常用属性：
- name：该属性是必选的，指定包的名字，这个名字将作为引用该包的键。注意，包的名字必须是唯一的，在一个struts.xml文件中不能出现两个同名的包。
- extends：该属性是可选的，允许一个包继承一个或多个先前定义的包。
- abstract：该属性是可选的，将其设置为true，可以把一个包定义为抽象的。抽象包不能有action定义，它只能作为“父”包，被其他包所继承。注意，以为Struts2的配置文件是从上到下处理的，所以父包应该在子包前面定义。
- namespace：该属性是可选的，将保存的action配置为不同的名称空间。
如果接收到一个请求为/space/main.action，框架将首先查找/space名称空间，如果找到了，则执行main.action；如果没有找到，则到默认的名称空间（name=”default”）中继续查找。
如果接收到一个请求为/main.action，框架将首先查找“/”名称空间，如果找到了，则执行main.action；如果没有找到，则到默认的名称空间（name=”default”）中继续查找。

# 配置文件
需要注意的是，在前两层中，我们的配置文件全部放到了src/test/resources中。但是，在这一层，我们的配置文件主要放在两个位置。一个是src/main/resources，另一个是src/main/webapp/WEB-INF。

1、在resources下，有struts.xml、database-conn.properties、log4j.properties。
2、在WEB-INF下，有applicationContext.xml、spring-persist.xml、web.xml。

# web.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://java.sun.com/xml/ns/javaee"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	version="2.5">
	<display-name>voidking</display-name>

	<filter>
		<filter-name>entityManagerFilter</filter-name>
		<filter-class>org.springframework.orm.jpa.support.OpenEntityManagerInViewFilter</filter-class>
	</filter>

	<filter-mapping>
		<filter-name>entityManagerFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>

	<filter>
		<filter-name>struts2</filter-name>
		<filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
	</filter>
	<filter-mapping>
		<filter-name>struts2</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
</web-app>
```
1、display-name，如果使用工具编辑部署描述符，display-name元素包含的就是XML编辑器显示的名称。

2、filter，用于指定Web容器中的过滤器。在请求和响应对象被servlet处理之前或之后，可以使用过滤器对这两个对象进行操作。配合filter-mapping，过滤器被映射到一个servlet或一个URL。这个过滤器的filter元素和filter-mapping元素必须具有相同的名称。


# applicationContext.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://www.springframework.org/schema/beans"
	xsi:schemaLocation="
    http://www.springframework.org/schema/beans 
    http://www.springframework.org/schema/beans/spring-beans.xsd">
    <import resource="spring-persist.xml"/>
    
</beans>
```

# 工作流程
1、客户端提交一个HttpServletRequest请求。
 
2、请求被提交到一系列Filter过滤器，如ActionCleanUp和FilterDispatcher等。
 
3、FilterDispatcher是Struts2控制器的核心，它通常是过滤器链中的最后一个过滤器。
 
4、请求被发送到FilterDispatcher后，FilterDispatcher询问ActionMapper时候需要调用某个action来处理这个Request。
 
5、如果ActionMapper决定需要调用某个action，FilterDispatcher则把请求交给ActionProxy进行处理。
 
6.ActionProxy通过Configuration Manager询问框架的配置文件struts.xml（本项目实现struts2零配置，所以询问action文件），找到调用的action类。
 
7.ActionProxy创建一个ActionInvocation实例，通过代理模式调用Action。
 
8.action执行完毕后，返回一个result字符串，此时再按相反的顺序通过Intercepter拦截器。
 
9.最后ActionInvocation实例，负责根据struts.xml中配置result元素，找到与之相对应的result，决定进一步输出。


# spring-persist.xml加载问题

在persist层和logic层测试的时候，我们是手动加载spring-persist.xml文件。那么，在ui层，spring-perisist.xml文件的加载流程是怎样的呢？
在web.xml中，可以通过`<context-param>`指定Spring配置文件，如果没有这个标签，则默认加载WEB-INF下的applicationContext.xml，也可以用它指定其他配置文件。

# 前端
前端使用BootStrap+AngularJs，其中BootStrap负责界面显示，AngularJs则是沟通前后端的桥梁。

这个部分比较复杂，给个例子，讲的很好。
http://www.runoob.com/angularjs/angularjs-application.html

# 效果演示

![](http://7oxjrx.com1.z0.glb.clouddn.com/%40%2Fimgs%2Fbook%2Fui%2F1.jpg)
![](http://7oxjrx.com1.z0.glb.clouddn.com/%40%2Fimgs%2Fbook%2Fui%2F2.jpg)
![](http://7oxjrx.com1.z0.glb.clouddn.com/%40%2Fimgs%2Fbook%2Fui%2F3.jpg)
![](http://7oxjrx.com1.z0.glb.clouddn.com/%40%2Fimgs%2Fbook%2Fui%2F4.jpg)
![](http://7oxjrx.com1.z0.glb.clouddn.com/%40%2Fimgs%2Fbook%2Fui%2F5.jpg)
![](http://7oxjrx.com1.z0.glb.clouddn.com/%40%2Fimgs%2Fbook%2Fui%2F6.jpg)


# 后记
这个图书管理系统，使用的技术，很多小编也是一知半解。接下来，会有一篇文章，专门针对这个项目进行改进。
全部代码自取https://github.com/voidking/bookmanage.git

# 参考文档

spring的applicationContext.xml如何自动加载
http://blog.csdn.net/fuqingtian/article/details/5545860

Spring controller
http://my.oschina.net/hcliu/blog/396887

Struts2注解配置之@Namespace(四)
http://blog.csdn.net/spyjava/article/details/13764757

Struts2注解使用说明
http://blog.csdn.net/wk313753744/article/details/19920195

Struts2的注解功能详解
http://my.oschina.net/victorHomePage/blog/56732
http://www.cnblogs.com/wayne_wang/archive/2011/01/24/1942927.html

Struts2 - 常用的constant总结
http://www.cnblogs.com/HD/p/3653930.html

Struts2 常用的常量配置
http://www.cnblogs.com/yokoboy/archive/2013/01/25/2877145.html

struts2.0中struts.xml配置文件详解
http://www.cnblogs.com/kay/archive/2007/11/28/976120.html

Web.xml详解
http://www.cnblogs.com/konbluesky/articles/1925295.html

web.xml详细介绍
http://mianhuaman.iteye.com/blog/1105522

struts2工作流程
http://huaxia524151.iteye.com/blog/1430148
