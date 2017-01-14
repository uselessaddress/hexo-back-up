title: Spring概述
date: 2014-02-22 10:10:24
tags:
- Spring
- JavaEE
categories: 设计开发
toc: true
---

### 名词解释
简单来说，Spring是一个轻量级的控制反转（IoC）和面向切面（AOP）的容器框架。

Spring框架是Rod Johnson开发的，2003年发布了Spring框架的第一个版本。Spring是一个从实际开发中抽取出来的框架，因此它完成了大量开发中的通用步骤，从而大大提高了企业应用的开发效率。

Spring为企业应用的开发提供了一个轻量级的解决方案。其中依赖注入、基于AOP的声明式事务管理、多种持久层的整合与优秀的Web MVC框架等最为人们关注。Spring可以贯穿程序的各层之间，但它并不是要取代那些已有的框架，而是以高度的开发性与它们紧密地整合，这也是Spring被广泛应用的原因之一。

Spring使用最基本的JavaBean来完成以前只可能由EJB完成的事情。然而，Spring的用途不仅限于服务器端的开发。从简单性、可测试性和松耦合性的角度而言，任何Java应用都可以从Spring中受益。

### 官网
http://spring.io

<!--more-->
### 下载
#### 方法一
http://repo.spring.io
搜索spring-framework，选择下载即可。

#### 方法二
1)打开eclipse-help-Install New Software...
2)Add...，弹出Add Repository。 
4)对话框里Name随意，Location输入：http://springide.org/updatesite/ ，点击OK。 
5)选择Core/Spring IDE，Next。


### 导入包
Spring4的包，官方没有给出下载，只给出了Maven的引用方法，个人感觉非常好。
Spring3及以前的包，可以使用Maven引用，也可以下载后导入jar包。

spring-framework-**.zip解压后,将spring-framework-**文件夹的dist目录下的jar包导入工程中。

### 分层架构

Spring框架的主要优势之一是其分层架构，分层架构允许选择使用任何一个组件，同时为J2EE应用程序开发提供集成的框架。Spring框架的功能可以用在任何J2EE服务器中，大多数功能也适用于不受管理的环境。Spring的核心要点是：支持不绑定到特定J2EE服务的可重用业务和数据访问对象。这样的对象可以在不同J2EE环境（Web或EJB）、独立应用程序、测试环境之间重用。

![Spring框架的组件结构图](http://www.loogdao.com/wp-content/uploads/2012/03/0_133161822834s4.gif)
组成Spring 框架的每个模块（或组件）都可以单独存在，或者与其他一个或多个模块联合实现。每个模块的功能如下：

- 核心容器：核心容器提供Spring 框架的基本功能。核心容器的主要组件是BeanFactory，它是工厂模式的实现。BeanFactory使用控制反转（IOC）模式将应用程序的配置和依赖性规范与实际的应用程序代码分开。

- Spring 上下文：Spring 上下文是一个配置文件，向Spring 框架提供上下文信息。Spring 上下文包括企业服务，例如JNDI 、EJB、电子邮件、国际化、校验和调度功能。

- Spring AOP ：通过配置管理特性，Spring AOP 模块直接将面向方面的编程功 能集成到了Spring 框架中。所以，可以很容易地使Spring 框架管理的任何对象支持AOP 。Spring AOP 模块为基于Spring 的应用程序中的对象提供了事务管理服务。通过使用Spring AOP ，不用依赖EJB 组件，就可以将声明性事务管理集成到应用程序中。

- Spring DAO ：JDBC DAO抽象层提供了有意义的异常层次结构，可用该结构来管理异常处理和不同数据库供应商抛出的错误消息。异常层次结构简化了错误处理，并且极大地降低了需要编写的异常代码数量（例如打开和关闭连接）。Spring DAO 的面向JDBC 的异常遵从通用的DAO 异常层次结构。

- Spring ORM ：Spring 框架插入了若干个ORM 框架，从而提供了ORM 的对象关系工具，其中包括JDO 、Hibernate 和iBatisSQLMap 。所有这些都遵从Spring 的通用事务和DAO异常层次结构。

- Spring Web：为基于Web的应用程序提供上下文。它建立在应用程序上下文模块之上，简化了处理多份请求及将请求参数绑定到域对象的工作。Spring框架支持Jakarta Struts的集成。

- Spring Web MVC：是一个全功能Web应用程序的MVC实现。通过策略接口实现高度可配置，MVC容纳了大量视图技术，其中包括JSP、Velocity、Tiles、iText和POI。

### 依赖注入
Spring的核心机制是依赖注入（Dependency Inversion），也称为控制反转。

#### 工厂模式

##### Human.java

```
package com.voidking.factory;

public interface Human {
	void eat();
	void walk();
}

```

##### Chinese.java
```
package com.voidking.factory;

public class Chinese implements Human {

	@Override
	public void eat() {
		// TODO Auto-generated method stub
		System.out.println("中国人很会吃！");

	}

	@Override
	public void walk() {
		// TODO Auto-generated method stub
		System.out.println("中国人健步如飞！");
	}

}

```
##### American.java
```
package com.voidking.factory;

public class American implements Human {

	@Override
	public void eat() {
		// TODO Auto-generated method stub
		System.out.println("美国人吃西餐！");
	}

	@Override
	public void walk() {
		// TODO Auto-generated method stub
		System.out.println("美国人经常坐车！");
	}

}

```

##### Factory.java
```
package com.voidking.factory;

public class Factory {
	public Human getHuman(String name)
	{
		if(name.equals("Chinese"))
		{
			return new Chinese();
		}else if (name.equals("American")) {
			return new American();
		}else {
			throw new IllegalArgumentException("参数不正确");
		}
	}
}

```

##### Test.java
```
package com.voidking.factory;

public class Test {
	public static void main(String[] args) {
		Human human=null;
		human= new Factory().getHuman("Chinese");
		human.eat();
		human.walk();
		
		human= new Factory().getHuman("American");
		human.eat();
		human.walk();
	}
}

```

#### 依赖注入应用
上面工厂模式中，甲组件需要乙组件的对象的时候，无需直接创建其实例，而是通过工厂获得，只要创建一个工厂即可。而Spring容器则提供了更好的办法，开发人员不用创建工厂，可以直接使用Spring提供的依赖注入方式。可以把上例修改为使用Spring容器来创建对象。

##### 引入Spring的jar包
spring.jar、spring-source.jar、commons-logging.jar。

##### applicationContext.xml
在src文件夹下，新建文件applicationContext.xml，内容如下：
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
	<bean id="chinese" class="com.voidking.factory.Chinese"></bean>
	<bean id="american" class="com.voidking.factory.American"></bean>	

</beans>

```

##### 修改Test.java
```
package com.voidking.factory;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.FileSystemXmlApplicationContext;

public class Test {
	public static void main(String[] args) {
		/*
		Human human=null;
		human= new Factory().getHuman("Chinese");
		human.eat();
		human.walk();
		
		human= new Factory().getHuman("American");
		human.eat();
		human.walk();
		*/
		
		ApplicationContext ctx = new FileSystemXmlApplicationContext("src/applicationContext.xml");
		Human human = null;
		human =(Human)ctx.getBean("chinese");
		human.eat();
		human.walk();
		human = (Human)ctx.getBean("american");
		human.eat();
		human.walk();
	}
}

```
所谓依赖注入，就是在运行的过程中，如果需要调用另一个对象协助时，无需在代码中创建被调用者，而是依赖于外部的注入。Spring的依赖注入对调用者和被调用者几乎没有任何要求，完全支持POJO之间依赖关系的管理。依赖注入通常有两种：


#### 设置注入

设置注入是通过set方法注入被调用者的实例。这种方法简单、直观，很容易理解，因而Spring的依赖注入被大量使用。

##### Human.java
```
package com.voidking.dependencyinversion;

public interface Human {
	void speak();
}

```

##### Languige.java
```
package com.voidking.dependencyinversion;

public interface Languige {
	public String kind();
}

```

##### Chinese.java
```
package com.voidking.dependencyinversion;

public class Chinese implements Human {
	
	private Languige lan;
	@Override
	public void speak() {
		// TODO Auto-generated method stub
		System.out.println(lan.kind());
	}

	public void setLan(Languige lan) {
		this.lan = lan;
	}
}

```

##### English.java
```
package com.voidking.dependencyinversion;

public class English implements Languige {

	@Override
	public String kind() {
		// TODO Auto-generated method stub
		return "中国人也会说英语！";
	}

}

```

##### applicationContext.xml
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
	<bean id="chinese" class="com.voidking.dependencyinversion.Chinese">
		<property name="lan" ref="english"></property>
	</bean>
	
	<bean id="english" class="com.voidking.dependencyinversion.English"></bean>

</beans>

```
各个Bean之间的依赖关系放在配置文件中完成，而不是用代码体现。通过配置文件，Spring能精确地为每个Bean注入属性。注意，配置文件的Bean的class属性值，不能是接口，必须是真正的实现类。

Spring会自动接管每个Bean定义里的property元素定义。Spring会在执行无参数的构造器并创建默认的Bean实例后，调用对应的set方法为程序注入属性值。

每个Bean的id属性是该Bean的唯一标识，程序通过id属性访问Bean。而且各个Bean之间的依赖关系也通过id属性关联。

##### Test.java
```
package com.voidking.dependencyinversion;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.FileSystemXmlApplicationContext;


public class Test {
	public static void main(String[] args) {
		ApplicationContext ctx = new FileSystemXmlApplicationContext("src/applicationContext.xml");
		Human human = null;
		human =(Human)ctx.getBean("chinese");
		human.speak();
	}
}

```


#### 构造注入
在构造实例时，已经为其完成了属性的初始化，利用构造函数来设置依赖注入的方法，成为构造注入。
在前面dependencyinversion工程的基础上修改。

##### 修改Chinese.java
```
package com.voidking.dependencyinversion2;

public class Chinese implements Human {
	
	private Languige lan;
	
	public Chinese() {
		super();
		// TODO Auto-generated constructor stub
	}

	public Chinese(Languige lan) {
		super();
		this.lan = lan;
	}

	@Override
	public void speak() {
		// TODO Auto-generated method stub
		System.out.println(lan.kind());
	}

}

```

##### 修改applicationContext.xml
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
	<bean id="chinese" class="com.voidking.dependencyinversion2.Chinese">
		<constructor-arg ref="english"></constructor-arg>
	</bean>
	
	<bean id="english" class="com.voidking.dependencyinversion2.English"></bean>

</beans>

```
设置注入和构造注入，区别在于创建Human实例中的Languige属性的时间不同。（呃，前面命名的时候搞错了，Language写成了Languige，领会精神。。。）
设置注入是先创建一个默认的Bean实例，然后调用对应的set方法注入依赖关系；而构造注入则在创建Bean实例时，已经完成了依赖关系的注入。

### Spring核心接口

#### BeanFactory
Bean工厂，由org.springframework.beans.factory.BeanFactory接口定义。

BeanFactory采用工厂设计模式。这个接口负责创建和分发Bean，但与其他工厂模式的实现不同，它们只分发一种类型的对象。BeanFactory是一个通用的工厂，可以创建和分发各种类型的Bean。

在Spring中有几种BeanFactory的实现，其中最常用的是org.springframework.bean.factory.xml.XmlBeanFactory。它根据XML文件中的定义装载Bean。

要创建XMLBeanFactory，需要传递一个java.io.InputStream对象给构造函数。InputStream对象提供XML文件给工厂。例如，下面的例子使用一个java.io.FileInputStream对象把Bean XML定义文件给XMLBeanFactory：
```
BeanFactory factory = new XmlBeanFactory(new FileInputStream("applicationContext.xml"));
```
这行代码告诉BeanFactory从XML文件中读取Bean的定义信息，但是现在BeanFactory没有实例化Bean，Bean被延迟加载到BeanFactory中，就是说BeanFactory会立即把Bean定义信息加载进来，但是Bean只有在需要的时候才被实例化。

为了从BeanFactory得到Bean，只要简单地调用getBean()方法，把需要的Bean的名字当做参数传递进去就行了。由于得到的是Object类型，所以要进行强制类型转化。
```
MyBean myBean = (MyBean)factory.getBean("myBean");
```
当getBean()方法被调用时，工厂就会实例化Bean，并使用依赖注入开始设置Bean的属性。这样就在Spring容器中开始了Bean的生命周期。


#### ApplicationContext
应用上下文，由org.springframework.context.ApplicationContext接口定义，是BeanFactory的子接口。

BeanFactory对简单应用来说已经很好了，但是为了获得Spring框架的强大功能，需要使用Spring更高级的容器——ApplicationContext。

表面上，ApplicationContext和BeanFactory差不多。两者都是载入Bean定义信息，装配Bean，根据需要分发Bean。但是ApplicationContext提供了更多功能：
1、应用上下文提供了文本解析工具，包括对国际化的支持。
2、应用上下文提供了载入文本资源的通用方法，如载入图片。
3、应用上下文可以向注册为监听器的Bean发送事件。

由于它提供了附加功能，几乎所有的应用系统都选择ApplicationContext，而不是BeanFactory。

在ApplicationContext的诸多实现中有三个常用的实现：
```
ApplicationContext context = new FileSystemXmlApplicationContext("c:/foo.xml");
ApplicationContext context = new FileSystemXmlApplicationContext("foo.xml");
ApplicationContext context = WebApplicationContextUtils.getWebApplicationContext(request.getSession().getServletContext());

```
FileSystemXmlApplicationContext只能在指定的路径中寻找foo.xml文件，而FileSystemXmlApplicationContext可以在整个类路径中寻找foo.xml。

ApplicationContext与BeanFactory的另一个重要区别是单实例Bean如何被加载。BeanFactory延迟加载所有Bean，直到getBean()被调用时，Bean才被创建。ApplicationContext则聪明一点，它会在上下文启动后预载入所有的单实例Bean。通过预载入单实例Bean，确保需要的时候它们已经准备好了，应用程序不需要等待它们被创建。

### Spring基本配置
在Spring容器中拼接Bean叫做装配。装配Bean实际上是告诉容器需要哪些Bean，以及容器如何使用依赖注入，将它们配合起来。

#### 使用XML装配
理论上，Bean装配可以从任何配置资源获得。但实际上，XML是最常见的Spring应用系统配置源。
```
<?xml version="1.0" encoding="UTF-8"?>
<beans...>
	<bean id="chinese" class="com.voidking.factory.Chinese"></bean>
	<bean id="american" class="com.voidking.factory.American"></bean>	
</beans>
```
在XML文件定义Bean，上下文定义文件的根元素是<beans>。<beans>有多个<bean>子元素，每个<bean>元素定义了一个Bean（任何一个Java对象）如何被装配到Spring容器中。

#### 添加一个Bean

在Spring中对一个Bean的最基本配置包括Bean的id和它的全称类名。向Spring容器中添加一个Bean只需要向XML文件中添加一个<bean>元素。

当通过Spring容器创建一个Bean时，不仅可以完成Bean实例的实例化，还可以为Bean指定特定的作用域。

1、原型模式与单实例模式：Spring中的Bean默认情况下是单实例模式。在容器分配Bean的时候，它总是返回同一个实例。但是，如果每次向ApplicationContext请求一个Bean的时候需要得到一个不同的实例，需要将Bean定义为原型模式。
```
<bean id="chinese" class="com.voidking.factory.Chinese" singleton="false"></bean> //原型模式Bean
```
2、request或session：对于每次HTTP请求或HttpSession，使用request或session定义的Bean都将产生一个新实例，即每次HTTP请求或HttpSession将会产生不同的Bean实例。只有在Web应用中使用Spring时，该作用域有效。

3、golbal session：每个全局的HttpSession对应一个Bean实例。典型情况下，仅在使用portlet context的时候有效。只有在Web应用中使用Spring时，该作用域才有效。

当一个Bean实例化的时候，有事需要做一些初始化的工作，然后才能使用。同样，当Bean不再需要，从容器中删除时，需要按顺序做一些清理工作。因此，Spring可以在创建和拆卸Bean的时候调用Bean的两个生命周期方法。

在Bean的定义中设置自己的init-method，这个方法在Bean被实例化时马上被调用。同样，也可以设置自己的destroy-method，这个方法在Bean从容器中删除之前调用。

一个典型的例子是连接池Bean：
```
public class MyConnectionPool{
	...
	public void initailize(){}
	public void close(){}
	....
}
```
Bean的定义如下：
```
<bean id="connectionPool" class="com.voidking.test.MyConnectionPool" init-method="initialize" destroy-method="close"></bean>
```
MyConnectionPool被实例化后，initialize方法马上被调用，给Bean初始化的机会。在Bean从容器中删除前，close方法将释放数据库连接。

### 源代码分享

https://github.com/voidking/factory.git

https://github.com/voidking/dependencyinversion.git

https://github.com/voidking/dependencyinversion2.git



### 参考文档
《Java EE基础实用教程》，郑阿奇主编

------
J2EE 领域的一些技术框架结构图
http://www.oschina.net/question/28_47106

------
Caused by:java.lang.ClassNotFoundException: org.apache.commons.logging.LogFactory
http://blog.csdn.net/liuxilil/article/details/5734079

------