title: JDBC概述
date: 2014-02-18 10:10:24
tags:
- Java
- JDBC
categories: 设计开发
toc: true
---

### 名词解释
JDBC（Java Data Base Connectivity,java数据库连接）是一种用于执行SQL语句的Java API，可以为多种关系数据库提供统一访问，它由一组用Java语言编写的类和接口组成。JDBC提供了一种基准，据此可以构建更高级的工具和接口，使数据库开发人员能够编写数据库应用程序。

### 下载jar包
MySQL：
[MySQL Connectors](http://dev.mysql.com/downloads/connector/)

Oracle：
[JDBC、SQLJ、Oracle JPublisher 和通用连接池 (UCP)](http://www.oracle.com/technetwork/cn/database/features/jdbc/index-093096-zhs.html?ssSourceSiteId=ocomcn)
[JDBC and Universal Connection Pool (UCP)](http://www.oracle.com/technetwork/database/features/jdbc/index-091264.html)

SQL Server：
[Microsoft JDBC Drivers 4.1 and 4.0 for SQL Server](http://www.microsoft.com/en-us/download/details.aspx?id=11774)
<!--more-->

### MySQL用户管理
创建新用户
```
mysql -u root -p
create user 'scott'@'localhost' identified by 'tiger';//创建本地用户
create user 'scott'@'%' identified by 'tiger';//创建远程用户，可选命令
create database test;
grant all prvivileges on test.* to scott;
flush privileges;
select host,user,password from mysql.user;//查看系统有哪些用户
exit
```

新用户登录
```
mysql -u scott -p
show databases;
use test;
show tables;
exit
```

删除新用户
```
mysql -u root -p
drop user 'scott'@'localhost';
drop user 'scott';//相当于drop user 'scott'@'%';
```

### 测试连通性

以测试eclipse和MySQL连接为例。
#### 设置驱动
打开eclipse，Window，Open Perspective，Other...，Database Development，OK。
在左侧Data Source Explorer中，右击Database Connections文件夹，New...，选中MySQL，Name随意，Description随意，Next，New Driver Definition，Name/Type中选中一个System Version，然后在JAR List中选中mysql-connector-java-*-bin.jar，Edit JAR/Zip...，然后选中刚才下载解压的jar包，OK。

#### 设置连接
在Properties的General选项卡中，输入Database、URL、User name、Password，Save password前打钩。

#### 测试连接
设置好连接后，点击Test Connection，即可测试连通性。会提示ping succeeded！或者ping failed！


### MySQL demo设计
使用scott登录MySQL
```
use test;
create table userbase(id int,username varchar(16),passwd varchar(16));
insert into userbase values(1,'voidking','voidking');
insert into userbase values(2,'voidking2','voidking2');
insert into userbase values(3,'voidking3','voidking3');
```

创建jdbc工程，创建包com.voidking.jdbc，新建JdbcMySQL类，内容如下。
```
package com.voidking.jdbc;

import java.sql.*;

public class JdbcMySQL {
	// JDBC driver name and database URL
	static final String JDBC_DRIVER = "com.mysql.jdbc.Driver";
	static final String DB_URL = "jdbc:mysql://localhost/test";

	// Database credentials
	static final String USER = "scott";
	static final String PASS = "tiger";

	public static void main(String[] args) {
		Connection conn = null;
		Statement stmt = null;
		try {
			// STEP 2: Register JDBC driver
			Class.forName(JDBC_DRIVER);

			// STEP 3: Open a connection
			System.out.println("Connecting to database...");
			conn = DriverManager.getConnection(DB_URL, USER, PASS);

			// STEP 4: Execute a query
			System.out.println("Creating statement...");
			stmt = conn.createStatement();
			String sql;
			sql = "select id,username,passwd from userbase";
			ResultSet rs = stmt.executeQuery(sql);

			// STEP 5: Extract data from result set
			while (rs.next()) {
				// Retrieve by column name
				int id = rs.getInt("id");
				String username = rs.getString("username");
				String passwd = rs.getString("passwd");

				// Display values
				System.out.print("ID: " + id);
				System.out.print(", username: " + username);
				System.out.println(", passwd: " + passwd);

			}
			// STEP 6: Clean-up environment
			rs.close();
			stmt.close();
			conn.close();
		} catch (SQLException se) {
			// Handle errors for JDBC
			se.printStackTrace();
		} catch (Exception e) {
			// Handle errors for Class.forName
			e.printStackTrace();
		} finally {
			// finally block used to close resources
			try {
				if (stmt != null)
					stmt.close();
			} catch (SQLException se2) {
			}// nothing we can do
			try {
				if (conn != null)
					conn.close();
			} catch (SQLException se) {
				se.printStackTrace();
			}// end finally try
		}// end try
		System.out.println("Goodbye!");
	}// end main
}// end JdbcMySQL


```

右击JRE System Library，Build Path，Configure Build Path...，Add External JARs...，选中下载解压好的mysql-connector-java-*-bin.jar。

运行项目，即可在控制台看到输出。


### SQL Server demo设计
使用sa登录SQL Server
```
create database test;
//切换到test数据库
create table userbase(id int,username varchar(16),passwd varchar(16));
insert into userbase values(1,'voidking','voidking');
insert into userbase values(2,'voidking2','voidking2');
insert into userbase values(3,'voidking3','voidking3');
```

新建JdbcSQLServer类，内容如下：
```
package com.voidking.jdbc;

import java.sql.*;

public class JdbcSQLServer {

	// JDBC driver name and database URL
	static final String JDBC_DRIVER = "com.microsoft.sqlserver.jdbc.SQLServerDriver";
	static final String DB_URL = "jdbc:sqlserver://127.0.0.1:1433;databaseName=test";

	// Database credentials
	static final String USER = "sa";
	static final String PASS = "123";

	public static void main(String[] args) {
		Connection conn = null;
		Statement stmt = null;
		try {
			// STEP 2: Register JDBC driver
			Class.forName(JDBC_DRIVER);

			// STEP 3: Open a connection
			System.out.println("Connecting to database...");
			conn = DriverManager.getConnection(DB_URL, USER, PASS);

			// STEP 4: Execute a query
			System.out.println("Creating statement...");
			stmt = conn.createStatement();
			String sql;
			sql = "select id,username,passwd from userbase";
			ResultSet rs = stmt.executeQuery(sql);

			// STEP 5: Extract data from result set
			while (rs.next()) {
				// Retrieve by column name
				int id = rs.getInt("id");
				String username = rs.getString("username");
				String passwd = rs.getString("passwd");

				// Display values
				System.out.print("ID: " + id);
				System.out.print(", username: " + username);
				System.out.println(", passwd: " + passwd);

			}
			// STEP 6: Clean-up environment
			rs.close();
			stmt.close();
			conn.close();
		} catch (SQLException se) {
			// Handle errors for JDBC
			se.printStackTrace();
		} catch (Exception e) {
			// Handle errors for Class.forName
			e.printStackTrace();
		} finally {
			// finally block used to close resources
			try {
				if (stmt != null)
					stmt.close();
			} catch (SQLException se2) {
			}// nothing we can do
			try {
				if (conn != null)
					conn.close();
			} catch (SQLException se) {
				se.printStackTrace();
			}// end finally try
		}// end try
		System.out.println("Goodbye!");
	}// end main
}

```

### Oracle demo设计
使用scott用户登录
```
create table userbase(id int,username varchar(16),passwd varchar(16));
insert into userbase values(1,'voidking','voidking');
insert into userbase values(2,'voidking2','voidking2');
insert into userbase values(3,'voidking3','voidking3');
```

新建JdbcOracle类，内容如下：

```
package com.voidking.jdbc;

import java.sql.*;

public class JdbcOracle {
	// JDBC driver name and database URL
	static final String JDBC_DRIVER = "oracle.jdbc.OracleDriver";
	static final String DB_URL = "jdbc:oracle:thin:@localhost:1521:orcl";

	// Database credentials
	static final String USER = "scott";
	static final String PASS = "tiger";

	public static void main(String[] args) {
		Connection conn = null;
		Statement stmt = null;
		try {
			// STEP 2: Register JDBC driver
			Class.forName(JDBC_DRIVER);

			// STEP 3: Open a connection
			System.out.println("Connecting to database...");
			conn = DriverManager.getConnection(DB_URL, USER, PASS);

			// STEP 4: Execute a query
			System.out.println("Creating statement...");
			stmt = conn.createStatement();
			String sql;
			sql = "select id,username,passwd from userbase";
			ResultSet rs = stmt.executeQuery(sql);

			// STEP 5: Extract data from result set
			while (rs.next()) {
				// Retrieve by column name
				int id = rs.getInt("id");
				String username = rs.getString("username");
				String passwd = rs.getString("passwd");

				// Display values
				System.out.print("ID: " + id);
				System.out.print(", username: " + username);
				System.out.println(", passwd: " + passwd);

			}
			// STEP 6: Clean-up environment
			rs.close();
			stmt.close();
			conn.close();
		} catch (SQLException se) {
			// Handle errors for JDBC
			se.printStackTrace();
		} catch (Exception e) {
			// Handle errors for Class.forName
			e.printStackTrace();
		} finally {
			// finally block used to close resources
			try {
				if (stmt != null)
					stmt.close();
			} catch (SQLException se2) {
			}// nothing we can do
			try {
				if (conn != null)
					conn.close();
			} catch (SQLException se) {
				se.printStackTrace();
			}// end finally try
		}// end try
		System.out.println("Goodbye!");
	}// end main
}// end JdbcOracle


```

### 源代码分享
https://github.com/voidking/jdbc.git

### 小结
通过上面三个连接不同数据库的例子，我们发现，代码的差别，仅仅在于驱动包名、数据库的地址、用户名、密码。
那么，这四个信息在哪里获得呢？除了自己记忆之外，小编提供一个查询测试的方法。
Database Development，右击连接，Properties，Driver Properties。这时，已经可以看到驱动、用户名、密码。
至于驱动，请接着点开Edit Driver Definition，Properties，Driver Class后面的就是驱动包。
至此，四项信息都有了。

### 参考文档
JDBC快速入门教程：http://www.yiibai.com/jdbc/jdbc_quick_guide.html


