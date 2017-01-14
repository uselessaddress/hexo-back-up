title: Maven之pom.xml
date: 2014-02-25 10:10:24
tags:
- Maven
categories: 设计开发
toc: true
---
# pom.xml速览
```
<project>  
  <modelVersion>4.0.0</modelVersion>  
  <groupId>...</groupId>  
  <artifactId>...</artifactId>  
  <version>...</version>  
  <packaging>...</packaging>  
  <dependencies>...</dependencies>  
  <parent>...</parent>  
  <dependencyManagement>...</dependencyManagement>  
  <modules>...</modules>  
  <properties>...</properties>  
  
  <build>...</build>  
  <reporting>...</reporting>  
  
  <name>...</name>  
  <description>...</description>  
  <url>...</url>  
  <inceptionYear>...</inceptionYear>  
  <licenses>...</licenses>  
  <organization>...</organization>  
  <developers>...</developers>  
  <contributors>...</contributors>  
  
  <issueManagement>...</issueManagement>  
  <ciManagement>...</ciManagement>  
  <mailingLists>...</mailingLists>  
  <scm>...</scm>  
  <prerequisites>...</prerequisites>  
  <repositories>...</repositories>  
  <pluginRepositories>...</pluginRepositories>  
  <distributionManagement>...</distributionManagement>  
  <profiles>...</profiles>  
</project>  
```

POM包括了所有的项目信息：

- groupId：项目或者组织的唯一标志，并且配置时生成的路径也是由此生成，如org.codehaus.mojo生成的相对路径为：/org/codehaus/mojo
- artifactId：项目的通用名称
- version：项目的版本
- packaging：打包的机制，如pom, jar, maven-plugin, ejb, war, ear, rar, par
- classifier：分类

# POM关系

## 依赖
```
<dependencies>  
    <dependency>  
      <groupId>junit</groupId>  
      <artifactId>junit</artifactId>  
      <version>4.0</version>  
      <type>jar</type>  
      <scope>test</scope>  
      <optional>true</optional>  
    </dependency>  
    ...  
</dependencies>  
```

- groupId, artifactId, version:描述了依赖的项目唯一标志
- type:相应的依赖产品包形式，如jar，war
- scope:用于限制相应的依赖范围，包括以下的几种变量：

> compile ：默认范围，用于编译
provided：类似于编译，但支持你期待jdk或者容器提供，类似于classpath
runtime:在执行时，需要使用
test:用于test任务时使用
system:需要外在提供相应得元素。通过systemPath来取得

- systemPath: 仅用于范围为system。提供相应的路径
- optional: 标注可选，当项目自身也是依赖时。用于连续依赖时使用

独占性外在告诉maven你只包括指定的项目，不包括相关的依赖。此因素主要用于解决版本冲突问题。
```
<dependencies>  
    <dependency>  
      <groupId>org.apache.maven</groupId>  
      <artifactId>maven-embedder</artifactId>  
      <version>2.0</version>  
      <exclusions>  
        <exclusion>  
          <groupId>org.apache.maven</groupId>  
          <artifactId>maven-core</artifactId>  
        </exclusion>  
      </exclusions>  
    </dependency> 
</dependencies>
```
表示项目maven-embedder需要项目maven-core，但我们不想引用maven-core。

## 聚合
为了能够使用一条命令就能构建account-email和account-persist两个模块，我们需要建立一个额外的名为account-aggregator的模块，然后通过该模块构建整个项目的所有模块。account-aggregator本身也是个 Maven项目，它的 POM如下
```
<project>
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.juvenxu.mvnbook.account</groupId>
	<artifactId>account-aggregator</artifactId>
	<version>1.0.0-SNAPSHOT</version>
	<packaging> pom </packaging>
	<name>Account Aggregator</name>
	 <modules>
		<module>account-email</module>
		<module>account-persist</module>
	 </modules>
</project>
```

注意：packaging的类型为pom ，module的值是一个以当前POM为主目录的相对路径。

## 继承
### 父模块配置
```
<project>
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.juvenxu.mvnbook.account</groupId>
	<artifactId> account-parent </artifactId>
	<version>1.0.0-SNAPSHOT</version>
	<packaging>pom</packaging>
	<name>Account Parent</name>
	
	<dependencyManagement>...</dependencyManagement>  
</project>
```



### 子模块配置
```
<project>
	<modelVersion>4.0.0</modelVersion>
	
	< parent >
		<groupId>com.juvenxu.mvnbook.account</groupId>
		<artifactId> account-parent </artifactId>
		<version>1.0.0-SNAPSHOT</version>
		< relativePath >../account-parent/pom.xml</ relativePath>
	</ parent >
	
	<artifactId> account-email </artifactId>
	<name>Account Email</name>
  ...
</project>
```

注意：
1、子模块没有声明groupId和version，这两个属性继承至父模块。但如果子模块有不同与父模块的 groupId、version ，也可指定；
2、不应该继承artifactId,如果groupId ，version，artifactId 完全继承的话会造成坐标冲突；另外即使使用不同的 groupId或version，同样的 artifactId也容易产生混淆。


### 聚合
使用继承后，parent也必须像子模块一样加入到聚合模块中。也就是在在聚合模块的 pom中加入`<module>account-parent</module>`。聚合的 POM如下：
```
<project>
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.juvenxu.mvnbook.account</groupId>
	<artifactId>account-aggregator</artifactId>
	<version>1.0.0-SNAPSHOT</version>
	<packaging> pom </packaging>
	<name>Account Aggregator</name>
	<modules>
		<module>account-email</module>
		<module>account-persist</module>
		<module> account-parent</module>
	</modules>
</project>
```

### 可继承的元素

```
groupId ：项目组 ID ，项目坐标的核心元素；
version ：项目版本，项目坐标的核心元素；
description ：项目的描述信息；
organization ：项目的组织信息；
inceptionYear ：项目的创始年份；
url ：项目的 url 地址
develoers ：项目的开发者信息；
contributors ：项目的贡献者信息；
distributionManagerment ：项目的部署信息；
issueManagement ：缺陷跟踪系统信息；
ciManagement ：项目的持续继承信息；
scm ：项目的版本控制信息；
mailingListserv ：项目的邮件列表信息；
properties ：自定义的 Maven 属性；
dependencies ：项目的依赖配置；
dependencyManagement ：醒目的依赖管理配置；
repositories ：项目的仓库配置；
build ：包括项目的源码目录配置、输出目录配置、插件配置、插件管理配置等；
reporting ：包括项目的报告输出目录配置、报告插件配置等。
```

我们知道dependencies是可以被继承的，这个时候我们就想到让我们的发生了共用的依赖元素转移到parent中，这样我们又进一步的优化了配置。可是问题也随之而来，如果有一天我创建了一个新的模块，但是这个模块不需要这些parent的依赖，这时候如何处理？

是的，maven的依赖管理就是来解决这个问题的：dependencyManagement。
从上面的列表中我们发现dependencyManagement也是可以被继承的，这恰恰满足了我们的需要，它既能够让子模块继承到父模块的依赖配置，又能保证子模块依赖使用的灵活性。

dependencyManagement的特性：在dependencyManagement中配置的元素既不会给parent引入依赖，也不会给它的子模块引入依赖，仅仅是它的配置是可继承的。

最佳实践：
这时候我们就可以在父POM中声明这些依赖：
```
<properties>
	<target.version>2.5.6</target.version>
</properties>

<dependencyManagement>
	<dependencies>
		<dependency>
			<groupId>your groupId</groupId>
			<artifactId>your artifactId</artifactId>
			<version>${target.version}</version>
		</dependency>
	</dependencies>
</dependencyManagement>
```

子模块的POM继承这些配置：子模块继承这些配置的时候，仍然要声明groupId和artifactId，表示当前配置是继承于父POM的，从而直接使用父POM的版本对应的资源。

```
<dependencies>
	<dependency>
		<groupId>your groupId</groupId>
		<artifactId>your artifactId</artifactId>
	</dependency>
</dependencies>
```
这个可以有效的避免多个子模块使用依赖版本不一致的情况，有助于降低依赖冲突的几率。

注：只有子模块配置了继承的元素，才会真正的有效，否则maven是不会加载父模块中声明的元素。

pluginManagement和dependencyManagement相类似，它是用来进行插件管理的。

## 聚合和继承的关系
区别 ：
1、对于聚合模块来说，它知道有哪些被聚合的模块，但那些被聚合的模块不知道这个聚合模块的存在。
2、对于继承关系的父POM来说，它不知道有哪些子模块继承与它，但那些子模块都必须知道自己的父 POM是什么。

共同点 ：
1.聚合 POM与继承关系中的父POM的packaging都是pom
2.聚合模块与继承关系中的父模块除了 POM之外都没有实际的内容。
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/maven/pom/diff.jpg)
注：在现有的实际项目中一个 POM既是聚合POM，又是父 POM，这么做主要是为了方便。


# 参考文档
Maven POM
http://www.yiibai.com/maven/maven_pom.html

maven核心，pom.xml详解
http://blog.csdn.net/zhuxinhua/article/details/5788546


maven 配置篇 之pom.xml(一）
http://www.iteye.com/topic/41754


史上最全的maven pom.xml文件教程详解
http://blog.csdn.net/yaerfeng/article/details/26448417

Maven聚合与继承
http://chenzhou123520.iteye.com/blog/1582166

Maven详解之聚合与继承
http://blog.csdn.net/wanghantong/article/details/36427411
