title: IoC-DI的Java编程实现
date: 2014-11-10 22:10:53
tags:
- Spring
- java
categories: 设计开发
toc: true
---

### 实验要求
加深对控制反转和依赖注入的理解，使用Spring框架实现一个小的demo。


### 实验题目
IoC-DI的Java编程实现及Spring程序设计与实现


### 实验原理
IoC（控制反转）是一种软件设计模式，遵从了DIP（依赖倒置原则）。
DI（依赖注入），是IoC的实现方式。它提供一种机制，将需要依赖（低层模块）对象的引用传递给被依赖（高层模块）对象。
IoC容器：依赖注入的框架，用来映射依赖，管理对象创建和生存周期（DI框架）。
Spring框架：IoC容器。Spring的IOC容器主要使用DI方式实现的。不需要主动查找，对象的查找、定位和创建全部由容器管理。

<!--more-->
设计如下：
数据层提供接口IPersonDao，业务逻辑层提供接口IPersonService。IPersonService的实现PersonService中通过Spring容器调用IPersonDao。测试类PersonServiceTest，通过Spring容器调用IPersonService。

目录结构：
![](http://voidking.qiniudn.com/@/imgs/javaee/ioc.jpg)



### 实验代码
```
//IPersonDao.java
package com.voidking.ioc.dao;

public interface IPersonDao {
	
	public void save();
	
}

```

```
//PersonDao
package com.voidking.ioc.dao.impl;

import com.voidking.ioc.dao.IPersonDao;

public class PersonDao implements IPersonDao {
	@Override
	public void save() {
		System.out.println("------------from PersonDao.save()");
	}

}
```

```
//IPersonService.java
package com.voidking.ioc.service;

public interface IPersonService {
	
	public void processSave();
}
```

```
//PersonServiceTest.java
package com.voidking.ioc.service;

import org.junit.Before;
import org.junit.Test;
import org.springframework.beans.factory.BeanFactory;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class PersonServiceTest {
	private BeanFactory factory = null;	
	@Before
	public void before() {
		factory = new ClassPathXmlApplicationContext("applicationContext.xml");
	}	
	@Test
	public void testSpring() {
		IPersonService personService = (IPersonService) factory.getBean("personService");
		personService.processSave();
	}
}
```

```
//PersonService.java
package com.voidking.ioc.service.impl;

import com.voidking.ioc.dao.IPersonDao;
import com.voidking.ioc.service.IPersonService;

public class PersonService implements IPersonService {
	private IPersonDao personDao;
	
	public void setPersonDao(IPersonDao personDao) {
		this.personDao = personDao;
	}

	@Override
	public void processSave() {
		System.out.println("-------------from PersonService.processSave()");
		
		personDao.save();
	}

}
```


applicationContext.xml配置文件内容：
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx" xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="
     http://www.springframework.org/schema/beans 
     http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
     http://www.springframework.org/schema/tx 
     http://www.springframework.org/schema/tx/spring-tx-3.0.xsd
     http://www.springframework.org/schema/aop 
     http://www.springframework.org/schema/aop/spring-aop-3.0.xsd
     http://www.springframework.org/schema/context
     http://www.springframework.org/schema/context/spring-context-3.0.xsd">

	<bean id="personDao" class="com.voidking.ioc.dao.impl.PersonDao"></bean>
	
	<bean id="personService" class="com.voidking.ioc.service.impl.PersonService">
		<property name="personDao" ref="personDao"></property>
	</bean>	
</beans>
```

pom.xml配置文件内容：
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.voidking.ioc</groupId>
  <artifactId>ioc</artifactId>
  <packaging>war</packaging>
  <version>0.0.1-SNAPSHOT</version>
  <name>ioc Maven Webapp</name>
  <url>http://maven.apache.org</url>
  
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
    </dependency>
    
    <!-- 添加Spring依赖 -->
    <dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-core</artifactId>
		<version>3.1.1.RELEASE</version>
	</dependency>
	
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-beans</artifactId>
		<version>3.1.1.RELEASE</version>
	</dependency>
	
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-context</artifactId>
		<version>3.1.1.RELEASE</version>
	</dependency>
    
    <dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-jdbc</artifactId>
		<version>3.1.1.RELEASE</version>
	</dependency>
	
  </dependencies>
  <build>
    <finalName>ioc</finalName>
  </build>
</project>
```

### 源代码下载
https://github.com/voidking/ioc.git

### 参考文档
http://www.cnblogs.com/niuniu1985/archive/2010/01/13/1646375.html
http://www.cnblogs.com/liuhaorain/p/3747470.html
http://blog.csdn.net/m13666368773/article/details/8053138
