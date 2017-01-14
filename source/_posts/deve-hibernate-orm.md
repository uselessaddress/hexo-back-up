title: Hibernate关系映射
date: 2014-02-21 12:10:24
tags:
- Hibernate
- JavaEE
categories: 设计开发
toc: true
---

### 前言
Hibernate关系映射的主要任务是实现数据库关系表与持久化类之间的映射。


### 一对一关联（共享主键方式）
在注册某个论坛会员的时候，往往不但要填写登录账户和密码，还要填写其他的详细信息，这两部分信息通常会放在不同的表中。
共享主键方式就是限制两个数据表的主键使用相同的值，通过主键形成一对一映射关系。

#### 建立表命令

建立login表和detail表：
```
create table login(
id int4 primary key not null,
username varchar(20) not null,
password varchar(20) not null
);

create table detail(
id int4 primary key auto_increment not null,
truename varchar(8),
email varchar(50)
);

```

登陆表和详细信息表属于典型的一对一关联关系，可按共享主键方式进行。
在Hibernate概述中实例的基础上，接着开发。
<!--more-->
#### Login.java
在包com.voidking.hibernate.model中，新建类Login，代码如下：
```
package com.voidking.hibernate.model;

public class Login implements java.io.Serializable {

	// Fields

	private Integer id;
	private String username;
	private String password;
	private Detail detail;		

	// Constructors

	/** default constructor */
	public Login() {
	}

	/** full constructor */
	public Login(String username, String password, Detail detail) {
		this.username = username;
		this.password = password;
		this.detail = detail;	
	}

	// Property accessors

	public Integer getId() {
		return this.id;
	}

	public void setId(Integer id) {
		this.id = id;
	}

	public String getUsername() {
		return this.username;
	}

	public void setUsername(String username) {
		this.username = username;
	}

	public String getPassword() {
		return this.password;
	}

	public void setPassword(String password) {
		this.password = password;
	}
	
	public Detail getDetail(){
		return this.detail;
	}
	public void setDetail(Detail detail){
		this.detail = detail;
	}

}
```
#### Login.hbm.xml

```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">

<hibernate-mapping>
    <class name="com.voidking.hibernate.model.Login" table="login">
        <id name="id" type="java.lang.Integer">
            <column name="ID" />
            <generator class="foreign">
            	<param name="property">detail</param>
            </generator>
        </id>
        <property name="username" type="java.lang.String">
            <column name="USERNAME" length="20" not-null="true" />
        </property>
        <property name="password" type="java.lang.String">
            <column name="PASSWORD" length="20" not-null="true" />
        </property>
		<!--constrained="true"表明当前的主键上存在一个外键约束-->
        <one-to-one name="detail" class="com.voidking.hibernate.model.Detail" constrained="true"></one-to-one>
    </class>
</hibernate-mapping>


```

#### Detail.java

```
package com.voidking.hibernate.model;

public class Detail implements java.io.Serializable {

	// Fields

	private Integer id;
	private String truename;
	private String email;
	private Login login;		

	// Constructors

	/** default constructor */
	public Detail() {
	}

	/** full constructor */
	public Detail(String truename, String email, Login login) {
		this.truename = truename;
		this.email = email;
		this.login = login;		
	}

	// Property accessors

	public Integer getId() {
		return this.id;
	}

	public void setId(Integer id) {
		this.id = id;
	}

	public String getTruename() {
		return this.truename;
	}

	public void setTruename(String truename) {
		this.truename = truename;
	}

	public String getEmail() {
		return this.email;
	}

	public void setEmail(String email) {
		this.email = email;
	}
	
	public Login getLogin(){
		return this.login;
	}
	public void setLogin(Login login){
		this.login = login;
	}

}
```

#### Detail.hbm.xml

```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">

<hibernate-mapping>
    <class name="com.voidking.hibernate.model.Detail" table="detail">
        <id name="id" type="java.lang.Integer">
            <column name="ID" />
            <generator class="identity" />
        </id>
        <property name="truename" type="java.lang.String">
            <column name="TRUENAME" length="8" />
        </property>
        <property name="email" type="java.lang.String">
            <column name="EMAIL" length="50" />
        </property>
		<!--cascade=all，表示主控类的所有操作，对关联类也执行同样操作-->
        <one-to-one name="login" class="com.voidking.hibernate.model.Login" cascade="all" lazy="false"></one-to-one>
    </class>
</hibernate-mapping>


```

#### hibernate.cfg.xml

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
    <property name="hbm2ddl.auto">create</property>
         
    <!-- Disable the second-level cache -->
    <property name="cache.provider_class">org.hibernate.cache.NoCacheProvider</property>
     
    <mapping resource="com/voidking/hibernate/model/Kcb.hbm.xml"/>
    <mapping resource="com/voidking/hibernate/model/Login.hbm.xml"/>
    <mapping resource="com/voidking/hibernate/model/Detail.hbm.xml"/>   
    </session-factory>
</hibernate-configuration>
```

#### SharePrimaryKey.java
```
package com.voidking.hibernate.test;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

import com.voidking.hibernate.model.Detail;
import com.voidking.hibernate.model.Login;

public class SharePrimaryKey{
	public static void main(String[] args) {
		Configuration cfg = new Configuration();
        SessionFactory sf = cfg.configure().buildSessionFactory();
		Session session = sf.openSession();
		
		// 创建事务对象
		Transaction ts=session.beginTransaction();
		//共享主键方式
		Detail detail=new Detail();
		Login login=new Login();
		login.setUsername("yanhong");
		login.setPassword("123");
		detail.setTruename("严红");
		detail.setEmail("yanhong@126.com");
		//相互设置关联
		login.setDetail(detail);
		detail.setLogin(login);
		//这样完成后就可以通过Session对象调用session.save(detail)来持久化该对象
		session.save(detail);
		
		ts.commit();
		
		session.close();
		sf.close();
	}
}

```

#### 运行
以Java Application运行，查看数据库，可以发现，数据成功插入。

### 唯一外键方式

唯一外键方式就是一个表的外键和另一个表的唯一主键形成一对一的映射关系，这种一对一的关系其实就是多对一关联关系的一种特殊情况而已。

唯一外键的情况很多，比如，每个人对应一个房间。很多情况下，可以是几个人住在同一个房间里面，就是多对一的关系。但是如果让一个人住一个房间，就变成了一对一的关系了，这就是前面说的一对一关系其实是多对一关系的一种特殊情况。



#### 创建表命令
创建表person和room：
```
create table person(
id int primary key auto_increment not null ,
name varchar(20) not null,
room_id int(4) 
);

create table room(
id int(4) primary key auto_increment not null,
address varchar(100) not null
);

```

#### Person.java
```
package com.voidking.hibernate.model;

public class Person implements java.io.Serializable {

	// Fields

	private Integer id;
	private String name;
	private Room room;			

	// Constructors

	/** default constructor */
	public Person() {
	}

	/** minimal constructor */
	public Person(String name) {
		this.name = name;
	}

	/** full constructor */
	public Person(String name, Room room) {
		this.name = name;
		//this.roomId = roomId;
		this.room = room;		
	}

	// Property accessors

	public Integer getId() {
		return this.id;
	}

	public void setId(Integer id) {
		this.id = id;
	}

	public String getName() {
		return this.name;
	}

	public void setName(String name) {
		this.name = name;
	}
	
	public Room getRoom(){
		return this.room;
	}
	public void setRoom(Room room){
		this.room = room;
	}
}
```

#### Person.hbm.xml
```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">

<hibernate-mapping>
    <class name="com.voidking.hibernate.model.Person" table="Person">
        <id name="id" type="java.lang.Integer">
            <column name="id" />
            <generator class="native" />
        </id>
        <property name="name" type="java.lang.String">
            <column name="name" length="20" not-null="true" />
        </property>
        <many-to-one name="room" class="com.voidking.hibernate.model.Room"  column="room_id" cascade="all" unique="true"></many-to-one>
    </class>
</hibernate-mapping>

```

#### Room.java
```
package com.voidking.hibernate.model;


public class Room implements java.io.Serializable {

	// Fields

	private Integer id;
	private String address;
	private Person person;

	// Constructors

	/** default constructor */
	public Room() {
	}

	/** full constructor */
	public Room(String address) {
		this.address = address;
		//this.person = person;
	}

	// Property accessors

	public Integer getId() {
		return this.id;
	}

	public void setId(Integer id) {
		this.id = id;
	}

	public String getAddress() {
		return this.address;
	}

	public void setAddress(String address) {
		this.address = address;
	}
	
	public Person getPerson(){
		return this.person;
	}
	public void setPerson(Person person){
		this.person = person;
	}

}
```

#### Room.hbm.xml

```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">

<hibernate-mapping>
    <class name="com.voidking.hibernate.model.Room" table="Room">
        <id name="id" type="java.lang.Integer">
            <column name="id" />
            <generator class="native" />
        </id>
        <property name="address" type="java.lang.String">
            <column name="address" length="100" not-null="true" />
        </property>

        <one-to-one name="person" class="com.voidking.hibernate.model.Person" property-ref="room"/>

    </class>
</hibernate-mapping>

```

#### hibernate.cfg.xml

```
<mapping resource="com/voidking/hibernate/model/Person.hbm.xml"/> 
<mapping resource="com/voidking/hibernate/model/Room.hbm.xml"/>  
```

##### OneForeign.java
```
package com.voidking.hibernate.test;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

import com.voidking.hibernate.model.Person;
import com.voidking.hibernate.model.Room;

public class OneForeign{
	public static void main(String[] args) {
		Configuration cfg = new Configuration();
        SessionFactory sf = cfg.configure().buildSessionFactory();
		Session session = sf.openSession();
		
		// 创建事务对象
		Transaction ts=session.beginTransaction();

		//唯一外键方式
		Person person=new Person();
		person.setName("liumin");
		Room room=new Room();
		room.setAddress("NJ-S1-328");
		person.setRoom(room);
		session.save(person);
		
		ts.commit();
		
		session.close();
		sf.close();
	}
}

```

#### 运行
以Java Application运行，查看数据库，可以发现，数据成功插入。

#### 关于外键
在创建表person时，我们并没有设置room_id为外键，但是，当我们运行这个项目的之后。使用`desc person;`命令可以发现，room_id变成了外键。
可见，Hibernate不仅可以改变数据库中表的数据，也可以改变数据库中表的属性。理论上，也可以创建删除表。
曾经开发时，使用JPA直接生成表，根本不用操作数据库，很方便。


### 多对一单向关联
只要把上例中的一对一的唯一外键关联实例稍微修改，就可以变成多对一。
#### Person.hbm.xml
```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">

<hibernate-mapping>
    <class name="com.voidking.hibernate.model.Person" table="Person">
        <id name="id" type="java.lang.Integer">
            <column name="id" />
            <generator class="native" />
        </id>
        <property name="name" type="java.lang.String">
            <column name="name" length="20" not-null="true" />
        </property>
        <many-to-one name="room" class="com.voidking.hibernate.model.Room"  column="room_id" cascade="all"></many-to-one>
    </class>
</hibernate-mapping>

```

#### Room.java
```
package com.voidking.hibernate.model;

public class Room implements java.io.Serializable {

	private Integer id;
	private String address;
	//setter和getter方法

}
```

#### Room.hbm.xml
```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">

<hibernate-mapping>
    <class name="com.voidking.hibernate.model.Room" table="Room">
        <id name="id" type="java.lang.Integer">
            <column name="id" />
            <generator class="native" />
        </id>
        <property name="address" type="java.lang.String">
            <column name="address" length="100" not-null="true" />
        </property>

    </class>
</hibernate-mapping>

```

#### ManyToOne.java
```
package com.voidking.hibernate.test;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

import com.voidking.hibernate.model.Person;
import com.voidking.hibernate.model.Room;

public class ManyToOne{
	public static void main(String[] args) {
		Configuration cfg = new Configuration();
        SessionFactory sf = cfg.configure().buildSessionFactory();
		Session session = sf.openSession();
		
		// 创建事务对象
		Transaction ts=session.beginTransaction();

		Room room=new Room();
		room.setAddress("NJ-S1-328");
		Person person=new Person();
		person.setName("liumin");
		
		person.setRoom(room);
		session.save(person);
		
		ts.commit();
		
		session.close();
		sf.close();
	}
}

```

### 一对多双向关联
上面的例子中，多对一单向关联是从多控制一，也就是说，从多可以知道一，但从一不知道多。如果一也只知道多，那么就变成了双向一对多（或多对一）关联。

#### Room.java
```
package com.voidking.hibernate.model;

import java.util.HashSet;
import java.util.Set;


public class Room implements java.io.Serializable {

	// Fields
	private Integer id;
	private String address;	
	private Set person = new HashSet();

	// Constructors

	/** default constructor */
	public Room() {
	}

	/** full constructor */
	public Room(String address) {
		this.address = address;
		//this.person = person;
	}

	// Property accessors

	public Integer getId() {
		return this.id;
	}

	public void setId(Integer id) {
		this.id = id;
	}

	public String getAddress() {
		return this.address;
	}

	public void setAddress(String address) {
		this.address = address;
	}

	public Set getPerson() {
		return person;
	}

	public void setPerson(Set person) {
		this.person = person;
	}
}
```

#### Room.hbm.xml
```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">

<hibernate-mapping>
    <class name="com.voidking.hibernate.model.Room" table="Room">
        <id name="id" type="java.lang.Integer">
            <column name="id" />
            <generator class="native" />
        </id>
        <property name="address" type="java.lang.String">
            <column name="address" length="100" not-null="true" />
        </property>
		<set name="person" inverse="false" cascade="all">
			<key column="room_id"></key>
			<one-to-many class="com.voidking.hibernate.model.Person"/>
		</set>
    </class>
</hibernate-mapping>

```

#### cascade取值
- all：表示所有操作在关联层级上进行连锁操作。
- save-update：表示只有save和update操作进行连锁操作。
- delete：表示只有delete操作进行连锁操作。
- all-delete-orphan：在删除当前持久化对象时，它相当于delete；在保存或更新当前持久化对象时，它相当于save-update。另外它还可以删除与当前持久化对象断开关联关系的其他持久化对象。

#### OneToMany.java
```
package com.voidking.hibernate.test;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

import com.voidking.hibernate.model.Person;
import com.voidking.hibernate.model.Room;

public class OneToMany{
	public static void main(String[] args) {
		Configuration cfg = new Configuration();
        SessionFactory sf = cfg.configure().buildSessionFactory();
		Session session = sf.openSession();
		
		// 创建事务对象
		Transaction ts=session.beginTransaction();

		Person person1=new Person(); 
		Person person2=new Person();
		Room room=new Room();
		room.setAddress("NJ-S1-328");
		person1.setName("李方方");
		person2.setName("王艳");
		person1.setRoom(room);
		person2.setRoom(room);
		//这样完成后就可以通过Session对象
		//调用session.save(person1)和session.save(person)
		//会自动保存room
		session.save(person1);
		session.save(person2);
		
		ts.commit();
		
		session.close();
		sf.close();
	}
}

```

### 多对多单向关联
学生和课程就是多对多的关系，一个学生可以选择多门课程，而一门课程又可以被多个学生选择。多对多关系在关系数据库中不能直接表现，还必须依赖一张连接表。
由于是单向的，也就是说一方可以知道另一方，反之不行。这里以从学生知道选择了哪些课程为例实现多对多单向关联。

#### 创建表命令
```
create table student(
id int primary key auto_increment not null ,
snumber varchar(10) not null,
sname varchar(10),
sage int
);

create table course(
id int primary key auto_increment not null,
cnumber varchar(10) not null,
cname varchar(20)
);

create table stu_cour(
sid int not null,
cid int not null,
primary key (sid,cid)
);

```

#### Student.java

```
package com.voidking.hibernate.model;
import java.util.HashSet;
import java.util.Set;

public class Student implements java.io.Serializable {

	// Fields
	private Integer id;
	private String snumber;
	private String sname;
	private Integer sage;
	private Set courses = new HashSet();

	// Constructors

	/** default constructor */
	public Student() {
	}

	/** minimal constructor */
	public Student(String snumber) {
		this.snumber = snumber;
	}

	/** full constructor */
	public Student(String snumber, String sname, Integer sage) {
		this.snumber = snumber;
		this.sname = sname;
		this.sage = sage;
	}

	// Property accessors

	public Integer getId() {
		return this.id;
	}

	public void setId(Integer id) {
		this.id = id;
	}

	public String getSnumber() {
		return this.snumber;
	}

	public void setSnumber(String snumber) {
		this.snumber = snumber;
	}

	public String getSname() {
		return this.sname;
	}

	public void setSname(String sname) {
		this.sname = sname;
	}

	public Integer getSage() {
		return this.sage;
	}

	public void setSage(Integer sage) {
		this.sage = sage;
	}
	
	public Set getCourses(){
		return courses;
	}
	public void setCourses(Set courses){
		this.courses = courses;
	}

}
```

#### Student.hbm.xml
```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">

<hibernate-mapping>
    <class name="com.voidking.hibernate.model.Student" table="student">
        <id name="id" type="java.lang.Integer">
            <column name="ID" />
            <generator class="identity" />
        </id>
        <property name="snumber" type="java.lang.String">
            <column name="SNUMBER" length="10" not-null="true" />
        </property>
        <property name="sname" type="java.lang.String">
            <column name="SNAME" length="10" />
        </property>
        <property name="sage" type="java.lang.Integer">
            <column name="SAGE" />
        </property>
        <set name="courses" table="stu_cour" lazy="true" cascade="all">
        	<key column="SID"></key>
        	<many-to-many class="com.voidking.hibernate.model.Course" column="CID"/>
        </set>
    </class>
</hibernate-mapping>

```

#### Course.java
```
package com.voidking.hibernate.model;


public class Course implements java.io.Serializable {

	// Fields
	private Integer id;
	private String cnumber;
	private String cname;

	// Constructors

	/** default constructor */
	public Course() {
	}

	/** minimal constructor */
	public Course(String cnumber) {
		this.cnumber = cnumber;
	}

	/** full constructor */
	public Course(String cnumber, String cname) {
		this.cnumber = cnumber;
		this.cname = cname;
	}

	// Property accessors

	public Integer getId() {
		return this.id;
	}

	public void setId(Integer id) {
		this.id = id;
	}

	public String getCnumber() {
		return this.cnumber;
	}

	public void setCnumber(String cnumber) {
		this.cnumber = cnumber;
	}

	public String getCname() {
		return this.cname;
	}

	public void setCname(String cname) {
		this.cname = cname;
	}

}
```
#### Course.hbm.xml
```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">

<hibernate-mapping>
    <class name="com.voidking.hibernate.model.Course" table="course">
        <id name="id" type="java.lang.Integer">
            <column name="ID" />
            <generator class="identity" />
        </id>
        <property name="cnumber" type="java.lang.String">
            <column name="CNUMBER" length="10" not-null="true" />
        </property>
        <property name="cname" type="java.lang.String">
            <column name="CNAME" length="20" />
        </property>
    </class>
</hibernate-mapping>

```

#### hibernate.cfg.xml
```
<mapping resource="com/voidking/hibernate/model/Student.hbm.xml"/> 
<mapping resource="com/voidking/hibernate/model/Course.hbm.xml"/>   
```

#### ManyToManySingle.java
```
package com.voidking.hibernate.test;

import java.util.HashSet;
import java.util.Set;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

import com.voidking.hibernate.model.Course;
import com.voidking.hibernate.model.Student;

public class ManyToManySingle{
	public static void main(String[] args) {
		Configuration cfg = new Configuration();
        SessionFactory sf = cfg.configure().buildSessionFactory();
		Session session = sf.openSession();
		
		// 创建事务对象
		Transaction ts=session.beginTransaction();

		//多对多单向关联
		Course cour1=new Course();
		Course cour2=new Course();
		Course cour3=new Course();
		cour1.setCnumber("101");
		cour1.setCname("计算机基础");
		cour2.setCnumber("102");
		cour2.setCname("数据库原理");
		cour3.setCnumber("103");
		cour3.setCname("计算机原理");
		Set courses=new HashSet();
		courses.add(cour1);
		courses.add(cour2);
		courses.add(cour3);
		Student stu=new Student();
		stu.setSnumber("081101");
		stu.setSname("李方方");
		stu.setSage(21);
		stu.setCourses(courses);
		session.save(stu);

		ts.commit();
		
		session.close();
		sf.close();
	}
}

```

### 多对多双向关联
学会多对多单向关联后，只要同时实现两个互逆的多对多单向关联便可轻而易举地实现多对多双向关联。在上面的例子中只要修改课程的代码就可以了。

#### Course.java
```
package com.voidking.hibernate.model;

import java.util.HashSet;
import java.util.Set;


public class Course implements java.io.Serializable {

	// Fields
	private Integer id;
	private String cnumber;
	private String cname;
	private Set stus = new HashSet();

	// Constructors

	/** default constructor */
	public Course() {
	}

	/** minimal constructor */
	public Course(String cnumber) {
		this.cnumber = cnumber;
	}

	/** full constructor */
	public Course(String cnumber, String cname) {
		this.cnumber = cnumber;
		this.cname = cname;
	}

	// Property accessors

	public Integer getId() {
		return this.id;
	}

	public void setId(Integer id) {
		this.id = id;
	}

	public String getCnumber() {
		return this.cnumber;
	}

	public void setCnumber(String cnumber) {
		this.cnumber = cnumber;
	}

	public String getCname() {
		return this.cname;
	}

	public void setCname(String cname) {
		this.cname = cname;
	}

	public Set getStus() {
		return stus;
	}

	public void setStus(Set stus) {
		this.stus = stus;
	}

}
```

#### Course.hbm.xml
```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">

<hibernate-mapping>
    <class name="com.voidking.hibernate.model.Course" table="course">
        <id name="id" type="java.lang.Integer">
            <column name="ID" />
            <generator class="identity" />
        </id>
        <property name="cnumber" type="java.lang.String">
            <column name="CNUMBER" length="10" not-null="true" />
        </property>
        <property name="cname" type="java.lang.String">
            <column name="CNAME" length="20" />
        </property>
        <set name="stus" table="stu_cours" lazy="true" cascade="all">
        	<key column="cid"></key>
        	<many-to-many class="com.voidking.hibernate.model.Student" column="sid"></many-to-many>
        </set>
    </class>
</hibernate-mapping>

```

#### ManyToManyDouble.java
```
package com.voidking.hibernate.test;

import java.util.HashSet;
import java.util.Set;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

import com.voidking.hibernate.model.Course;
import com.voidking.hibernate.model.Student;

public class ManyToManyDouble{
	public static void main(String[] args) {
		Configuration cfg = new Configuration();
        SessionFactory sf = cfg.configure().buildSessionFactory();
		Session session = sf.openSession();
		
		// 创建事务对象
		Transaction ts=session.beginTransaction();

		//多对多单向关联
		Course cour1=new Course();
		Course cour2=new Course();
		Course cour3=new Course();
		cour1.setCnumber("101");
		cour1.setCname("计算机基础");
		cour2.setCnumber("102");
		cour2.setCname("数据库原理");
		cour3.setCnumber("103");
		cour3.setCname("计算机原理");
		Set courses=new HashSet();
		courses.add(cour1);
		courses.add(cour2);
		courses.add(cour3);
		Student stu=new Student();
		stu.setSnumber("081101");
		stu.setSname("李方方");
		stu.setSage(21);
		stu.setCourses(courses);
		session.save(stu);

		ts.commit();
		
		session.close();
		sf.close();
	}
}

```


### 小结
我靠，Hibernate关系映射好复杂！根本没有明显规律，能不能愉快地玩耍了！

### 参考文档
《Java EE基础实用教程》，郑阿奇主编

初识Hibernate——关系映射：http://blog.csdn.net/laner0515/article/details/12905711