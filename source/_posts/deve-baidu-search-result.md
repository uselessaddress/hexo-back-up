title: 百度搜索结果页
toc: true
date: 2016-01-20 20:25:44
tags:
- 艾佳生活
- 前端
categories: 设计开发
---
# 前言
1月18日，入职艾佳，办好了各种手续。
从海哥那里，接到了第一个任务——周一到周三（18到20），高仿一个百度搜索结果页。
小编随手挖了一个坑：“上海”，接着便开始填坑。

<!--more-->

# 界面布局
浏览器，是一个长方体的不透明的可变体积的盒子，盒子上面开了一个长方形的窗口。从这个窗口，我们可以看到盒子里的内容。
盒子有很多层，IE、FireFox、Safari有2147483647层，Opera有2147483584层。一般情况下，10层，就足够我们使用了。 

界面布局，也就是在浏览器盒子里面摆放东西的方式。一般分为三种：标准文档流、浮动和定位。

## 标准文档流
标准文档流，就是在盒子的第一层摆放东西，按照从左到右，从上到下的方式，符合我们平时的思维习惯。

## 浮动
浮动会脱离标准文档流，不再按照从左到右，从上到下的方式。但是，浮动的元素，仍然处于第一层。

对于浮动，最认同张鑫旭的看法。浮动出现的意义其实只是用来让文字环绕图片而已，仅此而已。浮动的本质为“包裹与破坏”！

1、包裹
撇开浮动的“破坏性”，浮动就是个带有方位的display:inline-block属性。

2、破坏
文字之所以会环绕含有float属性的图片，是因为浮动破坏了正常的line boxes。

## 定位

### static
元素出现在标准文档流中。

### relative
生成相对定位的元素，相对于其正常位置进行定位。

因此，"left:20" 会向元素的 LEFT 位置添加 20 像素。

### absolute
生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。

元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。

### fixed
生成绝对定位的元素，相对于浏览器窗口进行定位。

元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。


# 盒子模型
上面我们说，要在浏览器这个盒子里摆放东西。那么，摆放什么东西呢？盒子！没有顶的盒子。
- margin：盒子间的距离。
- border：盒子的板厚度。
- padding：盒子里的减震泡沫厚度。
- content：盒子里可以摆放物品的空间尺寸。

# 全局设置
排版的时候，小编发现，在内容和浏览器窗口之间，会存在缝隙。所以，一般会设置一个全局样式：
```
*{
    margin: 0;
    padding: 0;
}
```

# 下拉菜单
![](http://7oxjrx.com1.z0.glb.clouddn.com/%40%2Fimgs%2Fbaidu-search%2Fdown.jpg)
鼠标移动到顶部的“设置”或者“用户名”下面，会出现下拉菜单。
这个效果，就需要用到定位中的“absolute”了，

# 块级元素行内元素
1、行内元素与块级元素直观上的区别
行内元素会在一条直线上排列，都是同一行的，水平方向排列
块级元素各占据一行，垂直方向排列。块级元素从新行开始结束接着一个断行。

2、块级元素可以包含行内元素和块级元素，行内元素不能包含块级元素。

3、行内元素与块级元素属性的不同，主要是盒模型属性上
行内元素设置width无效，height无效(可以设置line-height)，margin上下无效，padding上下无效

# line-height和vertical-align 
line-height适用于所有元素，设置元素中行的高度。
vertical-align适用于行内元素和单元格（table-cell）元素，设置元素内容的垂直对齐方式。

# 后记
本次任务，遵循一个原则：能用HTML+CSS实现的，不用JavaScript；能用JavaScript实现的，不用jQuery。
上面主要记录了这次任务用到的技术和原理，细节方面的东西，看代码吧，完成度80%：https://github.com/voidking/baidu-search-result.git

# 参考文档
如何利用 CSS 製作多級選單？
https://zespia.tw/blog/2012/01/24/css-multi-level-menu/

网页布局基础
http://www.imooc.com/learn/95

css知多少（7）——盒子模型
http://www.cnblogs.com/wangfupeng1988/p/4287292.html

CSS+DIV定位分析
http://blog.163.com/love_heartbreaking/blog/static/124561901201211334714800/

DOM中关于脱离文档流的几种情况分析
http://www.tuicool.com/articles/IBJvyy7
http://www.cnblogs.com/chuaWeb/p/html_css_position_float.html

[CSS float浮动的深入研究、详解及拓展(一)](http://www.zhangxinxu.com/wordpress/2010/01/css-float%E6%B5%AE%E5%8A%A8%E7%9A%84%E6%B7%B1%E5%85%A5%E7%A0%94%E7%A9%B6%E3%80%81%E8%AF%A6%E8%A7%A3%E5%8F%8A%E6%8B%93%E5%B1%95%E4%B8%80/)

CSS深入理解之float浮动
http://www.imooc.com/view/121

行内元素与块级元素比较全面的区别和转换
http://blog.csdn.net/sykent/article/details/7738408

line-height 和 vertical-align 行高与行对齐精解 （图文） 
http://www.jb51.net/css/29328.html



