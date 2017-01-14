---
title: 在Ubuntu16.04上安装Hadoop
toc: true
date: 2016-11-18 14:16:03
tags:
- Ubuntu
- Hadoop
- 大数据
categories: 设计开发
---
# 前言
专业方向，选择了大数据，那就在这方面深入研究一下。什么是大数据？正如字面意思，大量的数据。举个例子，Mysql的一张表里存了1万条数据，查询没问题；100万条数据，查询也没问题；那么，1亿条数据？100亿条数据？更大的数据？

> 大数据科学家JohnRauser提到一个简单的定义：大数据就是任何超过了一台计算机处理能力的庞大数据量。

为了处理大量的数据，我们必须找到更好的办法。谷歌经过研究，发表了一些关于大数据解决方案的论文，涉及MapReduce、BigTable、GFS等。但是，谷歌开发的大数据处理平台，并没有开源。一些勤奋的同学根据谷歌发表的论文，搞出了Hadoop平台，后来成为一个主流的大数据处理平台，也就是接下来一段时间小编要学习的平台。

<!--more-->

# 大数据分析
1、可视化分析
大数据分析的使用者有大数据分析专家，同时还有普通用户，但是他们二者对于大数据分析最基本的要求就是可视化分析，因为可视化分析能够直观的呈现大数据特点，同时能够非常容易被读者所接受，就如同看图说话一样简单明了。

2、数据挖掘算法
大数据分析的理论核心就是数据挖掘算法，各种数据挖掘的算法基于不同的数据类型和格式才能更加科学的呈现出数据本身具备的特点，也正是因为这些被全世界统计学家所公认的各种统计方法（可以称之为真理）才能深入数据内部，挖掘出公认的价值。另外一个方面也是因为有这些数据挖掘的算法才能更快速的处理大数据，如果一个算法得花上好几年才能得出结论，那大数据的价值也就无从说起了。

3、预测性分析能力
大数据分析最终要的应用领域之一就是预测性分析，从大数据中挖掘出特点，通过科学的建立模型，之后便可以通过模型带入新的数据，从而预测未来的数据。

4、数据质量和数据管理
大数据分析离不开数据质量和数据管理，高质量的数据和有效的数据管理，无论是在学术研究还是在商业应用领域，都能够保证分析结果的真实和有价值。

好了，废话到此结束，下面开始本文重点。

# 安装步骤
## 安装JDK
直接安装jdk7，会提示E: Invalid operation openjdk-7-jdk。所以，要先更新安装源。
```
add-apt-repository ppa:openjdk-r/ppa  
apt-get update   
apt-get install openjdk-7-jdk
```

安装好之后，`javac`，测试安装是否成功。

配置JAVA_HOME和JRE_HOME，`vim /etc/profile`，添加：
```
export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
export JRE_HOME=$JAVA_HOME/jre
export CLASSPATH=$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
```
然后使配置文件生效，`source /etc/profile`。

## 安装Hadoop
慕课教程中选择安装1.2.1，虽然有点老，但是学习足够了，小编也安装这一版，方便接下来的学习。

1、下载解压
```
wget http://mirror.bit.edu.cn/apache/hadoop/common/hadoop-1.2.1/hadoop-1.2.1.tar.gz
mv hadoop-1.2.1.tar.gz /opt/
cd /opt/
tar -zxvf hadoop-1.2.1.tar.gz
```

2、修改hadoop-env.sh
```
cd hadoop-1.2.1/conf/
vim hadoop-env.sh
```

主要修改JAVA_HOME如下：
```
# The java implementation to use.  Required.
export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
```

3、修改core-site.xml，内容如下：
```
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<!-- Put site-specific property overrides in this file. -->

<configuration>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/hadoop</value>
    </property>

    <property>
        <name>dfs.name.dir</name>
        <value>/hadoop/name</value>
    </property>

    <property>
        <name>fs.default.name</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```

4、修改hdfs-site.xml，内容如下：
```
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<!-- Put site-specific property overrides in this file. -->

<configuration>
    <property>
        <name>dfs.data.dir</name>
        <value>/hadoop/data</value>
    </property>
</configuration>

```

5、修改mapred-site.xml，内容如下：
```
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<!-- Put site-specific property overrides in this file. -->

<configuration>
    <property>
        <name>mapred.job.tracker</name>
        <value>localhost:9001</value>
    </property>
</configuration>

```

6、修改/etc/profile，修改PATH如下：
```
export HADOOP_HOME=/opt/hadoop-1.2.1
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$HADOOP_HOME/bin:$PATH
```

生效，`source /etc/profile`。

7、测试，`hadoop`，如果出现COMMAND提示，则表明安装配置成功。

8、在第7步测试时，出现“Warning: $HADOOP_HOME is deprecated.”，这是因为新版的hadoop废弃掉了HADOOP_HOME这个变量。若要除去这个警告，要么换用HADOOP_PREFIX，要么在hadoop-env.sh添加一句：
```
export HADOOP_HOME_WARN_SUPPRESS=1
```

9、namenode格式化，`hadoop namenode -format`。

10、启动hadoop，`start-all.sh`。

11、检查是否启动成功，`jps`。如果看到下图的进程，则表明启动成功。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/hadoop-on-ubuntu/jps.jpg)

12、查看hadoop下有哪些文件，`hadoop fs -ls /`。

# 后记
至此，hadoop环境安装配置完成，接下里可以愉快地进行大数据分析了！

# 书签
Hadoop大数据平台架构与实践--基础篇
http://www.imooc.com/learn/391

阿里云大数据教程：
https://yq.aliyun.com/edu/lessonTagSearch/cid_695-tagid_14065?spm=5176.8142029.388261.235.rtkpxj

云计算从入门到实践：
https://yq.aliyun.com/edu/lessonTagSearch/cid_14061-tagid_15114?spm=5176.100242.0.0.fU7F9h

什么是大数据？
https://www.zhihu.com/question/23896161

Warning: $HADOOP_HOME is deprecated.的原因以及解决方法。
http://blog.csdn.net/jsj568261496/article/details/10175209