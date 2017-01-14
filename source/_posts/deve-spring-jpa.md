title: "Spring JPA"
toc: true
date: 2015-07-31 22:00:12
tags:
- JavaEE
- JPA
- Spring
categories: 设计开发
---
# JPA例子（h2database版）
这个例子高仿照搬Spring官网给出的例子，只是包不同而已。

## 新建工程
Eclipse，File，New，Maven Project，Create a simple project打上勾，Next。

Group Id输入反向域名加上工程名，Artifact Id输入子工程名，Packaging处选择jar（意思是普通工程；如果是web工程，选择war；如果用来定义父工程，选择pom）。
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/jpa/id.jpg)

<!--more-->

## pom.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.voidking.book</groupId>
	<artifactId>book-jpa</artifactId>
	<version>0.0.1-SNAPSHOT</version>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.2.5.RELEASE</version>
	</parent>

	<properties>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>
		<dependency>
			<groupId>com.h2database</groupId>
			<artifactId>h2</artifactId>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

	<repositories>
		<repository>
			<id>spring-releases</id>
			<name>Spring Releases</name>
			<url>https://repo.spring.io/libs-release</url>
		</repository>
		<repository>
			<id>org.jboss.repository.releases</id>
			<name>JBoss Maven Release Repository</name>
			<url>https://repository.jboss.org/nexus/content/repositories/releases</url>
		</repository>
	</repositories>

	<pluginRepositories>
		<pluginRepository>
			<id>spring-releases</id>
			<name>Spring Releases</name>
			<url>https://repo.spring.io/libs-release</url>
		</pluginRepository>
	</pluginRepositories>

</project>
```

## Customer.java
`src/main/java/com/voidking/book/entity/Customer.java`

```
package com.voidking.book.entity;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class Customer {

    @Id
    @GeneratedValue(strategy=GenerationType.AUTO)
    private long id;
    private String firstName;
    private String lastName;

    protected Customer() {}

    public Customer(String firstName, String lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }

    @Override
    public String toString() {
        return String.format(
                "Customer[id=%d, firstName='%s', lastName='%s']",
                id, firstName, lastName);
    }

}
```

## CustomerRepository.java
`src/main/java/com/voidking/book/repository/Customer.java`

```
package com.voidking.book.repository;
import java.util.List;

import org.springframework.data.repository.CrudRepository;

import com.voidking.book.entity.Customer;

public interface CustomerRepository extends CrudRepository<Customer, Long> {

    List<Customer> findByLastName(String lastName);
}
```
Repository（资源库）：通过用来访问领域对象的一个类似集合的接口，在领域与数据映射层之间进行协调。这个叫法就类似于我们通常所说的DAO，在这里，我们就按照这一习惯把数据访问层叫Repository Spring Data，基础的Repository提供了最基本的数据访问功能，其几个子接口则扩展了一些功能。它们的继承关系如下： 
Repository：仅仅是一个标识，表明任何继承它的均为仓库接口类，方便Spring自动扫描识别 
CrudRepository： 继承Repository，实现了一组CRUD相关的方法 
PagingAndSortingRepository： 继承CrudRepository，实现了一组分页排序相关的方法 
JpaRepository： 继承PagingAndSortingRepository，实现一组JPA规范相关的方法 
JpaSpecificationExecutor： 比较特殊，不属于Repository体系，实现一组JPA Criteria查询相关的方法 
我们自己定义的XxxxRepository需要继承JpaRepository，这样我们的XxxxRepository接口就具备了通用的数据访问控制层的能力。 

## Application.java
`src/main/java/com/voidking/book/app/Application.java`

```
package com.voidking.book.app;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import com.voidking.book.entity.Customer;
import com.voidking.book.repository.CustomerRepository;

@SpringBootApplication
public class Application implements CommandLineRunner {

    @Autowired
    CustomerRepository repository;

    public static void main(String[] args) {
        SpringApplication.run(Application.class);
    }
    
    @Override
    public void run(String... strings) throws Exception {
        // save a couple of customers
        repository.save(new Customer("Jack", "Bauer"));
        repository.save(new Customer("Chloe", "O'Brian"));
        repository.save(new Customer("Kim", "Bauer"));
        repository.save(new Customer("David", "Palmer"));
        repository.save(new Customer("Michelle", "Dessler"));

        // fetch all customers
        System.out.println("Customers found with findAll():");
        System.out.println("-------------------------------");
        for (Customer customer : repository.findAll()) {
            System.out.println(customer);
        }
        System.out.println();

        // fetch an individual customer by ID
        Customer customer = repository.findOne(1L);
        System.out.println("Customer found with findOne(1L):");
        System.out.println("--------------------------------");
        System.out.println(customer);
        System.out.println();

        // fetch customers by last name
        System.out.println("Customer found with findByLastName('Bauer'):");
        System.out.println("--------------------------------------------");
        for (Customer bauer : repository.findByLastName("Bauer")) {
            System.out.println(bauer);
        }
    }

}
```


## mvn test异常
执行命令`mvn test`，抛出异常：

```
Exception in thread "main" java.lang.UnsupportedClassVersionError:
com/voidking/book/app/Application : Unsupported major.minor version 52.0
```
这个异常，是因为本机没有安装jdk8，我就把Properties->java build path->Libraries中的JVM8换成JVM7。

解决方法：
右击项目，Properties，Java Compiler，Compiler compliance level从1.8改成1.7，之后就可以运行了。

## 运行异常
`mvn spring-boot:run`，异常：

```
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'application': Injection of autowired dependencies failed; 
nested exception is org.springframework.beans.factory.BeanCreationException: Could not autowire field: com.voidking.book.repository.CustomerRepository com.voidking.book.app.Application.repository; 
nested exception is org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type [com.voidking.book.repository.CustomerRepository] found for dependency: expected at least 1 bean which qualifies as autowire candidate for this dependency. Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}
```
可以看到，异常层层连锁，看起来那么长，实际上重点在后面。
原来是缺少CustomerRepository类型的bean，那么，为什么缺少呢？是否是因为需要一个类似spring.xml的配置文件？

## Spring官方代码
看看Spring官方给出的例子，确实没有配置文件。`mvn spring-boot:run`，结果如下：
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/jpa/success.jpg)
运行成功，可见，这个例子是没有任何问题的。那么，问题出在哪里？因为分了不同的包？

试一试，把所有.java移动到一个包中，运行。。。成功！


## 醉了
从中午12点忙到晚上12点，此问题没有解决，我也是醉了。。。

## spring-boot
第二天，继续查找资料，终于明白了，这一切都是spring-boot搞的鬼。

spring-boot会自动配置通过jpa进行数据访问需要的bean。

spring-boot会根据classpath包含的内容自动推测用户的需求并自动配置。例如如果在classpath包含了hsqldb，并且用户未配置数据库连接，spring-boot将会配置一个hsqldb内存数据库和数据源。

spring-boot无代码生成，所有的配置可通过代码完成（spring 的javaconfig），不需要使用xml（虽然可以使用）。 

# JPA例子（hsql版）
h2database和hsql非常类似，适合作为嵌入式数据库使用，其它的数据库大部分都需要安装独立的客户端和服务器端。顺便比较一下各个数据库，如下图：
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/jpa/h2.jpg)



name属性用于定义持久化单元的名字(name必选,空值也合法);transaction-type指定事务类型(可选)  

# JPA例子（Mysql版）


# 源代码分享
Spring官方例子
`git clone https://github.com/spring-guides/gs-accessing-data-jpa.git`


# 后记
使用Spring，和单纯的JPA有什么区别？

- 多了一些标记为Bean的注解，比如@Repository、@Service、@Controller和@Component。

- JAP有持久化配置文件persistence.xml（src/META-INF），默认加载。在这个文件中，可以配置数据库连接信息，实体类等；
使用了Spring，除了persistence.xml，还多出了一个或多个Spring的配置文件，配置文件间可以通过`<import>`标签引用；
persistence.xml中的内容，可以改写到spring的配置文件中，如果内容全部改写，此时persistence.xml文件可以省略。


# 参考文档
Accessing Data with JPA
http://spring.io/guides/gs/accessing-data-jpa/

Spring Data JPA - Reference Documentation
http://docs.spring.io/spring-data/data-jpa/docs/current/reference/html/

使用 Spring Data JPA 简化 JPA 开发
http://www.ibm.com/developerworks/cn/opensource/os-cn-spring-jpa/

简单的Spring JPA实现例子
http://blog.csdn.net/kongxx/article/details/5653370

spring mvc 的jpa JpaRepository数据层 访问方式汇总
http://jishiweili.iteye.com/blog/2088265

使用spring-boot快速开发spring应用
http://itindex.net/detail/49108-spring-boot-%E5%BC%80%E5%8F%91

JPA 不在 persistence.xml 文件中配置每个Entity实体类的2种解决办法
http://www.cnblogs.com/taven/archive/2013/10/04/3351841.html

Jpa规范中persistence.xml 配置文件解析
http://www.cnblogs.com/edwardlauxh/archive/2012/08/10/2632292.html


