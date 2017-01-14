title: JPA基础
toc: true
date: 2015-07-30 22:00:12
tags:
- JavaEE
- JPA
categories: 设计开发
---
## 简介
Java持久化规范，是从EJB2.x以前的实体Bean(Entity bean)分离出来的，EJB3以后不再有实体bean，而是将实体bean放到JPA中实现。JPA是sun提出的一个对象持久化规范，各JavaEE应用服务器自主选择具体实现，JPA的设计者是Hibernate框架的作者，因此Hibernate作为Jboss服务器中JPA的默认实现，Oracle的Weblogic使用EclipseLink(以前叫TopLink)作为默认的JPA实现，IBM的Websphere和Sun的Glassfish默认使用OpenJPA(Apache的一个开源项目)作为其默认的JPA实现。

JPA的底层实现是一些流行的开源ORM(对象关系映射)框架，因此JPA其实也就是java实体对象和关系型数据库建立起映射关系，通过面向对象编程的思想操作关系型数据库的规范。

<!--more-->

## JPA的持久化策略文件
根据JPA规范要求，在实体bean应用中，需要在应用类路径(classpath)的META-INF目录下加入持久化配置文件——persistence.xml，该文件就是持久化策略文件。其内容如下：
```
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="1.0" xmlns="http://java.sun.com/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://java.sun.com/xml/ns/persistence http://java.sun.com/xml/ns/persistence/persistence_1_0.xsd">
	<persistence-unit name="持久化单元名" transaction-type="JTA">
		<jta-data-source>使用JCA部署在javaEE服务器上的数据源JNDI</jta-data-source>
        <!--指定JPA实现提供者，不指定就采用JavaEE服务器默认的JPA提供者，这里以OpenJPA为例-->	<provider>org.apache.openjpa.persistence.PersistenceProviderImpl</provider>
		<class>实体bean全路径名</class>
		……
		<properties>
            <!--配置JPA实现提供者的一些信息-->
		</properties>
	</persistence-unit>
</persistence>
```
注意：可以在持久化策略文件中配置多个持久化单元persistence-unit，在使用分布式数据库时经常配置多个持久化单元。

## 实体Bean的开发：
JPA规范中定义的实体bean开发的基本规范：
(1)在实体bean上添加”@Entity”注解，标识该bean为实体bean。
(2)实体bean必须序列化。
(3)使用”@Table(name=数据库表名)”注解，标识该实体bean映射到关系型数据库中的表名，如果不指定则JPA实现会自动生成默认的表名称。
(4)必须有一个使用”@Id”注解标识的主键，如指定自增主键的写法为：
```
@Id
@Column(name=”列名”)
@GeneratedValue(Strategy=GenerationType.Auto)
private int id;
```
注意：@Id和@GeneratedValue两个注解必须同时使用，标识了注解之后，要主键的生成策略。
(5)Bean的其他属性使用”@Column(name=列名)”注解，指定该属性映射到数据库表中的列名，如果不指定在JPA实现会自动生成默认的列名。
(6)实体Bean必须严格遵循JavaBean的规范，提供无参的默认构造方法，属性提供set和get方法。
(7)最好重写hashcode()和equals()方法，实体bean的唯一标识是主键，因此，使用实体bean的主键来比较。


## 实体bean的主键生成策略
实体bean的注解生成策略使用”@GeneratedValue”注解来指定主键的生成策略，默认使用Auto自增的策略，JPA不支持Hibernate的UUID主键生成策略，若要使用Hibernate的UUID做主键方式，方法如下：
(1)将Hibernate.annotation jar包加入到类路径下。
(2)在使用”@Id”的字段上同时加入如下的注解：
```
@GeneratedValue(generator=”UUID主键生成策略”)
@GenericGenerator(name=”UUID主键生成策略”, strategy=”uuid”)
```

## JPA开发的注意事项
JPA支持xml方式和注解方式，目前随着注解方式越来越成熟和流行，开发JPA时候，基本都只用注解方式，因此必须在JavaEE5以上版本才行，因为以前的版本没有对注解的支持。
和xml文件相比，java注解的优缺点：
优势：
(1)描述符减少，大大提高开发效率。
(2)编译期校验，错误的注解在编译期间就会报错。
(3)注解在java代码中，从而避免了额外的文件维护工作。
(4)注解被编译成java字节码，消耗的内存小，读取速度快，往往比xml配置文件解析快几个数量级，利用测试和维护。
缺点：
(1)配置信息分散，不利于集中维护管理。
(2)改动时涉及到了程序源代码，需要找到类的源代码才可以，而且必须通过编译这一步。相比较之下xml文件可能不需要找到类源码，同时也不需要重新编译。

## JPA注解的使用注意事项
JPA的注解支持字段注解，也支持属性注解。
字段注解：直接在变量上加注解。
属性注解：在set/get方法上加注解。
注意：不管使用哪种注解方式，在一个实体类中，不能混用两种注解方式。

## JPA其他的常用注解
除了@Entity、@Id、@Table、@Column、@GeneratedValue等注解以外，JPA还有以下常用的注解：
(1)@Temporal：主要用于标注时间类型，在javax.persistence.TemporalType枚举中定义了3种时间类型：
a.DATE：等同于java.sql.Date；
b.TIME：等同于java.sql.Time；
c.TIMESTAMP：等同于java.sql.Timestamp；
(2)@Transient：实体bean中默认所有的字段都会被映射到数据库中，如果某个属性不想被映射到数据库中，则需要对其加该注解。
(3)@Lob：将属性持久化为Blob或者Clob类型，为大数据类型。
(4)@Basic：一般可以用来控制是否进行延迟加载，用法示例：
延迟加载：@Basic(fetch=FetchType.Lazy)
非延迟加载：@Basic(fetch=FetchType.EAGER)
(5)@NamedQueryies和@NamedQuery：在实体Bean上定义命名查询。
名称查询类似于jdbc中的PrepareStatement，是在数据库中预编译的查询，可以大大提高查询效率，用法如下：
```
@NamedQueries({
       @NamedQuery(name=”命名查询名字”,query=JPQL查询语句),
       ……
})
```
(6)@OneToOne：
一对一映射注解，双向的一对一关系需要在关系维护端(owner side)的@OneToOne注解中添加mappedBy属性，建表时在关系被维护端(inverse side)建立外键指向关系维护端的主键列。
用法：@OneToOne(optional=true,casecade=CasecadeType.ALL,mappedBy=”被维护端外键”)
(7)@OneToMany：
一对多映射注解，双向一对多关系中，一端是关系维护端(owner side)，只能在一端添加mapped属性。多端是关系被维护端(inverse side)。建表时在关系被维护端(多端)建立外键指向关系维护端(一端)的主键列。
用法：@OneToMany(mappedBy = "维护端(一端)主键", cascade=CascadeType.ALL)
注意：在Hibernate中有个术语叫做维护关系反转，即由对方维护关联关系，使用inverse=false来表示关系维护放，在JPA的注解中，mappedBy就相当于inverse=false，即由mappedBy来维护关系。
(8)@ManyToOne：
多对一映射注解，在双向的一对多关系中，一端一方使用@OneToMany注解，多端的一方使用@ManyToOne注解。多对一注解用法很简单，它不用维护关系。
用法：@ManyToOne(optional = false, fetch = FetchType.EAGER)
(9)@ManyToMany：
多对多映射，采取中间表连接的映射策略，建立的中间关系表分别引入两边的主键作为外键，形成两个多对一关系。双向的多对多关系中，在关系维护端(owner side)的@ManyToMany注解中添加mappedBy属性，另一方是关系的被维护端(inverse side)，关系的被维护端不能加mappedBy属性，建表时，根据两个多端的主键生成一个中间表，中间表的外键是两个多端的主键。
用法：关系维护端——>@ManyToMany(mappedBy=”另一方的关系引用属性”)
关系被维护端——>@ManyToMany(cascade=CascadeType.ALL ,fetch = FetchType.Lazy)

## JPA的一对一关联映射
在JPA中两个实体之间是一一对应的关系称为一对一关联关系映射，如人和身份证号关系。
(1)一对一单向关联映射：
只能从映射端查找到随关联的一方，而不能反向查找。
在关联映射方关联属性上添加@OneToOne注解。
(2)一对一双向关联映射：
可以双向查找关联关系的实体。
关系维护端：在关联属性或字段上添加@OneToOne注解，同时制定@OneToOne注解的mappedBy属性。
关系被维护端：在关联属性或字段上添加@OneToOne注解。

## 一对一关联映射两种策略
(1)一对一主键关联：
一对一关联映射中，主键关联策略不会在两个关联实体对应的数据库表中添加外键字段，两个实体的表公用同一个主键(主键相同)，其中一个实体的主键既是主键又是外键。
主键关联映射：在实体关联属性或方法上添加@OneToOne注解的同时添加@PrimaryKeyJoinColumn注解(在一对一注解关联映射的任意一端实体添加即可)。
(2)一对一唯一外键关联：
一对一关联关系映射中，唯一外键关联策略会在其中一个实体对应数据库表中添加外键字段指向另一个实体表的主键，也是一对一映射关系中最常用的映射策略。
唯一外键关联：在关联属性或字段上添加@OneToOne注解的同时添加@JoinColumn(name=”数据表列名”，unique=true)注解。

## 一对多关联映射
在JPA中两个实体之间是一对多关系的称为一对多关联关系映射，如班级和学生关系。
(1)一对多单向关联映射：
在一对多单向关联映射中，JPA会在数据库中自动生成公有的中间表记录关联关系的情况。
在一端关联集合属性或字段上添加@OneToMany注解即可。
(2)一对多双向关联映射：
在一对多双向关联映射中，JPA不会在数据库中生成公有中间表。
在一端关联集合属性或字段上添加@OneToMany注解，同时指定其mappedBy属性。
在多端关联属性或字段上添加@ManyToOne注解。
注意：一对多关系映射中，mappedBy只能添加在OneToMany注解中，即在多端生成外键。

## 多对多关联映射
在JPA中两个实体之间是多对多关系的称为多对多关联关系映射，如学生和教师关系。
(1)多对多单向映射：
在其中任意实体一方关联属性或字段上添加@ManyToMany注解。
(2)多对多双向映射：
关系维护端关联属性或字段上添加@ManyToMany注解，同时指定该注解的mappedBy属性。
关系被维护端关联属性或字段上添加@ManyToMany注解。

## JPA中实体继承映射策略
在JPA中，实体继承关系的映射策略共有三种：单表继承策略、Joined策略和Table_PER_Class策略。
(1)单表继承策略：
单表继承策略，父类实体和子类实体共用一张数据库表，在表中通过一列辨别字段来区别不同类别的实体。具体做法如下：
a.在父类实体的@Entity注解下添加如下的注解：
```
@Inheritance(Strategy=InheritanceType.SINGLE_TABLE)
@DiscriminatorColumn(name=”辨别字段列名”)
@DiscriminatorValue(父类实体辨别字段列值)
```
b.在子类实体的@Entity注解下添加如下的注解：
```
@DiscriminatorValue(子类实体辨别字段列值)
```
(2)Joined策略：
Joined策略，父类实体和子类实体分别对应数据库中不同的表，子类实体的表中只存在其扩展的特殊属性，父类的公共属性保存在父类实体映射表中。具体做法：
只需在父类实体的@Entity注解下添加如下注解：
```
@Inheritance(Strategy=InheritanceType.JOINED)
```
子类实体不需要特殊说明。
(3)Table_PER_Class策略：
Table_PER_Class策略，父类实体和子类实体每个类分别对应一张数据库中的表，子类表中保存所有属性，包括从父类实体中继承的属性。具体做法：
只需在父类实体的@Entity注解下添加如下注解：
```
@Inheritance(Strategy=InheritanceType.TABLE_PER_CLASS)
```
子类实体不需要特殊说明。



## 参考文档

JPA学习笔记1——JPA基础
http://blog.csdn.net/chjttony/article/details/6086298

JPA学习笔记2——JPA高级
http://blog.csdn.net/chjttony/article/details/6086305

JPA学习笔记
http://www.blogjava.net/luyongfa/archive/2012/11/01/390572.html



