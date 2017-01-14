title: "图书管理系统之logic"
toc: true
date: 2015-08-02 22:00:12
tags:
- JavaEE
categories: 设计开发
---
# 前言
persist层基本搞清楚了，下面我们接着看logic层。

# 新建工程
1、打开Eclipse，File，New，Maven Project，勾选Create a simple project，Next。

2、填写Group Id和Artifact Id，Packaging选择jar。

<!--more-->

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
	<artifactId>book-logic</artifactId>

	<dependencies>
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
			<groupId> org.aspectj</groupId>
			<artifactId>aspectjweaver</artifactId>
		</dependency>

		<!-- persist -->
		<dependency>
			<groupId>com.voidking.book</groupId>
			<artifactId>book-persist</artifactId>
			<version>0.0.1-SNAPSHOT</version>
		</dependency>

		<!-- 日志打印 -->


		<!-- junit -->
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<scope>test</scope>
		</dependency>

		<!-- hibernate -->
		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-entitymanager</artifactId>
			<scope>provided</scope>
		</dependency>

	</dependencies>
  
</project>
```

# logic包
新建包com.voidking.book.logic，在该包下，新建AdminLogicService.java，内容如下。
```
package com.voidking.book.logic;

import com.voidking.book.entity.Admin;

public interface AdminLogicService {
	
	String login(Admin admin);
	
}

```
新建BookBaseLogicService.java、BookKindLogicService.java、ReaderBaseLogicService.java、ReaderKindLogicService.java、BorrowInfoLogicService.java、SearchLogicService.java、StatisticsLogicService.java，详细代码移步https://github.com/voidking/bookmanage.git


# logic.impl包
新建包com.voidking.book.logic.impl，在该包下，新建AdminLogicServiceImpl.java，内容如下。
```
package com.voidking.book.logic.imp;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.voidking.book.entity.Admin;
import com.voidking.book.logic.AdminLogicService;
import com.voidking.book.repository.AdminRepository;
import com.voidking.book.service.AdminService;

@Service
public class AdminLogicServiceImpl implements AdminLogicService {

	@Autowired
	private AdminService adminService;
	
	@Autowired
	private AdminRepository adminRepository;

	public String login(Admin admin) {
		if (admin == null) {
			return "表现层错误";
		}
		if (admin.getName() == null || admin.getName().equals("")) {
			return "用户名错误";
		}
		if (admin.getPwd() == null || admin.getPwd().equals("")) {
			return "密码错误";
		}

//		AdminBase tmp = this.adminBaseService.findByNameAndPwd(
//				adminBase.getName(), adminBase.getPwd());
		Admin tmp = this.adminRepository.findByNameAndPwd(admin.getName(), admin.getPwd());
		

		if (tmp == null) {
			return "用户名或者密码错误";
		}

		admin.setId(tmp.getId());

		//this.adminBaseService.save(adminBase);

		return "登录成功";

	}

}

```
新建BookBaseLogicServiceImpl.java、BookKindLogicServiceImpl.java、ReaderBaseLogicServiceImpl.java、ReaderKindLogicServiceImpl.java、BorrowInfoLogicServiceImpl.java、SearchLogicServiceImpl.java、StatisticsLogicServiceImpl.java，详细代码移步https://github.com/voidking/bookmanage.git

# 配置文件
spring-persist.xml、database-conn.properties、log4j.properties，和persist层相同。

# context:component-scan范围
book-persist工程下含有com.voidking.book.entity、com.voidking.book.repository、com.voidking.book.service、com.voidking.book.service.impl四个包。book-persist工程中Spring的配置文件，有这么一句：
```
<context:component-scan base-package="com.voidking.book" />
```
这个配置，会自动扫描com.voidking.book包及其子包下的注解，@Repository、@Service、@Controller 和 @Component，并把它们注册为Spring Bean。在book-persist工程中，似乎没有什么问题。

下面注意，问题来了！！！

在book-logic工程中，含有com.voidking.book.logic、com.voidking.book.logic.impl两个包。同时，引入了book-persist打包成的jar文件，jar文件中并没有Spring的配置文件，更没有加载配置文件这一过程。而book-logic工程中Spring的配置文件，也有这么一句：
```
<context:component-scan base-package="com.voidking.book" />
```
这时，请问，这里的com.voidking.book是指在当前工程下，还是jar文件里面，又或是两者都包括？
小编猜测两者都包括，那么怎么证明？
1、由book-persist工程，可以看出，当前工程下的包，是肯定会被扫描的。
2、由book-logic工程，可以看出，它使用了jar文件中的类，而且没有“new”，而是“@Autowired”，说明jar包中的包也是会被扫描的。

这时新的问题出现了，book-persist和book-logic这两个工程，是否需要什么必然的要求？比如必须在同一个groupId下面？

答案是没有特殊要求！
证明过程如下：
1、新建任意Maven工程（以book-jpa为例）。
2、在pom.xml中，像book-logic一样，引入book-persist的jar文件。
3、拷贝book-logic的Spring配置文件到book-jpa的对应位置。
4、在src/test/java下新建任意包，包中新建任意java文件。
5、在Test方法中，加载配置文件，使用book-persist中的Bean。
最后运行结果和预期一致，答案得证！

总结：context:component-scan扫描的范围包括当前工程和引入的jar文件，而且不要求当前工程和jar文件同处于一个groupId。

# 单元测试
在src/test/java文件夹下，新建包com.voidking.book.logic，新建AdminLogicServiceTest.java文件，内容如下。
```
package com.voidking.book.logic;

import org.junit.Before;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.voidking.book.entity.Admin;

public class AdminLogicServiceTest {

	private AdminLogicService adminLogicService;
	@Before
	public void prepare(){

		ApplicationContext ctx = new ClassPathXmlApplicationContext("spring-persist.xml");

		adminLogicService = ctx.getBean(AdminLogicService.class);
	}
	
	@Test
	public void testLogin() {
		Admin admin = new Admin("haojin","haojin");
		System.err.println(this.adminLogicService.login(admin));

	}
}

```
其他服务类的测试用例，自行编写。

注：打包jar文件时，src/test/java下的配置文件和src/test/resources下的类，不会被打包。


# 后记

没有人能随随便便成功，踏踏实实，一点一滴。。。



