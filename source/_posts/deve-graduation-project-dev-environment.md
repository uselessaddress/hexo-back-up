title: 毕业设计开发环境的搭建
toc: true
date: 2016-01-05 17:36:06
tags:
- node.js
- 毕设
categories: 设计开发
---

# Webstorm
访问Jetbrains的Webstorm官网：http://www.jetbrains.com/webstorm/ ，下载Webstorm，双击安装。

# Git
访问Git官网：https://git-scm.com/download/ ，下载Git，双击安装。需要注意的是，在Select Components界面，点选Simple context menu，方便以后直接右键打开Git bash。

# Node
访问Node官网：https://nodejs.org/en/ ，下载稳定版的Node，双击安装。
安装之后，win+R，cmd，打开命令提示符界面，输入`node --version`，如果可以看到版本号，就说明安装成功。

# Express
新建文件夹nodeforum，在文件夹下打开Git bash，`npm install express`。

# 测试运行
在nodeforum下新建文件app.js，内容如下：
```
var express = require('express');
var app = express();

app.get('/', function (req, res) {
   res.send('Hello World');
});

var server = app.listen(3000, function () {
  console.log('应用启动在port:'+3000)
});
```
在Git bash下输入：`node app.js`。然后访问http://localhost:3000 ，看到“Hello World”，就表明express安装成功！

# MongoDB
访问MongoDB官网：https://www.mongodb.com/download-center#community 
下载MongoDB，双击安装。之后，打开命令提示符界面，输入`mongo`，进入MongoDB shell，如果可以进入，说明安装成功。

# 项目结构
│  .gitignore                              
│  app.js                          
│  bower.json                                                           
│  config.js                      
│  package.json                                                  
│  README.md                    
│  socket-event.js             
│  web-router.js                        
├─bower_components                                      
├─controllers                                                          
├─models                         
├─node_modules                                             
├─public                             
│  ├─css                                                           
│  ├─img                                                    
│  ├─js                                                                                    
│  └─scss                                                                                              
├─schemas                            
└─views

- .gitignore内写的是不需要上传到Git服务器的文件。
- app.js是程序的入口。
- bower.json是Bower配置文件，声明了一系列与前端包有关的内容。
- config.js声明了一些全局配置。
- package.json是Npm的配置文件，声明了一系列与Node依赖包有关的内容。
- README.md是关于整个项目的说明。
- socket-event.js里定义了一个函数，用来处理socket.io的服务器端。
- web-router.js里写的是路由控制。
- bower_components文件夹里是开发过程中需要依赖的一些前端软件包。
- controllers文件夹里是处理请求，以及控制页面跳转的一些文件。
- models文件夹里是一些Mongoose建立的Model文件。
- node_modules文件夹里是开发过程中需要依赖的一些Node软件包。
- public文件夹里存放自己写的scss文件、js文件、需要用到图片文件。
- schemas文件夹里是一些Mongoose建立的Schema文件。
- views文件里存放的是前端模板文件。

