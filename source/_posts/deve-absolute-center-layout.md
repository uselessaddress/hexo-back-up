---
title: 绝对定位居中布局
toc: true
date: 2016-07-12 20:19:51
tags:
- css
- 前端
categories: 设计开发
---

# 前言
绝对定位并且居中显示，在开发中经常用到。总结了一下这种布局的三种方法，备忘。

# 方法一

```
position: absolute;
top: 0;
left: 0;
right: 0;
bottom: 0;
margin: auto;
width: 82%;
height: 40%;
```

<!--more-->

# 方法二

```
position: absolute;
left: 50%; 
top: 50%;
margin-top: -20%;
margin-left: -41%;
width: 82%;
height: 40%;
```

# 方法三

```
position: absolute;
left: 50%; 
top: 50%;
transform: translate(-50%, -50%);    /* 50%为自身尺寸的一半 */
margin: auto;
```

# 后记
以上三种绝对定位居中布局方法，同样适用于固定定位，即`position:fixed;`，只不过是相对的容器变成了浏览器。