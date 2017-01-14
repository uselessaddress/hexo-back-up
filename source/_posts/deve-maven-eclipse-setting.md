title: Eclipse配置Maven
date: 2014-02-28 10:10:24
tags:
- Maven
- Eclipse
categories: 设计开发
toc: true
---

# 前言
Eclipse的自带Maven，在一般情况下就够用了。但是，由于版本要求或者高级功能要求，我们不得不自己安装配置Maven。

# 下载安装
详见《Maven本地安装jar文件》

# Eclipse配置
Eclipse，Window，Preferences。
<!--more-->

1、Maven，Installations，Add，然后选择Maven的安装路径。
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/maven/eclipse/add.jpg)
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/maven/eclipse/installations.jpg)

2、Maven，User Settings，User Settings选择Maven下的setting.xml。
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/maven/eclipse/usersettings.jpg)

3、编辑settings.xml，释放`<localRepository></localRepository>`节点，中间加上本地Repository路径。
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/maven/eclipse/settings.jpg)

4、Java，Installed JREs，Edit，在Default VM arguments中设置
`-Dmaven.multiModuleProjectDirectory=$M2_HOME`

# 后记
在使用Eclipse自带Maven的过程中，如果出现了一些无法理解的错误，不妨试试自己配置一个Maven。