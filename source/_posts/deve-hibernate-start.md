title: Hibernate概述
date: 2014-02-21 10:10:24
tags:
- Hibernate
- JavaEE
categories: 设计开发
toc: true
---

### 名词解释

Hibernate是一个开放源代码的对象关系映射框架，它对JDBC进行了非常轻量级的对象封装（未完全封装），使得Java程序员可以随心所欲的使用对象编程思维来操纵数据库。 

Hibernate可以应用在任何使用JDBC的场合，既可以在Java的客户端程序使用，也可以在Servlet/JSP的Web应用中使用，最具革命意义的是，Hibernate可以在应用EJB的J2EE架构中取代CMP，完成数据持久化的重任。

对目前的JavaEE信息化系统而言，通常采用面向对象分析和面向对象设计的过程。系统从需求分析到系统设计都是按面向对象方式进行。但是到详细设计阶段，由于数据持久化需要保存到关系数据库，不得不自底向上修改设计方案，又回到了按照过程进行编程的老路上来，这是非常令人沮丧的。

但人们的智慧是无穷的，遇到问题总会想办法解决，而不是与之妥协或绕道而走。Hibernate的问世解决了这个问题，Hibernate是一个面向Java环境的对象/关系映射工具，它可将对象模型表示的对象映射到基于SQL的关系数据模型中。这样就不用再为怎样用面向对象的方法进行数据持久化而大伤脑筋了。
<!--more-->
### 官网
http://hibernate.org

### 下载
http://sourceforge.net/projects/hibernate/files/hibernate3/

### 导入包
解压缩hibernate-distribution-*.zip。导入hibernate-distribution-*GA/lib/required目录中的jar包。 
hibernate3.jar       核心类库
antlr-2.7.6.jar      代码扫描器,用来翻译HQL语句
commons-collections-3.1.jar    Apache Commons包中的一个，包含了一些Apache开发的集合类，功能比java.util.*强大 
dom4j-1.6.1.jar      一个Java的XML API，类似于jdom，用来读写XML文件的 
javassist-3.4.GA.jar    Javassist 字节码解释器
jta-1.1.jar     标准的JTA API。 
slf4j-api-1.5.2.jar 
slf4j-nop-1.5.2.jar 

### ORM
对象/关系映射ORM（Object-Relation Mapping）是用于将对象与对象之间的关系对应到数据库表与表之间的关系一种模式。

简单的说，ORM是通过使用描述对象和数据库之间映射的元数据，将Java程序中的对象自动持久化到关系数据库中。对象和关系数据是业务实现的两种表现形式，业务实体在内存中表现为对象，在数据库中表现为关系数据。内存中的对象之间存在着关联和继承关系。而在数据库中，关系数据无法直接表达多对多关联和继承关系。因此，ORM系统一般以中间件的形式存在，主要实现程序对象到关系数据库数据的映射。

一般的ORM包括四个部分：对持久类对象进行CRUD操作的API、用来规定类和类属性相关查询的语言或API、规定mapping metadata的工具，以及可以让ORM实现同事务对象一起进行dirty checking、lazy association fetching和其他优化操作的技术。

目前，很多厂商和开源社区都提供了持久层框架的实现，但其中Hibernate的轻量级ORM模型逐步确立了在Java ORM架构中的领导地位。

### Hibernate体系结构
Hibernate作为模型层/数据访问层。它通过配置文件（hibernate.cfg.xml或hibernate.properties）和映射文件（*.hbm.xml）把Java对象或持久化对象（Persistent Object，PO）映射到数据库中的数据表，然后通过操作PO，对数据库中的表进行各种操作。实际上，PO由POJO和映射文件生成。

### POJO、JavaBean和PO

POJO = pure old java object or plain ordinary java object or what ever。
POJO中文可以翻译成：普通Java类，具有一部分get/set方法的那种类就可以称作POJO。

JavaBean则比POJO复杂很多，JavaBean是一种组件技术，就好像你做了一个扳子，而这个扳子会在很多地方被拿去用，这个扳子也提供多种功能(你可以拿这个扳子扳、锤、撬等等)，而这个扳子就是一个组件。
很显然POJO也是JavaBean的一种。一般在web应用程序中建立一个数据库的映射对象时，我们只能称它为POJO。
JavaBean必须满足如下规范： 
1）必须有一个零参数的默认构造函数 
2）必须有get和set方法，类的字段必须通过get/set方法来访问。 


PO = persisent object 持久对象。
实际上，PO必须对应数据库中的entity，所以和POJO有所区别。比如说POJO是由new创建，由GC回收。但是PO是数据库insert创建，由数据库delete删除的。基本上持久对象生命周期和数据库密切相关。另外持久对象往往只能存在一个数据库Connection之中，Connnection关闭以后，持久对象就不存在了，而POJO只要不被GC回收，总是存在的。
由于存在诸多差别，因此PO在代码上肯定和POJO不同，起码PO相对于POJO会增加一些用来管理数据库entity状态的属性和方法。而ORM追求的目标就是要PO在使用上尽量和POJO一致，对于程序员来说，他们可以把PO当做POJO来用，而感觉不到PO的存在。
所以我们项目中的entity可以看作是POJO，其实Hibernate最后作持久，是把PO持久化的，主要的机制是：
1、编写POJO 
2、编译POJO 
3、直接运行，在运行期，由Hibernate的CGLIB动态把POJO转换为PO。

### 应用实例
Hibernate的配置文件是实体对象与数据库关系表之间相互转换的重要依据。一般而言，一个映射配置文件对应着数据库中一个关系表，关系表之间的关系也在映射文件中进行配置。

#### 建立数据库和表
使用MySQL，scott用户。在xscj数据库中建立kcb，表结构如下：

| 项目名 | 列名 | 数据类型 | 可空 | 默认值 | 说明 |
| :----: | :----: | :----- | :----: | :----: | :----- |
| 课程号 | KCH | 定长字符型（char3） | × | 无 | 主键 |
| 课程名 | KCM | 不定长字符串型（varchar12） | √ | 无 |  |
| 开学学期 | KXXQ | 整数型（smallint） | √ | 无 | 只能为1~8 |
| 学时 | XS | 整数型（int） | √ | 0 |  |
| 学分 | XF | 整数型（int） | √ | 0 |  |

```
create table kcb(
kch char(3) not null primary key,
kcm varchar(12),
kxxq smallint check(kxxq>=1 and kxxq<=8),
xs int default 0,
xf int default 0
);
```

#### 新建项目
新建Dynamic Web Project，命名为hibernate。

#### 添加hibernate开发能力
右击项目名，Properties，Java Build Path，Libraries，Add Library...，User Library，Next，User Libraries...，New...，输入“hibernate”，OK，Add External JARs...，选中需要的jar文件，打开，OK。


#### POJO对象和映射文件
新建包com.voidking.hibernate.model，在包中新建POJO，命名为Kcb，代码如下：
```
package com.voidking.hibernate.model;

public class Kcb {

	private String kch;
	private String kcm;
	private short kxxq;
	private Integer xs;
	private Integer xf;

	public Kcb() {

	}

	public String getKch() {
		return kch;
	}

	public void setKch(String kch) {
		this.kch = kch;
	}

	public String getKcm() {
		return kcm;
	}

	public void setKcm(String kcm) {
		this.kcm = kcm;
	}

	public short getKxxq() {
		return kxxq;
	}

	public void setKxxq(short kxxq) {
		this.kxxq = kxxq;
	}

	public Integer getXs() {
		return xs;
	}

	public void setXs(Integer xs) {
		this.xs = xs;
	}

	public Integer getXf() {
		return xf;
	}

	public void setXf(Integer xf) {
		this.xf = xf;
	}
	
	
}

```
在包中新建文件Kcb.hbm.xml，内容如下：
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-mapping PUBLIC
    "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
    "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
 
<hibernate-mapping>
<!-- name指定POJO类，table指定对应数据库的表 -->
    <class name="com.voidking.hibernate.model.Kcb" table="kcb">
    	<!-- name指定主键，type主键类型 -->
        <id name="kch" type="java.lang.String">
        	<column name="kch" length="3"></column>
        	<!-- 主键生成策略 -->
        	<generator class="assigned"></generator>
        </id>
        
        <!-- POJO属性及表中字段对应的属性 -->
        <property name="kcm" type="java.lang.String">
        	<column name="kcm" length="12"></column>
        </property>
        
        <property name="kxxq" type="java.lang.Short">
        	<column name="kxxq"></column>
        </property>
        
        <property name="xs" type="java.lang.Integer">
        	<column name="xs"></column>
        </property>
        
        <property name="xf" type="java.lang.Integer">
        	<column name="xf"></column>
        </property>
    </class>
</hibernate-mapping>

```

#### hibernate.cfg.xml
在src中新建hibernate.cfg.xml或者hibernate.properties，内容如下：
```
<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
     
<hibernate-configuration>
    <session-factory>
     
    <!-- Database connection settings -->
    <property name="connection.driver_class">com.mysql.jdbc.Driver</property>
    <property name="connection.url">jdbc:mysql://localhost/xscj</property>
    <property name="connection.username">scott</property>
    <property name="connection.password">tiger</property>
         
    <!-- JDBC connection pool (use the built-in) -->
    <!-- <property name="connection.pool_size">1</property> -->
         
    <!-- SQL dialect -->
    <property name="dialect">org.hibernate.dialect.MySQLDialect</property>
         
    <!-- Echo all executed SQL to stdout -->
    <property name="show_sql">true</property>
         
    <!-- Enable Hibernate's automatic session context management -->
    <!--<property name="current_session_context_class">thread</property>-->
         
    <!-- Drop and re-create the database schema on startup -->
    <!-- <property name="hbm2ddl.auto">create</property> -->
         
    <!-- Disable the second-level cache -->
    <property name="cache.provider_class">org.hibernate.cache.NoCacheProvider</property>
     
    <mapping resource="com/voidking/hibernate/model/Kcb.hbm.xml"/>
         
    </session-factory>
</hibernate-configuration>
```

#### 创建测试类
新建包com.voidking.hibernate.test，新建类Test，代码如下：
```
package com.voidking.hibernate.test;

import java.util.List;

import org.hibernate.Query;
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

import com.voidking.hibernate.model.Kcb;

public class Test {
	public static void main(String[] args){
		//创建Session对象
		Configuration cfg = new Configuration();
        SessionFactory sf = cfg.configure().buildSessionFactory();
		Session session = sf.openSession();
		
		//创建事务对象
		Transaction ts=session.beginTransaction();
		Kcb kc = new Kcb();
		kc.setKch("198");
		kc.setKcm("机电");
		kc.setKxxq(new Short((short) 5));
		kc.setXf(new Integer(5));
		kc.setXs(new Integer(59));
		
		//保存对象
		session.save(kc);
		ts.commit();
		Query query = session.createQuery("from Kcb where kch=198");
		List list = query.list();
		Kcb kcl=(Kcb)list.get(0);
		System.out.println(kcl.getKcm());
		session.close();
		sf.close();
		
	}
}

```

#### 导入数据库驱动
导入`mysql-connector-java-*-bin.jar`。

#### 运行错误
运行Test.java为Java Application，出现错误。
`Exception in thread "main" java.lang.NoClassDefFoundError: javax/persistence/EntityListeners`
原因是缺少`hibernate-jpa-*-api-*.Final.jar`，导入该包，问题解决。


#### 提示
运行错误解决了，但是控制台仍然有提示信息。
```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```
经过搜索，解决办法如下：
```
Placing one (and only one) of slf4j-nop.jar, slf4j-simple.jar, slf4j-log4j12.jar, slf4j-jdk14.jar or logback-classic.jar on the class path should solve the problem.
```
之前下载的hibernate压缩包里没有slf4j-nop.jar，就没有导入。单独下载该jar文件，导入，问题解决。

#### 再次运行错误
其实，报错是应该的，因为两次插入的数据完全相同，而主键要求唯一。这时，应该怎么配置，才能在不改变代码的情况下不断运行呢？
经测试，只要在hibernate.cfg.xml配置文件中，把注释中的这一句释放出来，就可以了。
```
<property name="hbm2ddl.auto">create</property>
```

### 各文件的作用

#### POJO和映射配置文件
POJO中的属性和表中的字段是一一对应的。那么通过什么方法把它们一一映射起来呢？就是*.hbm.xml映射文件。
该配置文件大致分为3个部分：
1、类、表映射配置
```
<class name="com.voidking.hibernate.model.Kcb" table="kcb">
```
2、id映射配置
```
<id name="kch" type="java.lang.String">
	<column name="kch" length="3"></column>
	<generator class="assigned"></generator>
</id>
```
Hibernate的主键生成策略分为三大类：Hibernate对主键id赋值、应用程序自身对id赋值、由数据库对id赋值。
- assigned：应用程序自身对id赋值。
- native：由数据库对id赋值。
- hilo：通过hi/lo算法实现的主键生成机制，需要额外的数据库表保存主键生成历史状态。
- seqhilo：与hi/lo类似，通过hi/lo算法实现的主键生成机制，只是主键历史状态保存在sequence中，适用于支持sequence的数据库，比如Oracle。
- increment：主键按数值顺序递增。
- identity：采用数据库提供的主键生成机制，如SQL Server、MySQL中的自增主键生成机制。
- sequence：采用数据库提供的sequence机制生成主键，如Oracle sequence。
- uuid.hex：由Hibernate基于128位唯一值产生算法，根据当前设备IP、时间、JVM启动时间、内部自增量等4个参数生成十六进制数值（编码长度为32位的字符串表示）作为主键。即使是在多实例并发运行的情况下，这种算法在最大程度上保证了产生id的唯一性。
- uuid.string：与uuid.hex类似，只是对生成的主键进行编码（长度16位）。在某些数据库中可能出现问题。
- foreign：使用外部表的字段作为主键。
- select：Hibernate3引入的主键生成机制，主要针对遗留系统的改造工程。

由于常用的数据库，如SQL Sever、MySQL等，都提供了易用的主键生成机制（如auto-increase字段）。可以在数据库提供的主键生成机制上，采用generator class="native"的主键生成方式。

3、属性、字段映射配置
```
<property name="kcm" type="java.lang.String">
	<column name="kcm" length="12"></column>
</property>
```

4、关系配置
表与表之间的关系，会被映射成类与类之间的关系，它们的关系的具体体现也会在该配置文件中配置。

#### hibernate.cfg.xml
Hibernate配置文件主要用于配置数据库连接和Hibernate运行时所需要的各种属性，配置文件一般默认为hibernate.cfg.xml，Hibernate初始化期间会自动在classpath中寻找这个文件，并读取其中的配置信息，为后期数据库操作做好准备。

#### HibernateSessionFactory
```
package com.voidking.hibernate.util;

import org.hibernate.HibernateException;
import org.hibernate.Session;
import org.hibernate.cfg.Configuration;
import org.hibernate.service.ServiceRegistry;
import org.hibernate.service.ServiceRegistryBuilder;

/**
 * Configures and provides access to Hibernate sessions, tied to the
 * current thread of execution.  Follows the Thread Local Session
 * pattern, see {@link http://hibernate.org/42.html }.
 */
public class HibernateSessionFactory {

    /** 
     * Location of hibernate.cfg.xml file.
     * Location should be on the classpath as Hibernate uses  
     * #resourceAsStream style lookup for its configuration file. 
     * The default classpath location of the hibernate config file is 
     * in the default package. Use #setConfigFile() to update 
     * the location of the configuration file for the current session.   
     */
	private static final ThreadLocal<Session> threadLocal = new ThreadLocal<Session>();
    private static org.hibernate.SessionFactory sessionFactory;
	
    private static Configuration configuration = new Configuration();
    private static ServiceRegistry serviceRegistry; 

	static {
    	try {
			configuration.configure();
			serviceRegistry = new ServiceRegistryBuilder().applySettings(configuration.getProperties()).buildServiceRegistry();
			sessionFactory = configuration.buildSessionFactory(serviceRegistry);
		} catch (Exception e) {
			System.err.println("%%%% Error Creating SessionFactory %%%%");
			e.printStackTrace();
		}
    }
    private HibernateSessionFactory() {
    }
	
	/**
     * Returns the ThreadLocal Session instance.  Lazy initialize
     * the <code>SessionFactory</code> if needed.
     *
     *  @return Session
     *  @throws HibernateException
     */
    public static Session getSession() throws HibernateException {
        Session session = (Session) threadLocal.get();

		if (session == null || !session.isOpen()) {
			if (sessionFactory == null) {
				rebuildSessionFactory();
			}
			session = (sessionFactory != null) ? sessionFactory.openSession()
					: null;
			threadLocal.set(session);
		}

        return session;
    }

	/**
     *  Rebuild hibernate session factory
     *
     */
	public static void rebuildSessionFactory() {
		try {
			configuration.configure();
			serviceRegistry = new ServiceRegistryBuilder().applySettings(configuration.getProperties()).buildServiceRegistry();
			sessionFactory = configuration.buildSessionFactory(serviceRegistry);
		} catch (Exception e) {
			System.err.println("%%%% Error Creating SessionFactory %%%%");
			e.printStackTrace();
		}
	}

	/**
     *  Close the single hibernate session instance.
     *
     *  @throws HibernateException
     */
    public static void closeSession() throws HibernateException {
        Session session = (Session) threadLocal.get();
        threadLocal.set(null);

        if (session != null) {
            session.close();
        }
    }

	/**
     *  return session factory
     *
     */
	public static org.hibernate.SessionFactory getSessionFactory() {
		return sessionFactory;
	}
	/**
     *  return hibernate configuration
     *
     */
	public static Configuration getConfiguration() {
		return configuration;
	}

}
```
上面的实例中，我并没有使用这个类，如果使用它的话，Test.java文件可以修改如下：
```
package com.voidking.hibernate.test;
import java.util.List;
import org.hibernate.Query;
import org.hibernate.Session;
import org.hibernate.Transaction;
import org.model.Kcb;
import com.voidking.hibernate.util.HibernateSessionFactory;
public class Test {
   public static void main(String[] args) {
	// 调用HibernateSessionFactory的getSession方法创建Session对象
	Session session=HibernateSessionFactory.getSession();
	// 创建事务对象
	Transaction ts=session.beginTransaction();
	Kcb kc=new Kcb();                       	// 创建POJO类对象
	kc.setKch("198");                        	// 设置课程号
	kc.setKcm("机电");                       	// 设置课程名
	kc.setKxxq(new Short((short) 5));			 	// 设置开学学期
	kc.setXf(new Integer(5));				 	// 设置学分
	kc.setXs(new Integer(59));				 	// 设置学时
	// 保存对象
	session.save(kc);
	ts.commit();                             	// 提交事务
	Query query=session.createQuery("from Kcb where kch=198");
	List list=query.list();
	Kcb kc1=(Kcb) list.get(0);
	System.out.println(kc1.getKcm());
	HibernateSessionFactory.closeSession();		 	// 关闭Session
   }
}

```
HibernateSessionFactory类是自定义的SessionFactory，名字可以根据自己的喜好来决定。在Hibernate中，Session负责完成对象持久化操作。该文件负责创建Session对象，以及关闭Session对象。从HibernateSessionFactory文件可以看出，Session对象的创建大致需要以下3个步骤：
1、初始化Hibernate配置管理类Configuration。
2、通过Configuration类实例创建Session的工厂类SessionFactory。
3、通过SessionFactory得到Session实例。


### Hibernate核心接口
Hibernate核心接口一共有5个：Configuration、SessionFactory、Session、Transaction和Query。这5个接口在任何开发中都会用到。通过这些接口，不仅可以对持久化对象进行存取，还能够进行事务控制。

#### Configuration
Configuration接口负责Hibernate的配置信息。Hibernate运行时需要一些底层实现的基本信息。这些信息包括：数据库URL、数据库用户名、数据库用户密码、数据库JDBC驱动类、数据库dialect。用于对特定数据库提供支持，其中包含了针对特定数据库特性的实现，如Hibernate数据库类型到特定数据库类型的映射等。
使用Hibernate必须首先提供这些基础信息以完成初始化工作，为后续操作做好准备。这些属性在Hibernate配置文件hibernate.cfg.xml中加以设定，当调用：
```
Configuration config = new Configuration().configure();
```
Hibernate会自动在目录下搜索hibernate.cfg.xml文件，并将其自动读取到内存中作为后续操作的基础配置。

#### SessionFactory
SessionFactory负责创建Session实例，可以通过Configuration实例构建SessionFactory。
```
Configuration config = new Configuration().configure();
SessionFactory sessionFactory = config.buildSessionFactory();
```
Configuration实例config会根据当前数据库的配置信息，构造SessionFactory实例并返回。SessionFactory一旦构造完毕，即被赋予特定的配置信息。也就是说，以后config的任何变更将不会影响到已经创建的SessionFactory实例sessionFactory。
如果应用中需要访问多个数据库，针对每个数据库，应分别对其创建对应的SessionFactory实例。
SessionFactory保存了对应当前数据库配置的所有映射关系，同时也负责维护当前的二级数据缓存和Statement Pool。由此可见，SessionFactory的创建过程非常复杂、代价高昂。由于SessionFactory采用了线程安全的设计，可由多个线程并发调用。大多数情况下，应用中针对一个数据库共享一个SessionFactory实例即可。

#### Session
Session是Hibernate持久化操作的基础，提供了众多持久化方法，如save、update、delete等。通过这些方法，透明地完成对象的增加、删除、修改、查找等操作。
同时，值得注意的是，Hibernate Session的设计是非线程安全的，即一个Session实例同时只可由一个线程使用。同一个Session实例的多线程并发调用将导致难以预知的错误。
Session实例由SessionFactory构建：
```
Configuration config = new Configuration().configure();
SessionFactory sessionFactory = config.buildSessionFactory();
Session session = sessionFactory.openSession();
```
之后可以调用Session提供的save、get、delete等方法完成持久层操作。

#### Transaction
Transaction是Hibernate中进行事务操作的接口，Transaction接口是对实际事务实现的一个抽象，这些实现包括JDBC的事务、JTA中的UserTransaction，甚至可以是CORBA事务。之所以这样设计是可以让开发者能够使用一个统一的操作界面，使得自己的项目可以在不同的环境和容器之间方便地移植。事务对象通过Session创建。
```
Transaction ts = session.beginTransaction();
```



#### Query
在Hibernate 2.X中，find()方法用于执行HQL语句。Hibernate 3.X废除了find()方法，取而代之的是Query接口，它们都用于执行HQL语句。Query和HQL是分不开的。
```
Query query = session.createQuery("from Kcb where kch=198");
```

```
Query query = session.createQuery("from Kcb where kch=?");
query.setString(0,"要设置的值");
```

```
Query query = session.createQuery("from Kcb where kch=:kchValue");
query.setString("kchValue","要设置的值");
```
由于上例中的kch为String类型，所以设置的时候用setString，如果是int类型就要用setInt。还有一种通用的设置方法，就是setParameter方法，不管什么类型的参数都可以应用。
```
query.setParameter(0,"要设置的值");
query.setParameter("kchValue","要设置的值");
```

Query还有一个list()方法，用于取得一个List集合的示例，此示例中包括可能是一个Object集合，也可能是Object数组集合。
```
Query query = session.createQuery("form Kcb where kch=198");
List list = query.list();
```

### HQL查询
HQL是Hibernate Query Language的缩写。HQL的语法很像SQL，但HQL是一种面向对象查询语言。SQL的操作对象是数据表和列等数据对象，而HQL的操作对象是类、实例、属性等。HQL的查询依赖于Query类，每个Query实例对应一个查询对象。

#### 基本查询
1、查询所有课程信息
```
Session session=HibernateSessionFactory.getSession();
Transaction ts=session.beginTransaction();

Query query=session.createQuery("from Kcb");
List list=query.list();

ts.commit(); 
HibernateSessionFactory.closeSession();		 	
```
2、查询某门课程信息
```
Session session=HibernateSessionFactory.getSession();
Transaction ts=session.beginTransaction();

Query query=session.createQuery("from Kcb order by xs desc");
query.setMaxResults(1);//设置最大检索数目为1
Kcb kc = (Kcb)query.uniqueResult();

ts.commit(); 
HibernateSessionFactory.closeSession();		 	
```
3、查询满足条件的课程信息
```
Session session=HibernateSessionFactory.getSession();
Transaction ts=session.beginTransaction();

Query query=session.createQuery("from Kcb where kch=001");
List list=query.list();

ts.commit(); 
HibernateSessionFactory.closeSession();		 	
```

#### 条件查询
1、按指定参数查询
```
Session session=HibernateSessionFactory.getSession();
Transaction ts=session.beginTransaction();

Query query=session.createQuery("from Kcb where kcm=?");
query.setParameter(0,"计算机基础");
List list=query.list();

ts.commit(); 
HibernateSessionFactory.closeSession();		 	
```

2、使用范围运算查询
```
Session session=HibernateSessionFactory.getSession();
Transaction ts=session.beginTransaction();

Query query=session.createQuery("from Kcb where (xs between 40 and 60) and kcm in('计算机基础','数据结构')");
List list=query.list();

ts.commit(); 
HibernateSessionFactory.closeSession();		 	
```

3、使用比较运算符查询
```
Session session=HibernateSessionFactory.getSession();
Transaction ts=session.beginTransaction();

Query query=session.createQuery("from Kcb where xs > 50 and kcm is not null");
List list=query.list();

ts.commit(); 
HibernateSessionFactory.closeSession();		 	
```

4、使用字符串匹配运算查询
```
Session session=HibernateSessionFactory.getSession();
Transaction ts=session.beginTransaction();

Query query=session.createQuery("from Kcb where kch like '%001%' and kcm like '计算机%'");
List list=query.list();

ts.commit(); 
HibernateSessionFactory.closeSession();		 	
```

#### 分页查询
在页面上显示数据结果时，如果数据太多，一个页面无法全部显示，这是务必要对查询结果进行分页显示。为了满足分页查询的需要，Hibernate的Query实例提供了两个有用的方法：setFirstResult(int firstResult)和setMaxResult(int maxResult)。

```
Session session=HibernateSessionFactory.getSession();
Transaction ts=session.beginTransaction();

Query query=session.createQuery("from Kcb");
int pageNow=1;
int pageSize=5;
query.setFirstResult((pageNow-1)*pageSize);
query.setMaxResult(pageSize);
List list=query.list();

ts.commit(); 
HibernateSessionFactory.closeSession();		 	
```
通常情况下，pageNow会作为一个参数传进来，这样就可以得到想要显示的页数的结果集了。


### 参考文档
《Java EE基础实用教程》，郑阿奇主编
关于对POJO PO 的理解：http://fluagen.blog.51cto.com/146595/36396/