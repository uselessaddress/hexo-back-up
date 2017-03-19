---
title: "CentOS7搭建Confluence Wiki"
toc: true
date: 2017-03-19 12:00:00
tags:
- centos
- wiki
categories: 设计开发
---
# 前言
在艾佳生活实习时，有三款团队协作系统特别喜欢：Wiki、Jira和Jenkins。对于Jenkins的搭建，之前《自动部署工具Jenkins》有过记录。这次，搭建一个Wiki，作为知识管理的工具，实现团队成员之间的协作和知识共享。

<!--more-->

# 准备
## 下载软件包
开始搭建Wiki前，需要下载一些[软件包](http://pan.baidu.com/s/1c1Z0lqw)。
- confluence5.6.6
- Confluence-5.6.6-language-pack-zh_CN
- mysql-connector
- confluence_keygen

## 安装配置java
```bash
yum install java
java -version
```

## 安装配置mysql
1、安装mysql后，登录mysql控制台，执行如下命令：
```
create database confluence default character set utf8;
grant all on confluence.* to 'confluenceuser'@'%' identified by 'confluencepasswd' with grant option;
grant all on confluence.* to 'confluenceuser'@localhost identified by 'confluencepasswd' with grant option;
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
## 安装confluence
1、使用xftp，上传atlassian-confluence-5.6.6-x64.bin到`/root`文件夹。

2、上传完成后，执行命令：
```bash
chmod 755 atlassian-confluence-5.6.6-x64.bin
./atlassian-confluence-5.6.6-x64.bin
```
confluence默认安装到`/opt/atlassian/confluence`和`/var/atlassian/application-data/confluence`目录下，并且confluence监听的端口是8090。

3、confluence的主要配置文件，存放在`/opt/atlassian/confluence/conf/server.xml`文件中。

4、测试访问，假设CentOS7的ip地址为`192.168.56.101`，那么在浏览器输入`http://192.168.56.101:8089`，即可看到Confluence的欢迎界面。
![欢迎](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-confluence-wiki/welcome.jpg?imageView2/0/w/700 "欢迎")

## 破解confluence
1、点击“Start setup”，看到如下界面。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-confluence-wiki/setup.jpg?imageView2/0/w/700)

2、复制Server ID并保存，然后关闭confluence。
```bash
/etc/init.d/confluence stop
```

3、从`/opt/atlassian/confluence/confluence/WEB-INF/lib`中，拷贝atlassian-extras-decoder-v2-3.2.jar到windows，并重命名为atlassian-extras-2.4.jar。

4、在windows下，生成License Key。
```
java -jar confluence_keygen.jar
```
把第二步中复制的Server ID粘贴进去，然后点击“.gen!”，保存生成的key。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-confluence-wiki/key.jpg?imageView2/0/w/700)

5、打补丁。点击“.patch!”，选择第3步中重命名的atlassian-extras-2.4.jar，会生成新的atlassian-extras-2.4.jar。

6、上传新的atlassian-extras-2.4.jar、Confluence-5.6.6-language-pack-zh_CN.jar、mysql-connector-java-5.1.39-bin.jar到`/opt/atlassian/confluence/confluence/WEB-INF/lib`，并且删除atlassian-extras-decoder-v2-3.2.jar。

5、启动confluence
```bash
/etc/init.d/confluence start
```

7、把生成的key复制粘贴到License Key框中，点击“Next”，如果顺利进入选择数据库页面，说明破解成功。

## 配置数据库
1、数据库选择MySQL，然后点击“External Database”，进入数据库配置页面。

2、点击“Direct JDBC”，User Name和Password填写**安装配置mysql**中设置的用户名和密码。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-confluence-wiki/database.jpg?imageView2/0/w/700)

3、点击“Next”，这一步花费时间较长，请耐心等待。数据写入成功，进入如下页面。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-confluence-wiki/content.jpg?imageView2/0/w/700)


4、第3步如果报错，请检查mysql数据库配置，然后卸载后重新安装，卸载命令如下。
```bash
/etc/init.d/confluence stop
cd /opt/atlassian/confluence/
./uninstall
```
或者：
```bash
/etc/init.d/confluence stop
rm -rf /opt/atlassian/
rm -rf /var/atlassian/
```

## 配置管理员
初始化一个样例站点，根据提示进行配置。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-confluence-wiki/config.jpg?imageView2/0/w/700)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-confluence-wiki/config2.jpg?imageView2/0/w/700)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-confluence-wiki/success.jpg?imageView2/0/w/700)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/centos-confluence-wiki/success2.jpg?imageView2/0/w/700)


# 书签
wiki系统confluence5.6.6安装、中文、破解及迁移
http://www.ilanni.com/?p=11989

confluence wiki搭建使用
http://www.cnblogs.com/guigujun/p/6137673.html


