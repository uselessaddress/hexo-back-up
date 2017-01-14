---
title: Node基础开发框架
toc: true
date: 2016-07-14 14:15:26
tags:
- node
categories: 设计开发
---
# 前言
测试新功能，开发新项目，按照本人的懒惰程度推算，八成会在原有项目的基础上开发。既然如此，抽出来一个node基础框架，似乎是一个很好的想法。
本框架保留了毕设的主体代码，删除了一些无关代码，并且会继续增减代码，逐渐完善。

<!--more-->

# nodebase
a node framework designed for myself  
version: v1.0.0  
url: https://github.com/voidking/nodebase.git  

# 作者相关
author: haojin(voidking)  
e-mail: voidking@qq.com    
site: http://www.voidking.com   

# 开发环境说明
1、安装5.6.0以上Node 
2、安装mongodb，不要配置密码
3、安装sass
4、全局安装bower
5、全局安装supervisor
6、全局安装node-inspector
7、全局安装puer
8、运行前执行命令
```
npm install
bower install
```


# node实时调试命令
```
node-inspector --web-port=8888
node --debug app.js
```

# 运行命令
`node app.js`，访问`http://localhost`








