title: OpenShift使用说明
date: 2013-11-03 17:34:38
tags:
- OpenShift
categories: 设计开发
---

## OpenShift使用说明
### 前言
小编的第一个网站，使用WordPress，搭建在OpenShift上面。OpenShift出身名门，无论从其稳定性还是功能性来说，都没有给RedHat丢人。
官网：https://www.openshift.com

### 常用命令
云服务器总是出问题，可以来这里试试openshift的命令来检查测试

gem install rhc安装rhc

gem update rhc更新rhc版本

rhc-chk检测本地环境配置

rhc-user-info显示用户信息
<!--more-->
rhc-create-domain：创建个人域

rhc-create-app创建应用

rhc-snapshot应用备份

rhc-tail-files查看应用日志

openshift: rhc setup登录

rhc app create -a  app_name -t php-5.3创建php应用

rhc app create -a wordpress -t php-5.3创建wordpress

wordpress添加mysql支持：rhc app cartridge add -a wordpress -c mysql-5.1
创建一个自定义应用：rhc-create-app -a  app_name -t diy-0.1
创建命名空间：rhc domain create -n mydomain -l rhlogin
创建phpmyadmin：rhc app cartridge add -a  app_name -c phpmyadmin-3.4
绑定域名:rhc alias add appname domian
删除绑定域名: rhc alias removeappname domian
如果你是创建一个wordpress，需要MongoDB支持，可以输入如下命令：rhc app cartridge add -a wordpress -c mongodb-2.2

启动应用程序：ctl_app start
停止应用程序：ctl_app stop
重启应用程序：ctl_app restart
查看应用程序：ctl_app status

启动应用程序：ctl_all start
停止应用程序：ctl_all stop
重启应用程序：ctl_all restart
查看应用程序：ctl_all status

### 参考文档

------
Red Hat老用户的OpenShift初体验
http://os.51cto.com/art/201211/365219.htm

------
OpenShift红帽免费云空间申请、WordPress安装(图文教程)
http://www.i7086.com/openshifthongmaomianfeiyunkongjianshenqing

------
RedHat Openshift 搭建个人博客(wordpress)指南
http://hi.baidu.com/liuhangbin/item/67772b2ddf56030c72863e10

------
OpenShift Redhat免费空间SSH登录管理和使用:下载文件安装程序和应用
http://www.freehao123.com/openshift-redhat-ssh/

------
OpenShift redhat推出PaaS云计算应用平台支持PHP、Java、MySQL
http://www.freehao123.com/openshift-redhat/

------
openshift免费空间绑定顶级域名
http://www.360doc.com/content/13/0505/15/11978685_283129694.shtml

------
友好面对开发者 OpenShift加入更多新元素
http://verdureorange.blog.51cto.com/632758/1092430/

------
OpenShift免费空间安装PhpMyadmin及SQL数据库管理（图文教程）
http://www.i7086.com/openshiftanzhuangphpmyadmin

------
OpenShift推出收费业务和解决OpenShift空间打不开和SSH无法连接
http://www.freehao123.com/openshift-kongjian/

------
