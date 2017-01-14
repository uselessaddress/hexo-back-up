title: Maven概述
date: 2014-02-24 10:10:24
tags:
- Maven
categories: 设计开发
toc: true
---

# Maven是什么

Maven这个单词来自于意第绪语，意为知识的积累，最早在Jakata Turbine项目中它开始被用来试图简化构建过程。

Maven是基于项目对象模型(POM，Project Object Model)，可以通过一小段描述信息来管理项目的构建，报告和文档的软件项目管理工具。

# Maven能做什么
Maven采用了一种被称之为POM的概念来管理项目，所有的项目配置信息都被定义在一个叫做pom.xml的文件中，通过该文件，Maven可以管理项目的整个声明周期，包括编译，构建，测试，发布，报告等等。目前Apache下绝大多数项目都已经采用Maven进行管理。而Maven本身还支持多种插件，可以方便更灵活的控制项目。

小编使用Maven，最深切的体会是，不用手动导入jar包，写写配置文件就搞定了，真方便！
<!--more-->

# jar包依赖

使用maven不需要上网单独下载jar包，只需要在配置文件pom.xml中配置jar包的依赖关系，就可以自动的下载jar包到我们的项目中。这样，别人开发或者使用这个工程时，不需要来回的拷贝jar包，只需要复制这个pom.xml就可以自动的下载这些jar包。

而且，我们自己下载jar包，还有可能造成版本的不一致，这样在协同开发的过程中就有可能造成代码运行的不一致。通过使用maven精确的匹配jar包，就不会出现这种问题了。


# 项目坐标

Maven通过特定的标识来定义项目名称，这样既可以唯一的匹配其他的jar包，也可以通过发布，使别人能使用自己的发布产品。这个标识就被叫做坐标，长的其实很普通，就是简单的xml而已：
```
<groupId>com.test</groupId>
<artifactId>maventest</artifactId>
<version>0.0.1-SNAPSHOT</version>
<packaging>jar</packaging>

<name>maventest</name>
<url>http://maven.apache.org</url>
```
groupId：所述的项目名称，由于有的项目并不是一个jar包构成的，而是由很多的jar包组成的。因此这个groupId就是整个项目的名称。

artifactId：包的名称。

version：版本号。

packaging：包的类型，一般都是jar，也可以是war之类的。如果不填，默认就是jar。

name和url，一个是名称，一个是maven的地址。主要就是上面的几个参数。

当想要依赖什么jar的时候就可以通过下面的方式依赖：
```
<dependencies>
    <dependency>
		<groupId>junit</groupId>
		<artifactId>junit</artifactId>
		<version>3.8.1</version>
		<scope>test</scope>
    </dependency>
</dependencies>
```

各个属性的内容基本上都是一样的。

这里要注意的是jar包的命名规则：

artifactId-version\[-classifier\].packaging

比如上面的pom.xml生成的jar包名字就是：maventest-0.0.1-SNAPSHOT.jar。

这里的classifier是可选的，但是有的项目可能还需要导出附属的一些文件，如javadoc，source等等，那么这个地方就需要配置一个字符串。一般都是JDKXXX之类的。

# 测试驱动

Maven是测试驱动的开发思路，因此工程创建初期，就包含两个文件夹，main和test。一个用于放置开发的java文件，一个用于写test单元测试。这样每次开发的时候，提前设计单元测试，就能帮助减少BUG。

# Maven生命周期
Maven有三套相互独立的生命周期，请注意这里说的是“三套”，而且“相互独立”。这三套生命周期分别是：

- Clean Lifecycle 在进行真正的构建之前进行一些清理工作。
- Default Lifecycle 构建的核心部分，编译，测试，打包，部署等等。
- Site Lifecycle 生成项目报告，站点，发布站点。

再次强调一下它们是相互独立的，你可以仅仅调用clean来清理工作目录，仅仅调用site来生成站点。当然你也可以直接运行 mvn clean install site 运行所有这三套生命周期。

## Clean
每套生命周期都由一组阶段(Phase)组成，我们平时在命令行输入的命令总会对应于一个特定的阶段。比如，运行mvn clean ，这个的clean是Clean生命周期的一个阶段。Clean生命周期一共包含了三个阶段：

- pre-clean  执行一些需要在clean之前完成的工作
- clean  移除所有上一次构建生成的文件
- post-clean  执行一些需要在clean之后立刻完成的工作

mvn clean中的clean就是上面的clean，在一个生命周期中，运行某个阶段的时候，它之前的所有阶段都会被运行，也就是说，mvn clean 等同于 mvn pre-clean clean ，如果我们运行 mvn post-clean ，那么 pre-clean，clean 都会被运行。这是Maven很重要的一个规则，可以大大简化命令行的输入。

## Site
Site生命周期的各个阶段：

- pre-site     执行一些需要在生成站点文档之前完成的工作
- site    生成项目的站点文档
- post-site     执行一些需要在生成站点文档之后完成的工作，并且为部署做准备
- site-deploy     将生成的站点文档部署到特定的服务器上
这里经常用到的是site阶段和site-deploy阶段，用以生成和发布Maven站点，这可是Maven相当强大的功能，Manager比较喜欢，文档及统计数据自动生成，很好看。

## Default
Maven的最重要的是Default生命周期，绝大部分工作都发生在这个生命周期中，这里，我只解释一些比较重要和常用的阶段：

- validate
- generate-sources
- process-sources
- generate-resources
- process-resources     复制并处理资源文件，至目标目录，准备打包。
- compile     编译项目的源代码。
- process-classes
- generate-test-sources 
- process-test-sources 
- generate-test-resources
- process-test-resources     复制并处理资源文件，至目标测试目录。
- test-compile     编译测试源代码。
- process-test-classes
- test     使用合适的单元测试框架运行测试。这些测试代码不会被打包或部署。
- prepare-package
- package     接受编译好的代码，打包成可发布的格式，如 JAR 。
- pre-integration-test
- integration-test
- post-integration-test
- verify
- install     将包安装至本地仓库，以让其它项目依赖。
- deploy     将最终的包复制到远程的仓库，以让其它开发人员与项目共享。

基本上，根据名称我们就能猜出每个阶段的用途，关于其它阶段的解释，请参考 http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html
 
记住，运行任何一个阶段的时候，它前面的所有阶段都会被运行，这也就是为什么我们运行mvn install 的时候，代码会被编译，测试，打包。


# Maven常用命令
- mvn -h
不会用时，可寻求帮助。

- mvn archetype:create -DgroupId=packageName -DartifactId=projectName  
创建Maven的普通java项目

- mvn archetype:create -DgroupId=packageName -DartifactId=webappName -DarchetypeArtifactId=maven-archetype-webapp
创建Maven的Web项目

- mvn compile 
编译源代码

- mvn test-compile    
编译测试代码

- mvn test
运行测试

- mvn install
在本地Repository中安装jar

- mvn clean install

- mvn jetty:run 
调用 Jetty 插件的 Run 目标在 Jetty Servlet 容器中启动 web 应用 




# 学习资料
官网
http://maven.apache.org

孔浩Maven视频教程：
http://www.icoolxue.com/album/show/45

Maven教程
http://www.yiibai.com/maven/

maven pom.xml文件详解
http://www.blogjava.net/hellxoul/archive/2013/05/16/399345.html
http://www.zuidaima.com/share/1781583829978112.htm

Maven那点事儿（Eclipse版）
http://www.cnblogs.com/xing901022/p/4170248.html

Maven生命周期详解
http://juvenshun.iteye.com/blog/213959

mvn常用命令
http://www.cnblogs.com/holly/archive/2013/06/15/3137041.html
http://www.cnblogs.com/phoebus0501/archive/2011/05/10/2042511.html





