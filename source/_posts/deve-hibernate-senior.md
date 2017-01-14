title: Hibernate高级应用
date: 2014-02-21 13:10:24
tags:
- Hibernate
- JavaEE
categories: 设计开发
toc: true
---

### Hibernate批量处理
#### 批量插入

##### Hibernate直接处理
首先在hibernate.cfg.xml中设置批量尺寸属性hibernate.jdbc.batch_size，最好关闭Hibernate的二级缓存以提高效率。
```
<hibernate-configuration>
	<session-factory>
		<property name="hibernate.jdbc.batch_size">50</property>
		<property name="hibernate.jdbc.use_second_level_cache">false</property>
	</session-factory>
</hibernate-configuration>
```
下面批量插入500个课程到数据库表中：
```
//KcbBatch.java
package com.voidking.hibernate.test;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

import com.voidking.hibernate.model.Kcb;

public class KcbBatch {
	public static void main(String[] args){
		//创建Session对象
		Configuration cfg = new Configuration();
        SessionFactory sf = cfg.configure().buildSessionFactory();
		Session session = sf.openSession();
		
		//创建事务对象
		Transaction ts=session.beginTransaction();
		
		for (int i = 0; i < 500; i++) {
			Kcb kcb = new Kcb();
			kcb.setKch(i+"");
			session.save(kcb);
			if(i%50==0)
			{
				session.flush();
				session.clear();
			}
		}
		
		ts.commit();
		
		session.close();
		sf.close();
		
	}
}


```

<!--more-->

##### 调用JDBC
由于Hibernate只是对JDBC进行了轻量级的封装，因此完全可以绕过Hibernate直接用JDBC进行批量插入。因此上面的代码可以改成：
```
//KcbBatch.java
package com.voidking.hibernate.test;

import java.sql.Connection;
import java.sql.PreparedStatement;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

import com.voidking.hibernate.model.Kcb;

public class KcbBatch2 {
	public static void main(String[] args){
		//创建Session对象
		Configuration cfg = new Configuration();
        SessionFactory sf = cfg.configure().buildSessionFactory();
		Session session = sf.openSession();
		
		//创建事务对象
		Transaction ts=session.beginTransaction();
		
		Connection conn = session.connection();
		
		try {
			PreparedStatement stmt = conn.prepareStatement("insert into kcb(kch) values(?)");
			for(int i=0;i<500;i++)
			{
				stmt.setString(1, i+"");
				stmt.addBatch();
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
		
		ts.commit();
		
		session.close();
		sf.close();
		
	}
}

```

##### 对比
在用Hibernate进行操作的时候操作的是表对应的类，而在用JDBC进行操作时操作的是数据库中的表。

#### 批量更新

##### Hibernate直接处理
为了使Hibernate的HQL直接支持update/delete的批量更新语法，首先要hibernate.cfg.xml中设置HQL/SQL查询翻译器属性hibernate.query.factory_class。
```
<property name="hibernate.query.factory_class">
	org.hibernate.hql.ast.ASTQueryTranslatorFactory
</property>
```
为了防止类名输入错误，可以按住Ctrl键，同时单击这个类名。如果可以跳转到.class文件，说明输入正确，否则输入错误。
下面使用HQL批量更新把课程表中XS修改为30。
```
Query query = session.createQuery("update kcb set xs=30");
query.executeUpdate();
```


##### 调用JDBC
```
try{
	Statement stmt = conn.createStatement();
	stmt.executeUpdate("update kcb set xs=30");
}catch(Exception){
	e.printStackTrace();
}
```

#### 批量删除

##### Hibernate直接处理
删除课程表中课程号大于200的课程。
```
Query query = session.createQuery("delete kcb where kch > 200");
query.executeUpdate();
```
##### 调用JDBC
```
try{
	Statement stmt = conn.createStatement();
	stmt.executeUpdate("delete from kcb where kch>200");
}catch(Exception){
	e.printStackTrace();
}
```

### 实体对象生命周期
实体对象，特指Hibernate O/R映射关系中的域对象（即O/R中的O）。

#### transient（瞬时态）
瞬时态，即实体对象在内存中的存在，与数据库中的记录无关。
```
Student stu = new Student();
stu.setSnumber("081101");
stu.setSname("李方方");
stu.setSage(21);
```
这里的stu对象，与数据库中的记录没有任何关联。

#### persisent（持久态）
```
Student stu = new Student();
stu.setSnumber("081101");
stu.setSname("李方方");
stu.setSage(21);

Student stu1 = new Student();
stu1.setSnumber("081102");
stu1.setSname("程明");
stu1.setSage(22);

Transaction ts = session.beginTransaction();

session.save(stu);//stu对象转换为持久态，由Hibernate纳入实体管理器；stu1仍然处于瞬时态

ts.commit();

Transaction ts2 = session.beginTransaction();
stu.setSname("李方");//虽然这个事务没有显式调用session.sava()保存stu对象，但是由于stu为持久态，将自动被固化到数据库
stu1.setSname("程明明");//stu1仍是一个普通Java对象，对数据库未产生任何影响

ts2.commit();

```
处于瞬时态的对象，可以通过Session的save()方法转换成持久状态。同样，如果一个实体对象由Hibernate加载，那么，它也处于持久状态。
```
Student stu = (Student)session.load(Student.class,net Integer(1));
```
持久对象对应着数据库中的一条记录，可以看做是数据库记录的对象化操作接口，其状态的变更将对数据库中的记录产生影响。

#### Detached（脱管状态）
处于持久化的对象，其对应的Session实例关闭之后，此对象就处于脱管状态。Session实例可以看做是持久对象的宿主，一旦此宿主失效，其从属的持久对象进入脱管状态。
```
Student stu = new Student();//stu对象为瞬时态
stu.setSnumber("081101");
stu.setSname("李方方");
stu.setSage(21);

Transaction ts = session.beginTransaction();

session.save(stu);//stu对象由Hibernate纳入管理容器，处于持久状态

ts.commit();

session.close();//stu对象为脱管状态，因为与其关联的session已经关闭


```
托管态和瞬时态有什么区别？瞬时状态的stu对象与库表中的数据缺乏对应关系，而脱管状态的stu对象，却在库表中存在相对应的记录，只不过由于脱管对象脱离Session这个数据操作平台，其状态的改变无法更新到库表中对应的记录。

有时候为了方便，将处于瞬时和脱管状态的对象统称为值对象（Value Object，VO），将处于持久态的对象称为持久对象（Persistent Object，PO）。也就是说，对Hibernate实体管理容器而言，非管理的实体对象统称为VO，被管理的对象称为PO。

### Hibernate事务管理
事务是数据库并发控制不可分割的基本工作单位，具有原子性、一致性、隔离性和持久性的特点。

事务（Transaction）是工作中的基本逻辑单元，可以用于确保数据库能够被正确修改，避免数据只修改一部分而导致数据不完整，或者在修改时受到用户干扰。

#### 基于JDBC的事务管理
Hibernate是JDBC的轻量级封装，本身并不具备事务管理能力。在事务管理层，Hibernate将其委托给底层的JDBC或JTA，以实现事务管理和调度功能。

在JDBC中，事务默认是自动提交。也就是说，一条对数据库的更新表达式代表一项事务操作。操作成功后，系统将自动调用commit提交。否则，将调用rollback回滚。

在JDBC中，可以通过调用setAutoCommit(false)禁止自动提交。之后就可以把多个数据库操作的表达式作为一个事务，在操作完成后调用commit进行整体提交。

将事务管理委托给JDBC进行处理是最简单的实现方式，Hibernate对于JDBC事务的封装也比较简单。如下面的代码：
```
//创建Session对象
Configuration cfg = new Configuration();
SessionFactory sf = cfg.configure().buildSessionFactory();
Session session = sf.openSession();

//创建事务对象
Transaction ts=session.beginTransaction();

seesion.save(stu);

ts.commit();

session.close();
sf.close();
```
从JDBC层面而言，上面的代码实际对应着：
```
Connection cn = getConnection;
cn.setAutoCommit(false);
//JDBC调用相关的SQL语句
cn.commit();
```

在sf.openSession()语句中，Hibernate会初始化数据库连接。与此同时，将其AutoCommit设为关闭状态（false）。即一开始获得的session，其自动提交属性已经被关闭。下面的代码不会对数据库产生任何效果：
```
//创建Session对象
Configuration cfg = new Configuration();
SessionFactory sf = cfg.configure().buildSessionFactory();
Session session = sf.openSession();

seesion.save(stu);

session.close();
sf.close();
```
这实际上相当于JDBC Connection的AutoCommit属性被设为false，执行了若干JDBC操作之后，没有调用commit操作。


#### 基于JTA的事务管理
JTA（Java Transaction API）是由Java EE Transaction Manager去管理的事务。其最大的特点是调用UserTransaction接口的begin、commit和rollback方法来完成事务范围的界定、事务的提交和回滚。JTA可以实现统一事务对应不同的数据库。

JTA主要用于分布式的多个数据源的两阶段提交的事务，而JDBC的Connection提供单个数据源的事务。后者因为只涉及一个数据源，所以其事务可以由数据库自己单独实现。而JTA事务因为其分布式和多数据库的特性，不可能由任何一个数据源实现事务。因此，JTA中的事务是由“事务管理器”实现的。它会在多个数据源之间统筹事务，具体使用的技术就是所谓的“两阶段提交”。

JTA提供了跨Session的事务管理能力。这一点是与JDBC Transaction最大的差异。JDBC事务由Connection管理，即事务管理实际上是在JDBC Connection中实现。事务周期限于Connection的生命周期之内。同样，对于基于JDBC Transaction的Hibernate事务管理机制而言，事务管理在Session所依托的JDBC Connection中实现，事务周期限于Session的生命周期。

JTA事务管理则由JTA容器实现。JTA对当前加入事务的众多Connection进行调度，实现事务性要求。JTA的事务周期可以横跨多个JDBC Connection生命周期。同样，对于基于JTA事务的Hibernate而言，JTA事务横跨多个Session。

#### 锁
Hibernate支持两种锁机制，悲观锁（Pessimistic Locking）和乐观锁（Optimistic Locking）。
悲观锁是指对数据被外界修改保持保守态度。假定任何时刻存取数据时，都可能有一个客户也正在存取同一数据。为了保持数据被操作的移植性，于是对数据采取了数据库层次的锁定状态，依靠数据库提供的锁机制来实现。

乐观锁则乐观地认为数据很少发生同时存取的问题，因此不做数据库层次上的锁定。为了维护正确的数据，乐观锁采用应用程序上的逻辑实现版本控制的方法。

### 源代码分享
Hibernate概述、Hibernate关系映射、Hibernate高级应用，三篇博客中使用的代码，都在下面这个工程。
https://github.com/voidking/hibernate.git

### 参考文档
《Java EE基础实用教程》，郑阿奇主编
