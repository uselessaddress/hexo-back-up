title: Maven本地安装jar文件
date: 2014-02-26 12:10:24
tags:
- Maven
categories: 设计开发
toc: true
---

### 下载安装Maven
平时使用的，是Eclipse的[Eclipse IDE for Java EE Developers](http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/marsr) 版本，自带Maven。
但是，想要使用Maven本地安装jar文件，就需要自己安装Maven。

下载地址： http://maven.apache.org/download.cgi

1、解压到自己喜欢的目录（这里小编放到`D:\Server`路径下）。
2、添加环境变量`M2_HOME`，值为`D:\Server\apache-maven-3.3.3`
3、在`Path`中添加`;%M2_HOME%\bin;`。

打开命令提示符，输入`mvn -v`，如果能够看到maven版本号，说明安装成功。

### Oracle驱动jar包安装

以安装Oracle驱动jar包为例。
由于Oracle授权问题，Maven不提供Oracle JDBC Driver，为了在Maven项目中应用Oracle JDBC driver,必须手动添加到本地仓库。

#### 下载jar包
[JDBC、SQLJ、Oracle JPublisher 和通用连接池 (UCP)](http://www.oracle.com/technetwork/cn/database/features/jdbc/index-093096-zhs.html?ssSourceSiteId=ocomcn)
[JDBC and Universal Connection Pool (UCP)](http://www.oracle.com/technetwork/database/features/jdbc/index-091264.html)

小编使用的是Oracle11g，下载下来的jar包名为ojdbc14.jar。通过解压缩软件看到，jar包中有一个META-INF/MANIFEST.MF文件。打开这个文件，我们看到
```
Manifest-Version: 1.0
Specification-Title:    Oracle JDBC driver classes for use with JDK14
Sealed: true
Created-By: 1.4.2_08 (Sun Microsystems Inc.)
Implementation-Title:   ojdbc14.jar
Specification-Vendor:   Oracle Corporation
Specification-Version:  Oracle JDBC Driver version - "10.2.0.3.0"
Implementation-Version: Oracle JDBC Driver version - "10.2.0.3.0"
Implementation-Vendor:  Oracle Corporation
Implementation-Time:    Tue Feb 27 15:23:24 2007

Name: oracle/sql/converter/
Sealed: false

Name: oracle/sql/
Sealed: false

Name: oracle/sql/converter_xcharset/
Sealed: false
```
等下我们要用到version信息：`10.2.0.3.0`。
<!--more-->
#### 安装命令
打开命令提示符，进入到ojdbc.jar所在的文件夹下，执行以下命令：
```
mvn install:install-file -DgroupId=com.oracle -DartifactId=ojdbc14 -Dversion=10.2.0.3.0 -Dpackaging=jar -Dfile=ojdbc14.jar
```
看到`BUILD SECCESS`就说明大功告成了！

也许你会问，Dfile参数可不可以使用绝对路径？答：不可以。我在使用绝对路径的时候失败了，不知道为什么。

其实，上面的DgroupId、DartifactId、Dversion全部可以按照自己喜好来定义，只是，在配置pom.xml的时候会用到。所以，我们还是尽可能规范一些。

#### 本地仓库路径
默认为`C:\Users\Administrator\.m2\repository`，在这个路径下，你可以看到本地已经安装的jar包。


#### pom.xml中配置
```
<!-- Oracle驱动 -->
<dependency>
	<groupId>com.oracle</groupId>
	<artifactId>ojdbc14</artifactId>
	<version>10.2.0.3.0</version>
</dependency>
```

### SQLServer驱动jar包安装
类似于Oracle驱动jar包安装，记录命令如下：
```
mvn install:install-file -DgroupId=com.microsoft.sqlserver -DartifactId=jdbc -Dversion=1.1.1 -Dpackaging=jar -Dfile=sqljdbc4.jar
```
pom.xml配置如下：
```
<dependency>
	<groupId>com.microsoft.sqlserver</groupId>
	<artifactId>sqljdbc4</artifactId>
	<version>1.1.1</version>
</dependency>
```

### 参考文档

在Maven仓库中添加Oracle JDBC驱动
http://www.linuxidc.com/Linux/2014-01/95933.htm

------

maven3 手动安装本地jar到仓库
http://www.linuxidc.com/Linux/2014-01/95934.htm

------
MAVEN安装JAR文件到本地仓库
http://www.blogjava.net/paulwong/archive/2014/07/05/415488.html

------

