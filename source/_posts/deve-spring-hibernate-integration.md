title: Spring与Hibernate整合应用
date: 2014-02-22 18:10:24
tags: 
- Spring
- Hibernate
- JavaEE
categories: 设计开发
toc: true
---

### 创建数据库和表
其实我们在之前的例子中已经创建过了，数据库名xscj，表为dlb。详情请见《Hibernate与Struts2整合应用》。

### 创建项目
创建Dynamic Web Project，命名为hibernate_spring。

### 加载Spring框架
右击项目名，Properties，Java Build Path，Libraries，Add Library…，User Library，Next，User Libraries…，New…，输入“spring”，OK，Add External JARs…，选中需要的jar文件，打开，OK。

### 加载Hibernate框架
右击项目名，Properties，Java Build Path，Libraries，Add Library…，User Library，Next，User Libraries…，New…，输入“hibernate”，OK，Add External JARs…，选中需要的jar文件，打开，OK。
<!--more-->
### 生成POJO类和*.hbm.xml
Window，Open Perspective，Hibernate。

Add Configuration…，选中Project（hibernate_spring），选择Database connection，Setup… Propery file，Setup… Configuration file（需要进行一些配置）。

这时，会提示缺少MySQL驱动，在Classpath选项卡中配置一下就好了。

File，New，Hibernate Reverse Engineering File（reveng.xml），选中hibernate_spring，Next，选择Console configuration，Refresh，

在工具栏中，有三个三角号（绿色圆底），其中一个三角号的右下角有Hibernate的小标志。点击它的下拉菜单，Hibernate Code Generation Configurations…。

New，在Main选项卡中选择Console configuration，选择Output directory，勾选Reverse engineer from JDBC Connection。

在Exporters选项卡中勾选Domain code（.java）和Hibernate XML Mappings（.hbm.xml）。Run。

### 编写DlDao接口
```
package com.voidking.hibernate_spring.dao;

import com.voidking.hibernate_spring.model.Dlb;

public interface DlDao {
	public void save(Dlb dl);
}

```

### 编写DlDaoImp实现类
```
package com.voidking.hibernate_spring.dao.imp;

import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.classic.Session;

import com.voidking.hibernate_spring.dao.DlDao;
import com.voidking.hibernate_spring.model.Dlb;

public class DlDaoImp implements DlDao {
	// 依赖注入SessionFactory对象，set方法注入
	private SessionFactory sessionFactory;
	public void setSessionFactory(SessionFactory sessionFactory) {
		this.sessionFactory = sessionFactory;
	}

	@Override
	public void save(Dlb dl) {
		try {
			Session session = sessionFactory.openSession();
			Transaction ts = session.beginTransaction();
			session.save(dl);
			ts.commit();
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		}
		
	}

}

```

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
	<!-- 用Bean定义数据源 -->
	<bean id="datasource" class="org.apache.commons.dbcp.BasicDataSource">
		<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
		<property name="url" value="jdbc:mysql://localhost:3306/xscj"></property>
		<property name="username" value="scott"></property>
		<property name="password" value="tiger"></property>
	</bean>
	
	<!-- 定义Hibernate的SessionFactory -->
	<bean id="sessionFactory" class="org.springframework.orm.hibernate3.LocalSessionFactoryBean">
		<!-- 定义SessionFactory必须注入DataSource -->
		<property name="dataSource">
			<ref bean="datasource"/>
		</property>
		
		<!-- 定义Hibernate的SessionFactory属性 -->
		<property name="hibernateProperties">
			<props>
				<prop key="hibernate.dialect">
					org.hibernate.dialect.MySQLDialect
				</prop>
			</props>
		</property>
		
		<!-- 定义POJO的映射文件 -->
		<property name="mappingResources">
			<list>
				<value>/com/voidking/hibernate_spring/model/Dlb.hbm.xml</value>
			</list>
		</property>
	</bean>
	
	<!-- 注入dlDao -->
	<bean id="dlDao" class="com.voidking.hibernate_spring.dao.imp.DlDaoImp">
		<property name="sessionFactory">
			<ref bean="sessionFactory"/>
		</property>
	</bean>
</beans>

```
Spring的Bean很好地管理了以前在hibernate.cfg.xml文件中创建的SessionFactory，使文件更易阅读。

### 编写测试类
```
package com.voidking.hibernate_spring.test;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.FileSystemXmlApplicationContext;

import com.voidking.hibernate_spring.dao.DlDao;
import com.voidking.hibernate_spring.model.Dlb;

public class Test {
	public static void main(String[] args) {
		Dlb dlb = new Dlb();
		dlb.setXh("081109");
		dlb.setKl("123456");
		ApplicationContext context = new FileSystemXmlApplicationContext("src/applicationContext.xml");
		DlDao dlDao = (DlDao)context.getBean("dlDao");
		dlDao.save(dlb);
		
	}
}

```

### 运行报错
`Cannot find class [org.apache.commons.dbcp.BasicDataSource]`

缺少commons-dbcp.jar、commons-pool.jar这两个包，在spring-framework-\*/lib/jakarta-commons目录下可以找到，添加进路径即可。

上述问题解决之后，运行成功，数据插入成功。但是，并不能运行第二次，因为`Duplicate entry '0' for key 'PRIMARY'`，且不去管它。

### 源代码分享

https://github.com/voidking/hibernate_spring.git

### 小结
这个工程中，并不需要hibernate.cfg.xml、hibernate.properties，因为它们完成的工作，applicationContext帮它们完成了。

### 参考文档
《Java EE基础实用教程》，郑阿奇主编

------
Cannot find class [org.apache.commons.dbcp.BasicDataSource]
http://blog.csdn.net/a105421548/article/details/43016953

------

