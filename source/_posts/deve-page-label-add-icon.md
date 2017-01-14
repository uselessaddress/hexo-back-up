title: 网页标签添加图标
date: 2015-01-10 21:34:05
tags: [网站,图标]
categories: 设计开发
toc: true
---

### 前言
这个标题是啥意思？如下图，每个网站都有自己的标签图标：
![](http://voidking.qiniudn.com/@/imgs/网页标签图标.jpg)
<!--more-->
### 制作图标
图标，值得你花费时间用心选择、设计。
每当你看到那个蓝色的脚印，都会想起百度！我们难以超越百度，但是，我们也能拥有自己的“品牌”，哪怕没人知道，也很爽，不是吗？

#### 在线生成
百度`在线favicon生成`，你会找到很多在线制作favicon图标的网站。上传一张图片，就可以自动生成你想要的favicon.ico图标，下载即可。

#### 软件生成
制作favicon的软件有很多，这里推荐一款`魔法ICO v2.00`。

### 使用方法
假设我们已经得到了favicon图标，这时，应该怎样使用它呢？

把favicon.ico上传到服务器放在网站根目录下，然后在首页文件中<head>段插入：
```
<link rel="shortcut icon" href="favicon.ico">
<link rel="Bookmark" href="favicon.ico">
```
> 第一句用来正常显示图标，第二句用来添加书签时显示图标。

如果你希望出现动画效果的favicon图标，那就上传animated_favicon1.gif（动画图标）并且添加如下的HTML标签：
```
<link rel="shortcut icon" href="favicon.ico" >
<link rel="icon" href="animated_favicon1.gif" type="image/gif" >
```
### 小结
整个过程还是很简单，记录一下，希望对不熟悉的小伙伴有点帮助。

