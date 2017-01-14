---
title: 响应式布局
toc: true
date: 2016-06-20 18:20:41
tags:
- 毕设
- Node
- 响应式
categories: 设计开发
---
# 前言
响应式布局，简单来说，就是利用CSS3的Media Query（媒介查询），来探测访问系统的终端的宽和高等属性，并以此来决定给用户展示什么样的页面。

毕设的社区网站系统设计了两套UI，以768px为两种UI的分割点。

<!--more-->

# 界面展示

![PC端](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/responsive/01.jpg)
![手机端](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/responsive/02.jpg)

# sass
```
/* 超小屏幕（手机，小于 768px） */
@media (max-width: 767px) {
}

/* 小屏幕（平板，大于等于 768px） */
@media (min-width: 768px) {
}
```

源码：https://github.com/voidking/nodeforum/blob/master/public/scss/partial/header.scss

# 书签
Bootstrap v3中文文档
http://v3.bootcss.com/getting-started/#examples

Bootstrap中文网开源项目免费 CDN 服务
http://www.bootcdn.cn/

20分钟打造你的Bootstrap站点
http://www.w3cplus.com/css/twitter-bootstrap-tutorial.html

程序员们最爱的12款Bootstrap模板 ！
http://www.chinaz.com/free/2014/0924/368583.shtml

15个好看的Bootstrap HTML网站模板下载
http://www.shejidaren.com/bootstrap-mo-ban.html

折腾响应式布局设计
http://caibaojian.com/356.html

响应式设计的性能优化
http://www.jianshu.com/p/193911ee72e2

Bootstrap File Input Demo
http://plugins.krajee.com/file-basic-usage-demo

彻底弄懂css中单位px和em,rem的区别
http://www.cnblogs.com/leejersey/p/3662612.html
