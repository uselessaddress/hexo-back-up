title: "图书管理系统之Spring JPA"
toc: true
date: 2015-08-01 22:00:12
tags:
- JavaEE
- JPA
categories: 设计开发
---
# 前言
这一个小系列的项目实战，小编会尽可能解释清楚每一个细节。有些内容在以前的文章中已经写过，这次就当复习了。


# Maven父工程

## 新建工程
1、打开Eclipse，File，New，Maven Project，勾选Create a simple project，Next。

<!--more-->

2、填写Group Id和Artifact Id，Packaging选择pom。如下图：
![](http://7oxjrx.com1.z0.glb.clouddn.com/%40%2Fimgs%2Fbook%2Fspringjpa%2Fparent.jpg)
解释：Maven是一个四维的仓库，里面存在着无数的jar包。四维向量分别是GroupId、ArtifactID、Version、Packaging，创建Maven工程的时候，需要输入这四个参数，其中Version默认为0.0.1-SNAPSHOT，Packaging默认为jar。上图相当于命令：
```
mvn archetype:create -DgroupId=com.voidking.book -DartifactId=book-persist 
```
然后把pom.xml中的packaging改成pom。
其中需要注意的是，在根目录下无法使用mvn命令，比如“E:\\”。

## pom.xml详解
pom.xml文件，是Maven的核心。
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.voidking.book</groupId>
	<artifactId>book-parent</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>pom</packaging>
	
	<!--聚合模块，一条命令就能构建三个模块-->
	<modules>
		<!--module的值是以当前POM为主目录的相对路径-->
		<module>../book-persist</module>
		<module>../book-logic</module>
		<module>../book-ui</module>
		<!--使用继承的话，parent也要加入这里-->
		<!--（但是个人实验证明，不加也可以）-->
		<module>book-parent</module>
	</modules>

	
	<properties>
		<!--自定义属性值，一般用于统一版本-->
		<struts.version>2.3.1</struts.version>
		<springframework.version>3.2.4.RELEASE</springframework.version>
	</properties>

	<!--dependencyManagement中配置的元素既不会给parent引入依赖，-->
	<!--也不会给它的子模块引入依赖，仅仅是它的配置是可继承的。-->
	<!--pluginManagement类似，它是用来进行插件管理的。-->
	<dependencyManagement>
		<!--在父POM中声明依赖-->
		<dependencies>
			<!-- struts2框架 -->
			<dependency>
				<groupId>org.apache.struts</groupId>
				<artifactId>struts2-core</artifactId>
				<version>${struts.version}</version>
			</dependency>
			<dependency>
				<groupId>org.apache.struts</groupId>
				<artifactId>struts2-spring-plugin</artifactId>
				<version>${struts.version}</version>
			</dependency>
			<dependency>
				<groupId>org.apache.struts</groupId>
				<artifactId>struts2-json-plugin</artifactId>
				<version>${struts.version}</version>
			</dependency>
			<dependency>
				<groupId>org.apache.struts</groupId>
				<artifactId>struts2-convention-plugin</artifactId>
				<version>${struts.version}</version>
			</dependency>
			
			<!-- sql server 驱动 -->
			<dependency>
				<groupId>com.microsoft.sqlserver</groupId>
				<artifactId>sqljdbc4</artifactId>
				<version>1.1.1</version>
			</dependency>
			
			<!-- mysql驱动 -->
			<dependency>
				<groupId>mysql</groupId>
				<artifactId>mysql-connector-java</artifactId>
				<version>5.1.30</version>
			</dependency>
			
			<!-- Oracle驱动 -->
			<dependency>
				<groupId>com.oracle</groupId>
				<artifactId>ojdbc14</artifactId>
				<version>10.2.0.3.0</version>
			</dependency>
			
			<!-- Hibernat 4 -->
			<dependency>
				<groupId>org.hibernate</groupId>
				<artifactId>hibernate-core</artifactId>
				<version>4.2.7.Final</version>
			</dependency>
			<dependency>
				<groupId>org.hibernate</groupId>
				<artifactId>hibernate-validator</artifactId>
				<version>5.0.1.Final</version>
			</dependency>
			<dependency>
				<groupId>org.hibernate</groupId>
				<artifactId>hibernate-entitymanager</artifactId>
				<version>4.2.6.Final</version>
			</dependency>

			<dependency>
				<groupId>junit</groupId>
				<artifactId>junit</artifactId>
				<version>4.11</version>
				<scope>test</scope>
			</dependency>

			<!-- Spring 核心库 -->
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-context</artifactId>
				<version>${springframework.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-core</artifactId>
				<version>${springframework.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-beans</artifactId>
				<version>${springframework.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-orm</artifactId>
				<version>${springframework.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-aop</artifactId>
				<version>${springframework.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-tx</artifactId>
				<version>${springframework.version}</version>
			</dependency>
			
			<!-- Spring MVC 库 -->
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-webmvc</artifactId>
				<version>${springframework.version}</version>
			</dependency>

			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-web</artifactId>
				<version>${springframework.version}</version>
			</dependency>

			<dependency>
				<groupId> org.aspectj</groupId>
				<artifactId>aspectjweaver</artifactId>
				<version> 1.6.11</version>
			</dependency>
			<dependency>
				<groupId>org.springframework.data</groupId>
				<artifactId>spring-data-jpa</artifactId>
				<version>1.4.1.RELEASE</version>
			</dependency>
			<dependency>
				<groupId>org.slf4j</groupId>
				<artifactId>slf4j-log4j12</artifactId>
				<version>1.7.5</version>
			</dependency>
			<dependency>
				<groupId>log4j</groupId>
				<artifactId>log4j</artifactId>
				<version>1.2.17</version>
			</dependency>

			<!-- 数据库连接池 -->
			<dependency>
				<groupId>commons-dbcp</groupId>
				<artifactId>commons-dbcp</artifactId>
				<version>1.4</version>
			</dependency>

			<dependency>
				<groupId>javax.servlet</groupId>
				<artifactId>servlet-api</artifactId>
				<version>2.5</version>
			</dependency>
		</dependencies>
	</dependencyManagement>
</project>
```

# Persist层工程

## 新建工程
1、File，New，Maven Project，勾选Create a simple project，Next。

2、填写Group Id和Artifact Id，Packaging选择jar。

## pom.xml详解

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<!--groupId和version省略了，因为使用了继承。-->
	<!--还有一些继承的元素，详见之前总结的《Maven之pom.xml》-->
	<artifactId>book-persist</artifactId>
	
	<!--写出parent的坐标和相对路径-->
	<parent>
		<groupId>com.voidking.book</groupId>
		<artifactId>book-parent</artifactId>
		<version>0.0.1-SNAPSHOT</version>
		<relativePath>../book-parent/pom.xml</relativePath>
	</parent>

	<!--真实依赖-->
	<dependencies>
		<!--只有子模块配置了继承的元素，才会真正的有效，否则不加载-->
		<!--主要配置groupId和artifactId-->
		<dependency>	
			<groupId>com.microsoft.sqlserver</groupId>
			<artifactId>sqljdbc4</artifactId>
			<!--这里并没有指定version，因为继承了parent的version。-->
		</dependency>


		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
		</dependency>
		<!-- Oracle驱动 -->
		<dependency>
			<groupId>com.oracle</groupId>
			<artifactId>ojdbc14</artifactId>
			<!--当然，也可以覆盖父类的version。-->
			<version>10.2.0.3.0</version>
		</dependency>

		<!-- Hibernat 4 框架 -->
		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-core</artifactId>
		</dependency>
		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-validator</artifactId>
		</dependency>
		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-entitymanager</artifactId>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<scope>test</scope>
		</dependency>
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
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjweaver</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.data</groupId>
			<artifactId>spring-data-jpa</artifactId>
		</dependency>

		<!-- 日志打印 -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
		</dependency>
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
		</dependency>

		<!-- 数据库连接池 -->
		<dependency>
			<groupId>commons-dbcp</groupId>
			<artifactId>commons-dbcp</artifactId>
		</dependency>
	</dependencies>

</project>

```
## 聚合和继承的关系
从上面两个pom.xml文件中，我们看到，模块聚合是写在父工程中的，而模块的继承则是写在子工程中的。

区别 ：
1、对于聚合模块来说，它知道有哪些被聚合的模块，但那些被聚合的模块不知道这个聚合模块的存在。
2、对于继承关系的父POM来说，它不知道有哪些子模块继承于它，但那些子模块都必须知道自己的父POM是什么。

共同点 ：
1、聚合POM与继承关系中的父POM的packaging都是pom。
2、聚合模块与继承关系中的父模块除了POM之外都没有实际的内容。
![](http://7oxjrx.com1.z0.glb.clouddn.com/%40%2Fimgs%2Fbook%2Fspringjpa%2Fjuhejicheng.jpg)
注：在现有的实际项目中，一个POM既是聚合POM，又是父POM，这么做主要是为了方便。

## 新建包
新建包com.voidking.book.entity、com.voidking.book.repository、com.voidking.book.service、com.voidking.book.service.impl。这样命名，比较规范。

其中，entity是数据映射层（有人习惯命名为domain，小编还是喜欢entity）；
repository是一个独立的层，介于领域层与数据映射层（数据访问层）之间。它的存在让领域层感觉不到数据访问层的存在，它提供一个类似集合的接口提供给领域层进行领域对象的访问。Repository是仓库管理员，领域层需要什么东西只需告诉仓库管理员，由仓库管理员把东西拿给它，并不需要知道东西实际放在哪；
service对业务逻辑层提供的访问接口；
service.impl是对访问接口的实现。

## entity包
在entity包下，新建Admin.java文件，内容如下。
```
package com.voidking.book.entity;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name="admin")
public class Admin {
	
	@Id
	@GeneratedValue
	private Long id;
	
	@Column(length=20,nullable=false)
	private String name;
	
	@Column(length=20,nullable=false)
	private String pwd;

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getPwd() {
		return pwd;
	}

	public void setPwd(String pwd) {
		this.pwd = pwd;
	}
	
	public Admin() {
		super();
	}

	public Admin(String name, String pwd) {
		super();
		this.name = name;
		this.pwd = pwd;
	}

	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + ((id == null) ? 0 : id.hashCode());
		return result;
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Admin other = (Admin) obj;
		if (id == null) {
			if (other.id != null)
				return false;
		} else if (!id.equals(other.id))
			return false;
		return true;
	}	
	
}

```
1、@Entity注解标识实体Bean。
2、@Table(name=数据库表名)注解，标识该实体Bean映射到关系型数据库中的表名3、如果不指定则JPA实现会自动生成默认的表名称。
4、@Id注解标识主键。
5、@GeneratedValue注解标识主键生成策略，@GeneratedValue(strategy=GenerationType.AUTO)是默认策略，相当于@GeneratedValue或者省略。
- TABLE：使用一个特定的数据库表格来保存主键。 
- SEQUENCE：根据底层数据库的序列来生成主键，条件是数据库支持序列。（适用于oracle） 
- IDENTITY：主键由数据库自动生成（主要是自动增长型，适用于sqlserver，mysql数据库中）
- AUTO：主键由程序控制。 

6、@Column注解有name、unique、nullable、length参数。若不写@Column注解，则一切使用@Column注解的默认值。


在entity下，新建BookBase.java、BookKind.java、ReaderBase.java、ReaderKind.java、BorrowInfo.java，内容分别如下。
```
package com.voidking.book.entity;

/**
 * BookBase
 * 
 */

@Entity
@Table(name="bookbase")
public class BookBase {

	@Id
	@GeneratedValue(strategy=GenerationType.AUTO)
	private Long id;
	
	@Column(length=100,nullable=false)
	private String name;
	
	@Column(length=100,nullable=false)
	private String author;
	
	@Column(length=100,nullable=false)
	private String press;
	
	@Column(length=50,nullable=true)
	private String publishdate;
	
	private Float price;
	
	private Integer page;

	@Column(length=100,nullable=true)
	private String keyword;
	
	@Column(length=50,nullable=true)
	private String registerdate;
	
	@Column(length=2,nullable=false)
	private String borrowed;
	
	@Lob
	private String notice;
	
	@ManyToOne(cascade=CascadeType.REFRESH)
	@JoinColumn(name="bookkind")
	private BookKind bookkind;
	
	//setter和getter方法省略
	
}

```
详细代码请移步https://github.com/voidking/bookmanage.git

1、JPA定义了one-to-one、one-to-many、many-to-one、many-to-many 4种关系。

对于数据库来说，通常在一个表中记录对另一个表的外键关联；对应到实体对象，持有关联数据的一方称为owning-side，另一方称为inverse-side。

为了编程的方便，我们经常会希望在inverse-side也能引用到owning-side的对象，此时就构建了双向关联关系。 在双向关联中，需要在inverse-side定义mappedBy属性，以指明在owning-side是哪一个属性持有的关联数据。

对关联关系映射的要点如下：
 
| 关系类型 | Owning-Side| Inverse-Side |
| --------   | :-----:  | -----:  |
| one-to-one | @OneToOne | @OneToOne(mappedBy="othersideName") |
| one-to-many/many-to-one	 | @ManyToOne | @OneToMany(mappedBy="othersideName") |
| many-to-many | @ManyToMany | @ManyToMany(mappedBy="othersideName") |

关联关系还可以定制延迟加载和级联操作的行为（owning-side和inverse-side可以分别设置）：
通过设置fetch=FetchType.LAZY 或 fetch=FetchType.EAGER来决定关联对象是延迟加载或立即加载。

通过设置cascade={options}可以设置级联操作的行为，其中options可以是以下组合：
- CascadeType.MERGE 级联更新
- CascadeType.PERSIST 级联保存
- CascadeType.REFRESH 级联刷新
- CascadeType.REMOVE 级联删除
- CascadeType.ALL 级联上述4种操作

2、@JoinColumn(name="bookkind")注释本表中指向另一个表的外键。如果不指定name，它会自动帮你生成你指向这个类的类名加上下划线再加上id的列,也就是默认列名是:bookkind_id。

## repository包
在repository包下，新建AdminRepository.java，内容如下。
```
package com.voidking.book.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import com.voidking.book.entity.Admin;

@Repository
public interface AdminRepository extends JpaRepository<Admin,Long>{
	Admin findByNameAndPwd(String name,String pwd);
}
```
Repository（资源库）：通过用来访问领域对象的一个类似集合的接口，在领域与数据映射层之间进行协调。这个叫法就类似于我们通常所说的DAO，在这里，我们就按照这一习惯把数据访问层叫Repository 
Spring Data给我们提供几个Repository，基础的Repository提供了最基本的数据访问功能，其几个子接口则扩展了一些功能。它们的继承关系如下：
 
- Repository：仅仅是一个标识，表明任何继承它的均为仓库接口类，方便Spring自动扫描识别 
- CrudRepository：继承Repository，实现了一组CRUD相关的方法 
- PagingAndSortingRepository：继承CrudRepository，实现了一组分页排序相关的方法 
- JpaRepository：继承PagingAndSortingRepository，实现一组JPA规范相关的方法 
- JpaSpecificationExecutor：比较特殊，不属于Repository体系，实现一组JPA Criteria查询相关的方法 
- 我们自己定义的XxxxRepository需要继承JpaRepository，这样我们的XxxxRepository接口就具备了通用的数据访问控制层的能力。 


1、CrudRepository<T, ID extends Serializable>：这个接口提供了最基本的对实体类的添删改查操作 
```
T save(T entity);//保存单个实体 
Iterable<T> save(Iterable<? extends T> entities);//保存集合 
T findOne(ID id);//根据id查找实体 
boolean exists(ID id);//根据id判断实体是否存在 
Iterable<T> findAll();//查询所有实体,不用或慎用! 
long count();//查询实体数量 
void delete(ID id);//根据Id删除实体 
void delete(T entity);//删除一个实体 
void delete(Iterable<? extends T> entities);//删除一个实体的集合 
void deleteAll();//删除所有实体,不用或慎用! 
```

2、PagingAndSortingRepository<T, ID extends Serializable> 
：这个接口提供了分页与排序功能 
```
Iterable<T> findAll(Sort sort);//排序 
Page<T> findAll(Pageable pageable);//分页查询（含排序功能） 
```

3、JpaRepository<T, ID extends Serializable>：这个接口提供了JPA的相关功能
``` 
List<T> findAll();//查找所有实体 
List<T> findAll(Sort sort);//排序 查找所有实体 
List<T> save(Iterable<? extends T> entities);//保存集合 
void flush();//执行缓存与数据库同步 
T saveAndFlush(T entity);//强制执行持久化 
void deleteInBatch(Iterable<T> entities);//删除一个实体集合 
```


1、简单条件查询:查询某一个实体类或者集合 
按照Spring data 定义的规则，查询方法以find|read|get开头 

2、涉及条件查询时，条件的属性用条件关键字连接，要注意的是：条件属性以首字母大写其余字母小写为规定。 
例如：定义一个Entity实体类 
class User｛ 
	private String firstname; 
	private String lastname; 
｝ 
使用And条件连接时，应这样写： 
findByLastnameAndFirstname(String lastname,String firstname); 
条件的属性名称与个数要与参数的位置与个数一一对应。

新建BookBaseRepository.java、BookKindRepository.java、ReaderBaseRepository.java、ReaderKindRepository.java、BorrowInfoRepository.java，详细代码请移步https://github.com/voidking/bookmanage.git

## service包
在service包中，新建BaseService.java，内容如下。
```
package com.voidking.book.service.impl;

import java.io.Serializable;
import java.util.List;

import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.jpa.repository.JpaRepository;

public interface BaseService<T,ID extends Serializable> 
{
	public boolean isExists(ID id);
	
	public T save(T t);
	
	public void delete(ID id);
	
	public void delete(T t);
	
	public List<T> findAll();
	
	public Page<T> findAll(Pageable pageable);
	
	public Page<T> findAll(int page,int size);
	
	public long count();
	
	public T findOne(ID id);
	
	void setJpaRepository(JpaRepository<T, ID> repository);
}
```
新建AdminService.java，内容如下。
```
package com.voidking.book.service.impl;

import com.voidking.book.entity.Admin;

public interface AdminService extends BaseService<Admin,Long> {
	
	Admin findByNameAndPwd(String name,String pwd);

}

```
问：我们只要entity包、repository包，就可以提供对logic层的服务了，为什么加一个service包呢？
答：信息隐藏，封装实现细节！有了service负责给logic提供服务，那么，我们就不知道底层是用repository还是其他方法实现。

新建BookBaseService.java、BookKindService.java、ReaderBaseService.java、ReaderKindService.java、BorrowInfoService.java，详细代码请移步https://github.com/voidking/bookmanage.git


## service.impl包
在service.impl包下，新建BaseServiceImpl.java，内容如下。
```
package com.voidking.book.service.impl;

import java.io.Serializable;
import java.util.List;

import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;
import org.springframework.data.jpa.repository.JpaRepository;

import com.voidking.book.service.BaseService;

public class BaseServiceImpl<T,ID extends Serializable> implements BaseService<T, ID>
{
	protected JpaRepository<T, ID> repository;
	
	public boolean isExists(ID id) {
		
		return repository.exists(id);
	}

	public T save(T t) {
		
		return repository.save(t);
	}

	public void setJpaRepository(JpaRepository<T, ID> repository) {
		
		this.repository = repository;
	}

	public void delete(ID id) {
		
		this.repository.delete(id);
	}

	public void delete(T t) {
		
		this.repository.delete(t);
		
	}

	public List<T> findAll() {
		
		return this.repository.findAll();
	}

	public Page<T> findAll(Pageable pageable) {
		
		return this.repository.findAll(pageable);
	}

	public long count() {
		
		return this.repository.count();
	}

	public T findOne(ID id) {
		
		return this.repository.findOne(id);
	}

	public Page<T> findAll(int page, int size) {
		
		return this.repository.findAll(new PageRequest(page, size));
	}
}

```
新建AdminServiceImpl.java，内容如下。
```
package com.voidking.book.service.impl;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Service;

import com.voidking.book.entity.Admin;
import com.voidking.book.repository.AdminRepository;
import com.voidking.book.service.AdminService;

@Service
public class AdminServiceImpl extends BaseServiceImpl<Admin, Long> implements AdminService
{
	private AdminRepository adminRepository;
	@Autowired
	@Override
	public void setJpaRepository(JpaRepository<Admin, Long> adminRepository) {
		super.repository = adminRepository;
		this.adminRepository=(AdminRepository) adminRepository;
	}

	public Admin findByNameAndPwd(String name, String pwd) {
		return this.adminRepository.findByNameAndPwd(name, pwd);
	}
	
}
```
1、Spring2.5引入了@Autowired注释，它可以对类成员变量、方法及构造函数进行标注，完成自动装配的工作。不过，现在更推荐使用@Resource。

2、Spring不但支持自己定义的@Autowired注解，还支持几个由JSR-250规范定义的注解，它们分别是@Resource、@PostConstruct以及@PreDestroy。 
@Resource的作用相当于@Autowired，只不过@Autowired按byType自动注入，而@Resource默认按byName自动注入罢了。


新建BookBaseServiceImpl.java、BookKindServiceImpl.java、ReaderBaseServiceImpl.java、ReaderKindServiceImpl.java、BorrowInfoServiceImpl.java，详细代码请移步https://github.com/voidking/bookmanage.git


## spring-persist.xml
在src/test/resources文件夹下，新建spring-persist.xml文件，内容如下。
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:cache="http://www.springframework.org/schema/cache"
	xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
	xmlns:jpa="http://www.springframework.org/schema/data/jpa" xmlns:task="http://www.springframework.org/schema/task"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/aop 
	http://www.springframework.org/schema/aop/spring-aop-3.2.xsd
    http://www.springframework.org/schema/task 
    http://www.springframework.org/schema/task/spring-task-3.2.xsd
    http://www.springframework.org/schema/beans 
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/cache 
    http://www.springframework.org/schema/cache/spring-cache-3.2.xsd
    http://www.springframework.org/schema/tx 
    http://www.springframework.org/schema/tx/spring-tx-3.2.xsd
    http://www.springframework.org/schema/context 
    http://www.springframework.org/schema/context/spring-context-3.2.xsd 
	http://www.springframework.org/schema/data/jpa
	http://www.springframework.org/schema/data/jpa/spring-jpa.xsd">

	<context:component-scan base-package="com.voidking.book" />

	<!-- 数据库属性文件 -->
	<bean id="propertyConfigure"
		class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
		<property name="location" value="classpath:database-conn.properties" />
	</bean>

	<!-- 配置MySQL数据源 -->
	<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource"
		destroy-method="close">
		<property name="driverClassName" value="${database.driver}" />
		<property name="url" value="${database.uri}" />
		<property name="username" value="${database.username}" />
		<property name="password" value="${database.password}" />
	</bean>

	<!-- JPA 配置 -->
	<jpa:repositories base-package="com.voidking.book.repository"></jpa:repositories>

	<bean id="entityManagerFactory"
		class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
		<property name="dataSource" ref="dataSource" />
		<property name="packagesToScan" value="com.voidking.book.entity" />
		<property name="jpaVendorAdapter">
			<bean class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter">
				<property name="showSql" value="${hibernate.show_sql}" />
				<property name="databasePlatform" value="${hibernate.dialect}" />
				<!--  <property name="database" value="${database.name}" />-->
			</bean>
		</property>
		<property name="jpaProperties">
			<props>
				<prop key="hibernate.hbm2ddl.auto">update</prop>
				<prop key="hibernate.connection.useUnicode">true</prop>
				<prop key="hibernate.connection.characterEncoding">UTF-8</prop>
				<prop key="hibernate.connection.charSet">UTF-8</prop>
			</props>
		</property>
	</bean>

	<!-- 事务处理 -->
	<bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager">
		<property name="entityManagerFactory" ref="entityManagerFactory" />
		<property name="jpaDialect">
			<bean class="org.springframework.orm.jpa.vendor.HibernateJpaDialect"></bean>
		</property>
	</bean>

	<tx:advice id="txAdvice" transaction-manager="transactionManager">
		<tx:attributes>
			<!-- hibernate4必须配置为开启事务 否则 getCurrentSession()获取不到 -->
			<tx:method name="find*" propagation="REQUIRED" read-only="true" />
			<tx:method name="update*" propagation="REQUIRED" />
			<tx:method name="delete*" propagation="REQUIRED" />
			<tx:method name="save*" propagation="REQUIRED" />
			<tx:method name="*" propagation="SUPPORTS" />
		</tx:attributes>
	</tx:advice>
	<tx:annotation-driven transaction-manager="transactionManager" />
	<!-- 事务处理配置完毕 -->

	<aop:aspectj-autoproxy expose-proxy="true" />
	<aop:config expose-proxy="true">
		<aop:pointcut id="baseBizMethods"
			expression="execution(public * com.voidking.book.service.impl.*.*(..))" />
		<aop:advisor pointcut-ref="baseBizMethods" advice-ref="txAdvice" />
	</aop:config>
</beans>
```
1、如果配置了context:component-scan那么context:annotation-config标签就可以不用在xml中配置了，因为前者包含了后者。
在xml配置了前者后，spring可以自动去扫描base-pack下面或者子包下面的java文件，如果扫描到有@Component、@Controller、@Service等这些注解的类，则把这些类注册为bean。

2、PropertyPlaceholderConfigurer用于Spring从外部属性文件中载入属性，并使用这些属性值替换Spring 配置文件中的占位符变量（${varible}）。 

3、Spring配置文件中关于事务配置总是由三个组成部分，分别是DataSource、TransactionManager和代理机制这三部分，无论哪种配置方式，一般变化的只是代理机制这部分。


## database-conn.properties
在src/test/resources文件夹下，新建database-conn.properties文件，内容如下。
```
database.name=MYSQL
database.driver=com.mysql.jdbc.Driver
database.uri=jdbc:mysql://localhost:3306/book
database.username=root
database.password=voidking
 
#hibernate
hibernate.dialect=org.hibernate.dialect.MySQL5Dialect
hibernate.show_sql=true
```
在mysql数据库中，新建book数据库。

## log4j.properties
在src/test/resources文件夹下，新建log4j.properties文件，内容如下。
```
log4j.rootCategory=INFO, stdout

###. \u5b9a\u4e49\u540d\u4e3a stdout \u7684\u8f93\u51fa\u7aef\u7684\u7c7b\u578b 
log4j.appender.stdout=org.apache.log4j.ConsoleAppender 
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout 
log4j.appender.stdout.layout.ConversionPattern=[Project] %p [%t] %C.%M(%L) | %m%n 
log4j.appender.stdout.encoding=UTF-8
### .spring \u914d\u7f6e 
log4j.logger.org.springframework=ERROR
 
### . hibernate \u914d\u7f6e 
log4j.logger.org.hibernate=ERROR

```
log4j启动时，默认会寻找source folder下的log4j.xml配置文件，若没有，会寻找log4j.properties文件。然后加载配置。配置文件放置位置正确，不用在程序中手动加载log4j配置文件。如果将配置文件放到了config文件夹下，在build Path中设置下就好了。

## 单元测试
在src/test/java文件夹下，新建包com.voidking.book.service，新建AdminServiceTest.java文件，内容如下。
```
package com.voidking.book.service;

import org.junit.Before;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class AdminServiceTest {
	private AdminService adminService;

	@Before
	public void prepare(){

		ApplicationContext ctx = new ClassPathXmlApplicationContext("spring-persist.xml");

		this.adminService = ctx.getBean(AdminService.class);
		
	}

	@Test
	public void testAdminService(){
		
	}
}


```

上面的测试，主要用来加载配置文件，检验配置文件是否正确，以及数据库的连接是否正常。


# 后记
@Repository、@Service、@Controller和@Component都可以不用在xml文件里进行显式配置，他们都会被默认注册为Spring Bean。

# 参考文档
JPA入门例子(采用JPA的hibernate实现版本)
http://blog.csdn.net/hmk2011/article/details/6289151

Spring声明式事务配置管理方法
http://www.cnblogs.com/rushoooooo/archive/2011/08/28/2155960.html

@Repository、@Service、@Controller 和 @Component
http://blog.csdn.net/ye1992/article/details/19971467

Spring组件扫描\<context:component-scan/\>使用详解
http://blog.csdn.net/a9529lty/article/details/8251003

 \<context:component-scan\>使用说明
http://blog.csdn.net/chunqiuwei/article/details/16115135

使用注解来构造IoC容器
http://www.cnblogs.com/xdp-gacl/p/3495887.html

使用Spring Data JPA 简化 JPA 开发
http://www.ibm.com/developerworks/cn/opensource/os-cn-spring-jpa/

实用的代码规范--SpringSide Coding Standards 
http://blog.chinaunix.net/uid-9354-id-2425025.html

Repository（资源库）模式
http://blog.csdn.net/happinessmoon/article/details/7708785
http://www.cnblogs.com/dudu/archive/2011/05/25/repository_pattern.html

@GeneratedValue
http://blog.csdn.net/fancylovejava/article/details/7438660

JPA常用注解
http://1194867672-qq-com.iteye.com/blog/1730513

@JoinColumn详解
http://blog.sina.com.cn/s/blog_64e8d29b0100z7hx.html

hibernate的注解属性mappedBy详解
http://shenyuc629.iteye.com/blog/1681225

JPA概要
http://www.cnblogs.com/holbrook/archive/2012/12/30/2839842.html

主键中mappedBy的具体使用及其含义
http://blog.sina.com.cn/s/blog_697b968901016s7f.html

简单的Spring JPA实现例子
http://blog.csdn.net/kongxx/article/details/5653370

spring mvc的jpa JpaRepository数据层访问方式汇总
http://jishiweili.iteye.com/blog/2088265

Spring @Autowired详解
http://my.oschina.net/u/138995/blog/181626?fromerr=x1lK5xJq

Spring注解
http://www.360doc.cn/article/495229_37264450.html

Spring注解讲解
http://www.douban.com/note/71602488/

Spring及springmvc注解(annotation)使用详解
http://www.360sdn.com/springmvc/2013/0627/407.html

context:component-scan使用说明
http://blog.csdn.net/chunqiuwei/article/details/16115135

Spring配置之PropertyPlaceholderConfigurer
http://bjyzxxds.iteye.com/blog/427437

Spring事务Transaction配置的五种注入方式详解
http://blog.csdn.net/yaerfeng/article/details/28390773

log4j，如何“自动加载”？
http://www.cnblogs.com/alipayhutu/archive/2013/04/18/3028249.html

log4j.properties配置与加载应用
http://blog.csdn.net/javaloveiphone/article/details/7994313


log4j.properties路径问题
http://blog.csdn.net/caomiao2006/article/details/22062001
