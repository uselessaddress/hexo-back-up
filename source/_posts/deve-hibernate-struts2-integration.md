title: Hibernate与Struts2整合应用
date: 2014-02-21 14:10:24
tags:
- Hibernate
- Struts2
- JavaEE
categories: 设计开发
toc: true
---


应用实例：学生选课系统。
要求：学生登录系统后，可以查看、修改个人信息，查看个人选课情况，选定课程及退选课程。

### 建立数据库和表
小编仍然使用MySQL数据库，scott用户。在xscj数据库中建立登录表、学生表、专业表、课程表，以及学生课程表即连接表。
```
create table dlb(
id int not null primary key,
xh char(6) not null ,
kl varchar(20)
);

create table xsb(
xh char(6) not null primary key,
xm varchar(8) not null,
xb bit not null check(xb=0 or xb=1),
cssj datetime ,
zy_id int,
zxf int default 0 check(0<=zxf<160),
bz varchar(500),
zp longblob
);

create table zyb(
id int not null auto_increment primary key,
zym varchar(12) not null,
rs int default 0,
fdy varchar(8)
);

create table kcb(
kch char(3) not null primary key,
kcm varchar(12),
kxxq smallint check(kxxq>=1 and kxxq<=8),
xs int default 0,
xf int default 0
);

create table xs_kcb(
xh char(6),
kch char(6),
primary key(xh,kch)
);

```
<!--more-->
### 创建Web项目
新建Dynamic Web Project，命名为struts2_hibernate。

### 添加Hibernate开发能力
右击项目名，Properties，Java Build Path，Libraries，Add Library...，User Library，Next，User Libraries...，New...，输入“hibernate”，OK，Add External JARs...，选中需要的jar文件，打开，OK。

### 自动生成POJO类和映射文件

#### 新建JPA工程
新建JPA Project，输入一个工程名，Next，Next，走到一个界面，提示`At least one user library must be selected.`。
勾选User Library中hibernate之后，显示`The class 'javax.persistence.Convert' is required to be in the selected libraries.`莫非Hibernate的某些包没包含？研究了所有的包，发现，确实没有含有Convert这个类的。估计又是版本问题，真麻烦！

Back，Back，在JPA version中选择v1.0。Next，Next，勾选hibernate，Finish。

右击工程名，JPA Tools，Generate Entities from Tables...。
选择Connection，激活连接，勾选Tables，Finish（或者一路Next，Finish）。

查看src文件夹，发现需要的POJO类已经生成，拷贝到需要的工程下即可。

但是，问题来了：POJO类有了，但是没有*.hbm.xml文件！

聪明的小伙伴肯定想到了，使用myeclipse生成POJO和*.hbm.xml，然后全部拷贝到eclipse的工程下。我只想说，同时安装eclipse和myeclipse，还换着用，这是有多无聊！

自己写？当然可以！但是人家myeclipse的POJO类和*.hbm.xml都可以自动生成，eclipse凭什么不可以？下面我们就研究一下方法。

#### Hibernate Tools插件

##### 第一招
Help，Eclipse Marketplace...，在find中输入“Hibernate”，搜索到的选项中有Hibernate Tools（Indigo）3.4.X，单击Install。
安装过程中，报错中断：
```
'Installing Software ' has encountered a problem.
An error occurred while collecting items to be installed

```
本人的Eclipse是Luna版本（当前最新版），怀疑是兼容问题。重来，搜索到的选项中有JBoss Tools（Luna），单击Install。安装过程中选择Hibernate Tools安装，最终，依然报错，还能不能愉快地玩耍了！

##### 第二招

我们来到[JBoss Tools官网](http://tools.jboss.org)，然后找到[下载界面](http://tools.jboss.org/downloads/jbosstools/luna/4.2.3.Final.html)。可以看到，官方给出了三种下载方式：Eclipse Marketplace，Update Site和Artifacts。

刚才我们无意中使出第一招，失败了，让我们试试第二招。

Help ，Install New Software… ，Work with：http://download.jboss.org/jbosstools/updates/stable/luna/ ，然后选择Hibernate Tools，Next。

慢，非常慢，完全受不了这个速度。。。

##### 第三招

第三招是我喜欢的方式：离线安装。感觉很靠谱，来试试看。
首先下载所需的zip文件，这里小编选择的是Update site (including sources) bundle of all Hibernate Tools。下载下来文件名为jbosstools-4.2.3.Final_2015-03-26_23-05-30-B264-updatesite-hibernatetools.zip。

Help ， Install New Software… ， Add… ， Archive… 。选中下载的文件，选择需要安装的插件，Next。啊勒，我靠，为什么还是这么慢！仔细观察一下进度条的提示，在Fetching什么玩意，貌似明白了什么。点击停止，然后把`Contact all update sites during install to find requred software`前面的勾去掉，再次Next。果然，进度马上就跑起来了！但是，跑着跑着就停了！进度条提示`Connot perform operation.Computing alternate solutions,may take a while:`，这个a while有点长啊！-_-|||，还能怎么办？放大招吧！下载Update site (including sources) bundle of all JBoss Core Tools，然后全部安装。我就不信了，全部的工具都在这，你还缺少依赖？

我去，这下更不得了，需要下载好多好多依赖的包，彻底醉了。。。

只勾选Hibernate Tools，试一试，成功！不容易啊！T_T

##### 使用
官方教程：http://tools.jboss.org/features/hibernate.html

[这篇文章](http://www.javacodegeeks.com/2013/10/step-by-step-auto-code-generation-for-pojo-domain-java-classes-and-hbm-using-eclipse-hibernate-plugin.html)写得更详细：Step by step auto code generation for POJO domain java classes and hbm using Eclipse Hibernate plugin

[这篇文章](http://www.mkyong.com/hibernate/how-to-generate-code-with-hibernate-tools/)写得也不错：How to generate Hibernate mapping files & annotation with Hibernate Tools

Window，Open Perspective，Hibernate。

Add Configuration...，选中Project，Database connection ，Setup... Propery file，Setup... Configuration file（并且进行一些配置）。然后，然后，然后就出现了错误！！！
`[Classpath]: Could not parse configuration:/hibernate.cfg.xml`，试了各种方法，都没法解决这个问题，这是要闹哪样啊！

多么奇葩的问题，本以为还要花费一番功夫。半小时后，莫名其妙的好了！好了。。。

重新来一遍，完整的：

Window，Open Perspective，Hibernate。

Add Configuration...，选中Project（struts2_hibernate），选择Database connection，Setup... Propery file，Setup... Configuration file（需要进行一些配置）。

这时，会提示缺少MySQL驱动，在Classpath选项卡中配置一下就好了。

File，New，Hibernate Reverse Engineering File（reveng.xml），选中struts2_hibernate，Next，选择Console configuration，Refresh，

在工具栏中，有三个三角号（绿色圆底），其中一个三角号的右下角有Hibernate的小标志。点击它的下拉菜单，Hibernate Code Generation Configurations...。
New，在Main选项卡中选择Console configuration，选择Output directory，勾选Reverse engineer from JDBC Connection。
在Exporters选项卡中勾选Domain code（.java）和Hibernate XML Mappings（.hbm.xml）。Run。

观察struts2_hibernate工程下的文件，可以发现，我们需要的POJO类和*.hbm.xml文件都有了！

##### 断网配置本地dtd文件
刚才完整操作的时候，断网了，无法解析hibernate.cfg.xml文件，看来需要配置个本地dtd文件才可以。这里需要配置hibernate-configuration-3.0.dtd，最好也配置一下hibernate-mapping-3.0.dtd，这两个文件在hibernate3.jar中可以找到。配置好之后，问题完美解决。选中一些表，Include...，Finish。



下面是Struts2验证框架出现同样问题时的解决办法，从《Struts2概述》摘录，方便查阅：

1、解压xwork-core-*.jar包，找到xwork-validator-1.0.dtd。

2、eclipse，Window，Preferences，XML，XML Catelog，Add，File System...，选中刚才解压的xwork-validator-1.0.dtd，打开，

3、Location已经选好`D:\jar\struts2\xwork-validator-1.0.dtd`，Key type选择`Public ID`，Key填`-//OpenSymphony Group//XWork Validator Config 1.0//EN`，Alternative web address填`http://struts.apache.org/dtds/xwork-validator-1.0.dtd`。（本地dtd不存在时回去web上去找dtd）

4、StrutsAction-validation.xml需要修改如下：
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE validators PUBLIC
        "-//OpenSymphony Group//XWork Validator Config 1.0//EN"
        "D:\jar\struts2\xwork-validator-1.0.dtd">
<validators>
<!--需要校验的字段的字段名-->
	<field name="name">
		<field-validator type="requiredstring">
			<!--去空格-->
			<param name="trim">true</param>
			<!--错误提示信息-->
			<message>姓名是必须的</message>
		</field-validator>
	</field>
</validators>
```

### *.hbm.xml文件修改
#### Xsb.hbm.xml
```
<?xml version="1.0"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">
<!-- Generated 2015-5-23 0:27:57 by Hibernate Tools 3.4.0.CR1 -->
<hibernate-mapping>
    <class name="Xsb" table="xsb" catalog="xscj">
        <id name="xh" type="string">
            <column name="xh" length="6" />
            <generator class="assigned" />
        </id>
        <property name="xm" type="string">
            <column name="xm" length="8" not-null="true" />
        </property>
        <property name="xb" type="boolean">
            <column name="xb" not-null="true" />
        </property>
        <property name="cssj" type="timestamp">
            <column name="cssj" length="19" />
        </property>
        <property name="zyId" type="java.lang.Integer">
            <column name="zy_id" />
        </property>
        <property name="zxf" type="java.lang.Integer">
            <column name="zxf" />
        </property>
        <property name="bz" type="string">
            <column name="bz" length="500" />
        </property>
        <property name="zp" type="binary">
            <column name="zp" />
        </property>
        
        <many-to-one name="zyb" class="com.voidking.struts2_hibernate.model.Zyb" fetch="select" cascade="all" lazy="false">
        	<column name="zy_id"></column>
        </many-to-one>
        
        <set name="kcs" table="xs_kcb" lazy="false" cascade="save-update">
        	<key column="xh"></key>
        	<many-to-many class="com.voidking.struts2_hibernate.model.Kcb" column="kch"></many-to-many>
        </set>
    </class>
</hibernate-mapping>

```
#### Kcb.hbm.xml
```
<?xml version="1.0"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">
<!-- Generated 2015-5-23 0:27:57 by Hibernate Tools 3.4.0.CR1 -->
<hibernate-mapping>
    <class name="Kcb" table="kcb" catalog="xscj">
        <id name="kch" type="string">
            <column name="kch" length="3" />
            <generator class="assigned" />
        </id>
        <property name="kcm" type="string">
            <column name="kcm" length="12" />
        </property>
        <property name="kxxq" type="java.lang.Short">
            <column name="kxxq" />
        </property>
        <property name="xs" type="java.lang.Integer">
            <column name="xs" />
        </property>
        <property name="xf" type="java.lang.Integer">
            <column name="xf" />
        </property>
        <set name="xss" table="xs_kcb" lazy="true" inverse="true">
        	<key column="kch"></key>
        	<many-to-many class="com.voidking.struts2_hibernate.model.Xsb"></many-to-many>
        </set>
    </class>
</hibernate-mapping>

```

### Dao层组件实现

#### DlDao.java
```
package com.voidking.struts2_hibernate.dao;

import com.voidking.struts2_hibernate.model.Dlb;

public interface DlDao {
	public Dlb validate(String xh,String kl);
}

```

#### DlDaoImp.java
```
package com.voidking.struts2_hibernate.dao.imp;

import org.hibernate.Query;
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

import com.voidking.struts2_hibernate.dao.DlDao;
import com.voidking.struts2_hibernate.model.Dlb;

public class DlDaoImp implements DlDao {

	@Override
	public Dlb validate(String xh, String kl) {
		// TODO Auto-generated method stub
		try {
			Configuration cf = new Configuration();
			SessionFactory sf = cf.buildSessionFactory();
			Session session = sf.openSession();
			
			Transaction ts = session.beginTransaction();
			Query query = session.createQuery("from Dlb where xh=? and kl=?");
			query.setParameter(0, xh);
			query.setParameter(1, kl);
			query.setMaxResults(1);
			Dlb dlb =(Dlb)query.uniqueResult();
			
			ts.commit();
			session.close();
			sf.close();
			
			if(dlb!=null){
				return dlb;
			}else{
				return null;
			}
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		}
		return null;
	}

}


```

#### XsDao.java
```
package com.voidking.struts2_hibernate.dao;

import com.voidking.struts2_hibernate.model.Xsb;

public interface XsDao {
	public Xsb getOneXs(String xh);
	
	public void update(Xsb xs);
}

```

#### XsDaoImp.java
```
package com.voidking.struts2_hibernate.dao.imp;

import org.hibernate.Query;
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

import com.voidking.struts2_hibernate.dao.XsDao;
import com.voidking.struts2_hibernate.model.Xsb;

public class XsDaoImp implements XsDao {

	@Override
	public Xsb getOneXs(String xh) {
		// TODO Auto-generated method stub
		try {
			Configuration cf = new Configuration();
			SessionFactory sf = cf.buildSessionFactory();
			Session session = sf.openSession();
			Transaction ts = session.beginTransaction();
			Query query = session.createQuery("from xsb where xh=?");
			query.setParameter(0, xh);
			query.setMaxResults(1);
			Xsb xs = (Xsb)query.uniqueResult();
			ts.commit();
			session.close();
			sf.close();
			
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		}
		return null;
	}

	@Override
	public void update(Xsb xs) {
		// TODO Auto-generated method stub
		try {
			Configuration cf = new Configuration();
			SessionFactory sf = cf.buildSessionFactory();
			Session session = sf.openSession();
			Transaction ts = session.beginTransaction();
			
			session.update(xs);
			
			ts.commit();
			session.close();
			sf.close();
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		}
	}

}

```
写完这个文件，我突然发现，写一个HibernateSessionFactory类当做工具真的太必要了！不然的话，每次我想用Session的时候，都要
```
Configuration cf = new Configuration();
SessionFactory sf = cf.buildSessionFactory();
Session session = sf.openSession();
```
每次都重新new一个Configuration，建立一个SessionFactory，一定加大了系统的开销。而且，一个数据库对应一个SessionFactory就够了！

myeclipse自动生成的HibernateSessionFactory类，感觉有点复杂，自己写一个吧！

#### MySessionFactory.java
```
package com.voidking.struts2_hibernate.util;

import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;
 
//在使用hibernate开发项目，请一定保证只有一个SessionFactory
//一个数据库对应一个SessionFactory 对象.
final public class MySessionFactory {
 
    private static SessionFactory sessionFactory=null;
     
    private MySessionFactory(){
         
    }
     
    static{         
        sessionFactory =new Configuration().configure("/hibernate.cfg.xml").buildSessionFactory(); 
    }
     
    public static SessionFactory getSessionFactory(){
        return sessionFactory;
    }
     
}
```

#### 重写DlDaoImp.java

```
package com.voidking.struts2_hibernate.dao.imp;

import org.hibernate.Query;
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

import com.voidking.struts2_hibernate.dao.DlDao;
import com.voidking.struts2_hibernate.model.Dlb;
import com.voidking.struts2_hibernate.util.MySessionFactory;

public class DlDaoImp implements DlDao {

	@Override
	public Dlb validate(String xh, String kl) {
		// TODO Auto-generated method stub
		try {
			Session session = MySessionFactory.getSessionFactory().openSession();
			
			Transaction ts = session.beginTransaction();
			Query query = session.createQuery("from Dlb where xh=? and kl=?");
			query.setParameter(0, xh);
			query.setParameter(1, kl);
			query.setMaxResults(1);
			Dlb dlb =(Dlb)query.uniqueResult();
			
			ts.commit();
			session.close();
			
			if(dlb!=null){
				return dlb;
			}else{
				return null;
			}
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		}
		return null;
	}

}

```

#### 重写XsDaoImp.java
```
package com.voidking.struts2_hibernate.dao.imp;

import org.hibernate.Query;
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

import com.voidking.struts2_hibernate.dao.XsDao;
import com.voidking.struts2_hibernate.model.Xsb;
import com.voidking.struts2_hibernate.util.MySessionFactory;

public class XsDaoImp implements XsDao {

	@Override
	public Xsb getOneXs(String xh) {
		// TODO Auto-generated method stub
		try {

			Session session = MySessionFactory.getSessionFactory().openSession();
			Transaction ts = session.beginTransaction();
			Query query = session.createQuery("from xsb where xh=?");
			query.setParameter(0, xh);
			query.setMaxResults(1);
			Xsb xs = (Xsb)query.uniqueResult();
			ts.commit();
			session.close();
			
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		}
		return null;
	}

	@Override
	public void update(Xsb xs) {
		// TODO Auto-generated method stub
		try {
			Session session = MySessionFactory.getSessionFactory().openSession();
			Transaction ts = session.beginTransaction();
			
			session.update(xs);
			
			ts.commit();
			session.close();
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		}
	}

}

```
#### ZyDao.java
```
package com.voidking.struts2_hibernate.dao;

import java.util.List;

import com.voidking.struts2_hibernate.model.Zyb;

public interface ZyDao {
	public Zyb getOneZy(Integer zyId);
	public List getAll();
}

```

#### ZyDaoImp.java
```
package com.voidking.struts2_hibernate.dao.imp;

import java.util.List;

import org.hibernate.Query;
import org.hibernate.Session;
import org.hibernate.Transaction;

import com.voidking.struts2_hibernate.dao.ZyDao;
import com.voidking.struts2_hibernate.model.Zyb;
import com.voidking.struts2_hibernate.util.MySessionFactory;

public class ZyDaoImp implements ZyDao {

	@Override
	public Zyb getOneZy(Integer zyId) {
		try {
			Session session = MySessionFactory.getSessionFactory().openSession();
			Transaction ts = session.beginTransaction();
			
			Query query = session.createQuery("from zyb where id=?");
			query.setParameter(0, zyId);
			query.setMaxResults(1);
			Zyb zy = (Zyb)query.uniqueResult();
			
			ts.commit();
			session.close();
			
			return zy;
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		}
		return null;
	}

	@Override
	public List getAll() {
		// TODO Auto-generated method stub
		try {
			Session session = MySessionFactory.getSessionFactory().openSession();
			Transaction ts = session.beginTransaction();
			
			List list = session.createQuery("from zyb").list();
			
			ts.commit();
			session.close();
			return list;
			
		} catch (Exception e) {
			e.printStackTrace();
		}
		return null;
	}

}

```

#### KcDao.java
```
package com.voidking.struts2_hibernate.dao;

import java.util.List;

import com.voidking.struts2_hibernate.model.Kcb;

public interface KcDao {
	public Kcb getOneKc(String kch);
	public List getAll();
}

```

#### KcDaoImp.java
```
package com.voidking.struts2_hibernate.dao.imp;

import java.util.List;

import org.hibernate.HibernateException;
import org.hibernate.Query;
import org.hibernate.Session;
import org.hibernate.Transaction;

import com.voidking.struts2_hibernate.dao.KcDao;
import com.voidking.struts2_hibernate.model.Kcb;
import com.voidking.struts2_hibernate.util.MySessionFactory;

public class KcDaoImp implements KcDao {

	@Override
	public Kcb getOneKc(String kch) {
		// TODO Auto-generated method stub
		try {
			Session session = MySessionFactory.getSessionFactory().openSession();
			Transaction ts = session.beginTransaction();
			
			Query query = session.createQuery("from kcb where kch=?");
			query.setParameter(0, kch);
			query.setMaxResults(1);
			Kcb kc = (Kcb)query.uniqueResult();
			
			ts.commit();
			session.close();
			return kc;
			
		} catch (Exception e) {
			e.printStackTrace();
		}
		return null;
	}

	@Override
	public List getAll() {
		// TODO Auto-generated method stub
		try {
			Session session = MySessionFactory.getSessionFactory().openSession();
			Transaction ts = session.beginTransaction();
			List list = session.createQuery("from kcb order by kch").list();
			ts.commit();
			session.close();
			return list;
		} catch (HibernateException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		return null;
	}

}


```

### struts.xml
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
    "http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
	<constant name="struts.multipart.saveDir" value="/tmp"/>
    <package name="default"  extends="struts-default">
    	<action name="login" class="com.voidking.struts2_hibernate.action.LoginAction">
			<result name="success">/main.jsp</result>
			<result name="error">/login.jsp</result>
		</action>
		<action name="xsInfo" class="org.action.XsAction">
			<result name="success">/xsInfo.jsp</result>
		</action>
		<action name="getImage" class="com.voidking.struts2_hibernate.action.XsAction" method="getImage"></action>
		<action name="updateXsInfo" class="com.voidking.struts2_hibernate.action.XsAction" method="updateXsInfo">
			<result name="success">/updateXsInfo.jsp</result>
		</action>
		<action name="updateXs" class="com.voidking.struts2_hibernate.action.XsAction" method="updateXs">
			<result name="success">/updateXs_success.jsp</result>
		</action>
		<action name="getXsKcs" class="com.voidking.struts2_hibernate.action.XsAction" method="getXsKcs">
			<result name="success">/xsKcs.jsp</result>
		</action>
		<action name="deleteKc" class="com.voidking.struts2_hibernate.action.XsAction" method="deleteKc">
			<result name="success">/deleteKc_success.jsp</result>
		</action>
		<action name="getAllKc" class="com.voidking.struts2_hibernate.action.KcAction">
			<result name="success">/allKc.jsp</result>
		</action>
		<action name="selectKc" class="com.voidking.struts2_hibernate.action.XsAction" method="selectKc">
			<result name="success">/selectKc_success.jsp</result>
			<result name="error">/selectKc_fail.jsp</result>
		</action>
    </package>
</struts>


```

### web.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app id="WebApp_9" version="2.4" xmlns="http://java.sun.com/xml/ns/j2ee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
	<welcome-file-list>
		<welcome-file>/login.jsp</welcome-file>
	</welcome-file-list>
    <filter>
        <filter-name>struts2</filter-name>
        <filter-class>org.apache.struts2.dispatcher.FilterDispatcher</filter-class>
    </filter>

    <filter-mapping>
        <filter-name>struts2</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

</web-app>

```

### 界面交互

Action类和jsp界面，在此省略。详情请见源代码。



### 严重错误
部署，运行，404错误。进入Tomcat的Manager App界面，start项目，报错如下：
```
SEVERE:Exception starting filter struts2
java.lang.ClassNotFoundException: org.apache.struts2.dispatcher.FilterDispatcher
```
我勒个去，怎么就找不到过滤器了！换成`org.apache.struts2.dispatcher.ng.filter.StrutsPrepareFilter`试试，报同样的错：
```
SEVERE:Exception starting filter struts2
java.lang.ClassNotFoundException: org.apache.struts2.dispatcher.ng.filter.StrutsPrepareFilter
```
这个工程，和之前写的struts2工程有什么差别？
struts2中，我直接把jar包放到了WebContent/WEB-INF/lib里面，而在这个项目中，我把jar包放到了WebContent/WEB-INF/lib/struts2里。删除一层目录试试，问题完美解决。

### 很多错误

输入学号，密码，点击登录，报错如下：

```
HTTP Status 500 - 

type Exception report

message 

description The server encountered an internal error that prevented it from fulfilling this request.

exception 

java.lang.reflect.InvocationTargetException
	sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
	……
	
root cause 
	……

```
root caose下面的内容是重点，要么是lib中缺少jar文件，要么某个类写错了，要么配置文件错误……总之各种错误，根据提示定位修改即可。

### 源代码分享

https://github.com/voidking/struts2_hibernate.git

### 小结
太复杂了，根本没法愉快的玩耍！改了几十个错误！！！尤其是Hibernate中POJO类和*.hbm.xml对应的部分，太难搞了！记得JPA似乎简单一点，等到复习到那部分再深入研究。最终，程序跑起来了，但是很多功能都没法用，各种not mapped，实在搞不下去了，能力有限啊。。。

发现：在JavaSE中，可以通过引入jar包的方式使用外部类。但是在JavaEE中，也就是Web工程中，需要拷贝jar包到lib文件夹中才可以使用外部类。
分析：发布到Tomcat中的工程，根本没有记录引用外部类的文件，也就是说，找不到外部jar包。而lib文件夹中的jar包，应该是默认寻找外部类的地方。


### 参考文档
《Java EE基础实用教程》，郑阿奇主编

------
在Eclipse中从数据库表自动生成hibernate的java实体类
http://www.shangxueba.com/jingyan/2453455.html
http://blog.csdn.net/zheng2008hua/article/details/6274659

------
Hibernate初始化时的Could not parse configuration
http://blog.csdn.net/mydeman/article/details/6134820

------
如何理解Hibernate中的HibernateSessionFactory类
http://www.cnblogs.com/dyllove98/archive/2013/08/07/3243769.html

------
org.hibernate.MappingException:An association from the table * refers to an unmapped class:
http://blog.csdn.net/jayqean/article/details/5608330

------
org.hibernate.HibernateException: Unable to instantiate default tuplizer
http://www.cnblogs.com/chuyuhuashi/archive/2012/03/29/2423235.html

------
