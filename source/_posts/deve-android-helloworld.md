title: Android开发——helloworld
toc: true
date: 2015-07-13 21:41:25
tags:
- android
categories: 设计开发
---
# 前言
Android的学习，拖拉了太久太久，这个夏天，搞个作品出来吧！从0开始，用博客记录进度！

# 工具
之前使用adt，断断续续学习过安卓开发。今年谷歌宣布Android Studio将取代Eclipse，正式成为官方集成开发软件，中止对后者支持。小编也紧跟潮流，换用Android Studio（以后简称AS）。

<!--more-->

# 步骤
## 下载安装
1、Android Studio中文社区
http://www.android-studio.org/

2、百度“Android Studio”，从百度软件中心下载即可。

## 新建工程
打开AS，选择工作路径，输入工程名（首字母习惯大写），选择sdk版本，选择一个模板，finish即可。（和Eclipse非常类似）

## 新建模拟器
点击AVD Manager，Create Virtual Device，选择一款模拟器，next，输入模拟器名称，finish即可。

## 运行
点击Run 'app'，Choose Device，OK。正常情况下，就可以看到经典的“helloworld”了。小编在启动AVD的时候出现了端口冲突，下面是解决办法。

# 端口冲突
## 详细描述
启动AVD报错：
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/helloworld/error.jpg)

## 解决办法
### 查看端口
`netstat -ano|findstr "5037"`
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/helloworld/5037.jpg)
看到PID为20968。

### kill进程
ctrl+shift+esc，打开任务管理器，详细信息，找到PID为20968的进程，结束进程。
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/helloworld/任务管理器.jpg)

# 运行结果
解决完上述错误，再次Run 'app'，就可以看到：
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/helloworld/helloworld.jpg)

# 后记
安卓开发，就当做娱乐来搞吧。白天复习备考，晚上搞搞开发，也是一件很有意思的事情。贵在坚持，不出作品不止！