---
title: 基于JQuery实现滚动到页面底端时自动加载更多信息
date: 2016-06-15 15:48:55
toc: false
tags:
- jquery
- 前端
categories: 设计开发
---
这篇文章主要介绍了基于JQuery实现滚动到页面底端时自动加载更多信息，类似微博，新浪新闻的评论等，都采用了这方法,需要的朋友可以参考下关键代码：
```
var stop=true; 
$(window).scroll(function(){ 
    totalheight = parseFloat($(window).height()) + parseFloat($(window).scrollTop()); 
    if($(document).height() <= totalheight){ 
        if(stop==true){ 
            stop=false; 
            $.post("ajax.php", {start:1, n:50},function(txt){ 
                $("#Loading").before(txt); 
                stop=true; 
            },"text"); 
        } 
    } 
});
```

<!--more-->

HTML代码如下:
```
<div id="Loading">Loading...</div>
```

实现方法是比较页面总高度和下滚高度以判断是否到达底端，若到达底端则通过ajax读取更多的内容，用before插入到Loading之前。
stop参数是考虑到ajax读取耗时，防止在一次ajax读取过程中多次触发事件，造成多次加载内容。

转载自http://www.3lian.com/edu/2014/02-10/127916.html
