---
title: css雪碧图
toc: true
date: 2016-08-01 12:58:00
tags:
- css
- 前端
- sprite
categories: 设计开发
---
# 前言
CSS雪碧 即CSS Sprite，也有人叫它CSS精灵，是一种CSS图像合并技术，该方法是将小图标合并到一张图片上，然后利用css的背景定位来显示需要显示的图片部分。

优点：减少加载网页图片时对服务器的请求次数，提高页面的加载速度，减少鼠标滑过的一些bug。

<!--more-->

# 制作
雪碧图的制作，可以使用PS，也可以使用专门的雪碧图制作工具。制作时，最好制作成一列或者一行，定位时会方便一些。下面这个工具挺好用，分享给大家：https://yunpan.cn/cMKygj2hnrBBe  访问密码 f516

css雪碧图简单制作工具（源码）
https://github.com/iwangx/sprite

如果要制作svg雪碧图，推荐使用AI。

# 定位
## 位置
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/sprite/xbt.jpg)
雪碧图定位的关键，在于background-position。诀窍在于“调试”，在页面控制背景图上下左右移动，很快就定位好了。

![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/sprite/icon.png)
以上面的雪碧图为例，假设我们要显示微博的图标，那么scss代码如下：

```
.icon-weibo{
    width: 20px;
    height: 20px;
    background: url(../../img/test/index/icon.png) no-repeat;
    background-position: 0px -60px;
}
```

## 大小
假设我们的要显示的图标比雪碧图大，或者比雪碧图小，该怎么办？background-size。

```
.icon-weibo{
    width: 40px;
    height: 40px;
    background: url(../../img/test/index/icon.png) no-repeat;
    background-position: -2px -116px;
    background-size: 118%;
}
```

## 移动端
很多时候，我们并不使用px作为单位，而是rem或者百分比，这时候，该怎么控制雪碧图的位置和大小？利用svg图片。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/sprite/svg.png)

```
<span class="i i_menu_0"></span>
```

```
.i {
    width: 0.8rem; height: 0.8rem;
    background: url(../images/ico_global.svg) no-repeat;
    display: inline-block;
    background-size: 1100%;
}
.i_menu_0 { background-position: 0% 0%; }
.i_menu_1 { background-position: 10% 0%; }
.i_menu_2 { background-position: 20% 0%; }
.i_menu_3 { background-position: 30% 0%; }
.i_menu_4 { background-position: 40% 0%; }
.i_menu_5 { background-position: 50% 0%; }
```

# 后记
至于PS和AI的使用，在慕课网和网易云课堂上有很多优秀教程，不要错过。

# 书签
CSS雪碧图的实现方法（即背景定位）
http://www.suixin8.com/59.html

CSS3技术-雪碧图自适应缩放
http://www.imooc.com/wiki/detail/id/183

利用动态viewport+rem制作一张自适应的svg雪碧图icon
http://www.open-open.com/lib/view/open1452229325136.html

SVG的用法
http://www.webhek.com/svg/