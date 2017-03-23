---
title: "CentOS7搭建Atlassian Jira"
toc: true
date: 2017-03-21 09:00:00
tags:
- centos
- jira
categories: 设计开发
---
# 前言
Jira是Atlassian公司出品的项目与事务跟踪工具，被广泛应用于缺陷跟踪（bug管理）、客户服务、需求收集、流程审批、任务跟踪、项目跟踪和敏捷管理等工作领域。

写完了《自动部署工具Jenkins》和《CentOS7搭建Confluence Wiki》，感觉有些缺憾，决定把Jira的搭建方法也记录一下。

<!--more-->

# 准备
## 下载软件包
开始搭建Jira前，需要下载一些[软件包]()。
- atlassian-jira-software-7.2.2-x64
- JIRA Core-7.2.1-language-pack-zh_CN
- mysql-connector-java-5.1.39-bin
- atlassian-extras-3.1.2

## 安装配置java
```bash
yum install java
java -version
```

## 安装配置mysql
1、安装mysql后，登录mysql控制台，执行如下命令：
```
create database jira default character set utf8;
grant all on jira.* to 'jirauser'@'%' identified by 'jirapasswd' with grant option;
grant all on jira.* to 'jirauser'@localhost identified by 'jirapasswd' with grant option;
flush privileges;
```

2、进入/usr/local/mysql文件夹，在my.cnf中添加：
```
binlog_format=mixed
```

3、重启mysql
```
service mysqld stop
service mysqld start
```

## 关闭防火墙
```bash
systemctl stop firewalld.service
```


# 详细步骤
## 安装jira
1、使用xftp，上传atlassian-jira-software-7.2.2-x64.bin到`/root`文件夹。

2、上传完成后，执行命令：
```bash
chmod 755 atlassian-jira-software-7.2.2-x64.bin
./atlassian-jira-software-7.2.2-x64.bin
```
jira默认安装到`/opt/atlassian/jira`和`/var/atlassian/application-data/jira`目录下，并且jira监听的端口是8080。

3、jira的主要配置文件，是`/opt/atlassian/jira/conf/server.xml`。

4、此时不要测试访问，切记。

## 破解jira
1、关闭jira
```
/etc/init.d/jira stop
```

2、把atlassian-extras-3.1.2.jar和mysql-connector-java-5.1.39-bin.jar两个文件上传到`/opt/atlassian/jira/atlassian-jira/WEB-INF/lib/`里。
其中atlassian-extras-3.1.2.jar是用来替换原来的atlassian-extras-3.1.2.jar文件，用作破解jira系统的；mysql-connector-java-5.1.39-bin.jar是用来连接mysql数据库的驱动软件包。

3、启动jira
```
/etc/init.d/jira start
```

4、测试访问，假设CentOS7的ip地址为`192.168.56.101`，那么在浏览器输入`http://192.168.56.101:8080`，即可看到jira的安装页面。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/setup.jpg?imageView2/0/w/700)

5、选择I'll set it up myself，然后“Next”，进入数据库设置页面。

6、选择MySQL数据库，输入**安装配置mysql**中设置的账号和密码。点击“Test Connection”，确认数据库连接是否成功。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/database.jpg?imageView2/0/w/700) 

7、点击“Next”，向数据库写入数据，这一步花费时间较长，请耐心等待。
数据库的配置文件，是`/var/atlassian/application-data/jira/dbconfig.xml`


8、报错。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/failed.jpg?imageView2/0/w/700)

9、忽略以上错误，重启jira服务。
```
/etc/init.d/jira start
/etc/init.d/jira stop
```

10、再次访问`http://192.168.56.101:8080`，进入jira配置页面。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/app.jpg?imageView2/0/w/700)

## 试用jira
1、选择Private模式，在这个模式下，用户需要由管理员创建。而在Public模式下，用户可以自己进行注册。

2、点击generate a JIRA trial license，登录atlassian，获取试用license。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/key.jpg?imageView2/0/w/700)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/license.jpg?imageView2/0/w/700)

3、获取license后在[这个页面](https://my.atlassian.com/product)查看。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/license2.jpg?imageView2/0/w/700)

4、拷贝license，粘贴到jira配置页面，“Next”。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/key2.jpg?imageView2/0/w/700)

5、再次报错，不过不要放弃。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/error.jpg?imageView2/0/w/700)

6、重启，再次拷贝license，粘贴到jira配置页面，“Next”。

## 配置管理员
1、上一步后，成功进入管理员配置页。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/admin.jpg?imageView2/0/w/700)

2、配置管理员后，下一步进入邮件设置页面。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/email.jpg?imageView2/0/w/700)

3、点击“Finish”，进入欢迎页面。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/welcome.jpg?imageView2/0/w/700)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/welcome2.jpg?imageView2/0/w/700)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/welcome3.jpg?imageView2/0/w/700)

4、创建新项目，选择Scrum software development，“Next”。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/project.jpg?imageView2/0/w/700)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/project2.jpg?imageView2/0/w/700)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/project3.jpg?imageView2/0/w/700)

5、稍等片刻，便会跳转到管理页面。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/success.jpg?imageView2/0/w/700)

## 查看破解
点击右上角齿轮形状的管理图标，选择“Applications”，查看破解信息。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/crack.jpg?imageView2/0/w/700)
可以看到，到期日期是2033年2月8日，破解成功。

## 汉化
1、点击右上角齿轮形状的管理图标，选择“Add-ons”，再选择“Manage add-ons”。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/add-ons.jpg?imageView2/0/w/700)

2、点击“Upload add-on”，选择上传JIRA Core-7.2.1-language-pack-zh_CN.jar。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/lan.jpg?imageView2/0/w/700)

3、依次点击“System”，“Edit Settings”。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/setting.jpg?imageView2/0/w/700)

4、找到Internationalization，修改Indexing language和Default language为中文，修改Default user time zone为亚洲上海。然后点击页面底部的“Update”按钮。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/chinese.jpg?imageView2/0/w/700)

# 后记
至此，jira破解和汉化完成。虽然中途有些波折，但总归是安装成功了，吼吼！
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-atlassian-jira/finish.jpg?imageView2/0/w/700)


# 书签
烂泥：jira7.2安装、中文及破解 
http://blog.chinaunix.net/uid-21710354-id-5756990.html

jira下载页
https://www.atlassian.com/software/jira/download

jira语言包下载页
https://translations.atlassian.com/dashboard/download?lang=zh_CN#/JIRA Core/7.2.1

