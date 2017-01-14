---
title: input绑定回车事件
toc: false
date: 2016-06-27 10:26:43
tags: 
- 前端
- jquery
categories: 设计开发
---

html部分：
```
<input id="search-key" type="text" placeholder="请输入关键字">
```

<!--more-->

JavaScript部分：
```
$('#search-key').keypress(function(event) {
    var key = event.which;
    console.log(key);
    if(key == 13){
        //do something
    }
});
```
