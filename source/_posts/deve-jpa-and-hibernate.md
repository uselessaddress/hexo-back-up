title: JPA与Hibernate的关系
date: 2014-02-17 20:32:28
tags:
- JPA
- Hibernate
categories: 设计开发
---
### 名词解释
JPA：Java Persistence API，是Java EE 5的标准ORM接口，也是ejb3规范的一部分。JPA通过JDK5.0注解或XML描述对象-关系表的映射关系，并将运行期实体对象持久化到数据库中去。

ORM：Object-Relational Mapping，对象关系映射，即实体对象和数据库表的映射。

Hibernate：是一个开放源代码的对象关系映射框架，它对JDBC进行了非常轻量级的对象封装，使得Java程序员可以随心所欲的使用对象编程思维来操纵数据库。

### JPA与Hibernate的关系
JPA和Hibernate之间的关系，可以简单的理解为JPA是标准接口，Hibernate是实现。
<!--more-->
Hibernate主要通过hibernate-annotation、hibernate-core和hibernate-entitymanager三个组件来实现与JPA的关系。

hibernate-annotation是Hibernate支持annotation方式配置的基础，它包括了标准的JPA annotation以及Hibernate自身特殊功能的annotation。

hibernate-core是Hibernate的核心实现，提供了Hibernate所有的核心功能。

hibernate-entitymanager实现了标准的JPA，可以把它看成hibernate-core和JPA之间的适配器，它并不直接提供ORM的功能，而是对hibernate-core进行封装，使得Hibernate符合JPA的规范。

### 参考文档
http://blog.163.com/hero_213/blog/static/398912142010312024809/