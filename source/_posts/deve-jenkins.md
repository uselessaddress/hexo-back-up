---
title: 自动部署工具Jenkins
toc: true
date: 2016-11-10 09:24:29
tags:
- 自动部署
- jenkins
- centos
categories: 设计开发
---
# 前言
为了方便部署项目，同时使前端开发者不用在本地部署后端项目，决定搭建一个服务器（CentOS7），安装Jenkins，用于自动部署项目。

<!--more-->

# 安装Java
jenkins需要java环境，所以，先安装java：
```
yum install java
java -version
```


# 安装Jenkins
1、添加jenkins源：
```
wget -O /etc/yum.repos.d/jenkins.repo http://jenkins-ci.org/redhat/jenkins.repo 
rpm --import http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key
```

2、安装jenkins：
```
yum install jenkins
```

3、启动jenkins：
```
service jenkins start
```

Jenkins安装目录：`/usr/lib/jenkins/`。
Jenkins配置文件：`/etc/sysconfig/jenkins`。

4、在浏览器输入centos地址：`http://192.168.56.101:8080`，即可看到jenkins页面。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/jenkins/start.jpg)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/jenkins/service.jpg)

# 安装SVN
1、安装SVN：
```
yum install subversion
svnserve --version
```

2、创建版本库：
```
mkdir -p /var/svn/svnrepos
svnadmin create /var/svn/svnrepos
```

3、进入conf目录（该svn版本库配置文件），`cd /var/svn/svnrepos/conf`。
- authz文件是权限控制文件
- passwd是帐号密码文件
- svnserve.conf SVN服务配置文件

4、设置帐号密码，`vi passwd`。
在[users]块中添加用户和密码，格式：帐号=密码，如voidking=woaixuexi。
```
### This file is an example password file for svnserve.
### Its format is similar to that of svnserve.conf. As shown in the
### example below it contains one section labelled [users].
### The name and password for each user follow, one account per line.

[users]
# harry = harryssecret
# sally = sallyssecret
voidking=woaixuexi
```

5、设置权限，`vi authz`。
在末尾添加如下代码：
```
[/]
voidking=rw
```
意思是版本库的根目录quwenzhe对其有读写权限。

6、修改svnserve.conf文件，`vi svnserve.conf`。
打开下面的几个注释：
```
anon-access = read #匿名用户可读
auth-access = write #授权用户可写
password-db = passwd #使用哪个文件作为账号文件
authz-db = authz #使用哪个文件作为权限文件
realm = /var/svn/svnrepos # 认证空间名，版本库所在目录
```

7、启动svn版本库，`svnserve -d -r /var/svn/svnrepos`。
（停止SVN命令，`killall svnserve`）

8、使用SVN
Windows上右键，SVN Checkout。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/jenkins/checkout.jpg)


# 使用Jenkins
1、新建项目
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/jenkins/new.jpg)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/jenkins/freeproject.jpg)

2、添加SVN
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/jenkins/svn.jpg)


3、安装插件

# 后记
最终，也没有设置好jenkins的php项目自动部署。不玩了，有些事情不是单纯的“努力”二字就能搞定，留个坑，以后填。

# 书签
CentOS 安装 Jenkins
https://segmentfault.com/a/1190000004639325

CentOS 7搭建SVN服务器
http://www.centoscn.com/CentosServer/ftp/2015/0622/5708.html

Jenkins常用插件之Deploy Plugin
http://blog.csdn.net/jiang1986829/article/details/51173251

Jenkins详细安装与构建部署使用教程
http://blog.csdn.net/evankaka/article/details/50518959

jenkins plugins
http://updates.jenkins-ci.org/download/plugins/
