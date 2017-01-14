---
title: Hadoop单词计数
toc: true
date: 2016-11-24 20:00:14
tags:
- hadoop
categories: 设计开发
---
# 前言
上文中，已经搭建好了hadoop平台。接下来，小编利用hadoop来实现单词的计数的功能，视频教程参见慕课网Kit_Ren同学的《Hadoop大数据平台架构与实践——基础篇》。

要求：计算文件中出现每个单词的频数，输入结果按照字母顺序进行排序。
输入：
```
hello world bye world
hello hadoop bye hadoop
bye hadoop hello hadoop
```
输出：
```
bye     3
hello   3
hadoop  4
world   2
```

<!--more-->

# MapReduce过程
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/hadoop-wordcount/map.jpg)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/hadoop-wordcount/reduce.jpg)

# 编译打包和运行
代码从Kit_Ren同学那里下载，没有做修改。

视频教程中，代码是在ubuntu下编译运行的。小编打算把编译打包过程放在本地，运行放在ubuntu。

1、首先，使用xftp，把ubuntu中/opt/hadoop-1.2.1文件夹下和/opt/hadoop-1.2.1/lib文件夹下所有的jar包拷贝到本地。

2、引入jar包后，运行代码，失败，“Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: 0”。这时，到java项目的bin文件夹下寻找WordCount.class、WordCount\$WordCountMap.class、WordCount$Reduce.class。

3、打包三个.class文件为一个jar文件，`jar -cvf wordcount.jar *.class`。

4、拷贝第3步中的wordcount.jar文件到ubuntu的任意文件夹，假设为/home/voidking/hadoop。

5、在/home/voidking/hadoop中新建两个文件，file1和file2，内容分别为：
```
hello world bye world
hello hadoop bye hadoop
bye hadoop hello hadoop
```

```
hello world bye world
system hello hadoop
```

6、把file1和file2放到hdfs中，`hadoop fs -mkdir wordcount_input`，`hadoop fs -put file* wordcount_input/`。

7、执行jar包，`hadoop jar wordcount.jar WordCount wordcount_input wordcount_output`。

至此，理论上应该成功，但是报错了。
Exception in thread "main" java.lang.UnsupportedClassVersionError: WordCount : Unsupported major.minor version 52.0

------

也许是因为本机是java1.8，而ubuntu是java1.7。好吧，重来，完全按照视频教程。

1、上传WordCount.java文件到ubuntu的/home/voidking/hadoop文件夹。

2、编译java文件，`javac -classpath /opt/hadoop-1.2.1/hadoop-core-1.2.1.jar:/opt/hadoop-1.2.1/lib/commons-cli-1.2.jar WordCount.java`

3、打包三个.class文件为一个jar文件，`jar -cvf wordcount.jar *.class`。

4、在/home/voidking/hadoop中新建两个文件，file1和file2，内容同上。

5、把file1和file2放到hdfs中，`hadoop fs -mkdir wordcount_input`，`hadoop fs -put file* wordcount_input/`。

6、执行jar包，`hadoop jar wordcount.jar WordCount wordcount_input wordcount_output`。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/hadoop-wordcount/execute.jpg)

7、查看结果，`hadoop fs -ls wordcount_output`，`hadoop fs -cat wordcount_output/part-r-00000`。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/hadoop-wordcount/result.jpg)

# 源码分享
https://github.com/voidking/hadoop-start.git

