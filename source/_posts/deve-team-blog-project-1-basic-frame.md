title: 团队博客项目（一）——基础框架的搭建
date: 2014-10-22 18:33:47
tags: [node.js,项目]
categories: 设计开发
---
### 开发环境
下载安装[node.js](http://nodejs.org/)。安装完成后，进入命令提示符界面，输入`node --version`，如果能出现node的版本号，则说明安装成功。
### 开发工具的选用
java程序开发，我喜欢eclipse；node.js程序开发，我选择的工具是webstorm。至于软件下载破解啥的，都是基本功，在此不赘述。
### 生成基本目录和文件
1、打开webstorm，file -> new project

2、输入project name，选择location，project type选择node.js express app

3、node.js interpreter和npm excutable的位置默认就好（自动读取），version of express-generator选择最新版4.9.0，template engine选择ejs，css engine选择plain css。

4、ok，this window，如果接下来弹出configure node.js v0.10.31 core modules sources，直接点cancel就好，用不到。

5、好了，项目基础框架搭建好了，看下效果：
![][1]


  [1]: http://voidking.qiniudn.com/@/imgs/%E5%9B%A2%E9%98%9F%E5%8D%9A%E5%AE%A2%E9%A1%B9%E7%9B%AE/1.1.jpg
