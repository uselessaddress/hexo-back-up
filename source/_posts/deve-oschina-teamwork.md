---
title: 开源中国团队协作开发平台
toc: true
date: 2016-08-28 22:00:11
tags:
- wiki
- jira
- 文档
- git
categories: 设计开发
---
# 前言
在公司时，使用wiki来写文档，使用jira来记录工作进度和bug，非常方便。马上要进行项目管理，本来打算自己搭建wiki和jira系统，后来想到开源中国有一个团队协作开发平台。试用了一下，基本满足要求，nice。

<!--more-->

# 写文档
本平台内置markdown编辑器，建议学习一下markdown写法。
## 普通文档
1、点击左侧导航栏的“文档”，然后点击“新文档”。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/teamwork/newdoc.jpg?imageView2/0/w/700)

2、输入文档信息。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/teamwork/newdoc2.jpg?imageView2/0/w/700)

3、之后点击文档名，进入编辑页面。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/teamwork/editdoc.jpg?imageView2/0/w/700)

4、编辑完成，可以进行预览。

## 接口文档
接口文档，在普通文档的基础上，规定结构如下：
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/teamwork/tree.jpg?imageView2/0/w/700)

下面是一个示例：
http://doc.oschina.net/interfacedemo
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/teamwork/interfacedemo.jpg?imageView2/0/w/700)

PS：接口文档后端负责编写。
后端在开发前，先写好接口文档，然后把文档地址发到讨论组里，前端就可以自己模拟数据独立进行开发。
后端在开发好接口之后，自己使用HttpRequester测试一下，确保接口没有问题，然后在讨论组里声明哪些接口已经可以使用。

## 渲染文档
thinkphp框架的前后端分离做的不彻底，默认架构为`后端路由+后端渲染+后端接口`，适合全栈开发者使用。理想的前后端分离框架，应该是由前端来负责路由控制和前端渲染，后端专注于接口，也就是`前端路由+前端渲染+后端接口`。

因为thinkphp框架中的路由控制由后端来控制，页面渲染也属于后端渲染，所以页面渲染的工作理论上应该归属于后端开发者。但是，后端开发者负责页面渲染的话，前后端就混到了一起，加大了开发的复杂度。

相较而言，让前端开发者负责页面渲染的更好一些。那么，前端开发者就需要拿到页面地址，以及用来页面渲染的数据。页面地址和页面渲染数据需要后端提供，因此需要一个文档来说明。

更多关于前后端渲染的内容，请参考《JavaScript模板引擎》。

规定渲染文档结构如下：
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/teamwork/tree2.jpg?imageView2/0/w/700)

下面是一个示例：
http://doc.oschina.net/interfacedemo2
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/teamwork/interfacedemo2.jpg?imageView2/0/w/700)

PS：渲染文档同样由后端负责编写。
后端渲染虽然不便于前后端分离，但是也有好处。因为后端渲染的话，页面加载的速度会更快些。
如果变更架构为`前端渲染+后端路由+后端接口`，也很简单，只要在前端加一个模板引擎即可。


# 记录工作进度
## 新建任务
1、每次开发前，点击左侧导航栏的“任务”，然后点击“新建任务”。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/teamwork/task.jpg?imageView2/0/w/700)

2、选择项目，标题输入当天的任务（或一段时间的任务），任务标签选择功能（feature），指派成员选自己，选择开始时间和结束时间，其他可选。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/teamwork/task2.jpg?imageView2/0/w/700)

3、点击确认创建。

## 关闭任务
当任务完成后，变更任务状态为已完成或验收通过。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/teamwork/task3.jpg?imageView2/0/w/700)

## 生成周报
每周日，点击左侧导航栏的“周报”，然后点击“提交周报”，自动生成周报。

# 记录bug
和记录工作进度类似，只不过任务标签选择缺陷（bug），指派成员选择模块的开发者。并且，在任务描述中添加一些bug的描述信息。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/teamwork/bug.jpg?imageView2/0/w/700)

# 代码托管
## 获取项目
```
git clone https://git.oschina.net/voidking/volunteer.git
```

## 上传项目
```
git pull
git add .
git commit -m "message"
git push
```

# 后记
有任何问题，或者好的建议，都可以提出来，希望大家在这个团体中共同进步，成为更加牛逼的自己。

# 书签
卓音工作室—Team@OSC团队协作开发平台
https://team.oschina.net/pandazhuoyin

码云平台帮助文档_V1.2
http://git.mydoc.io/?t=84110

码云 - 开源中国
http://git.oschina.net/