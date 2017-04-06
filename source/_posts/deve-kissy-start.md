---
title: KISSY入门篇
toc: true
date: 2017-04-06 09:00:00
tags:
- 前端框架
- kissy
categories: 设计开发
---
# WHAT IS KISSY ?
KISSY 是一款跨终端、模块化、高性能、使用简单的 JavaScript 框架。除了完备的工具集合如 DOM、Event、Ajax、Anim 等，它还提供了经典的面向对象、动态加载、性能优化解决方案。作为一款全终端支持的 JavaScript 框架，KISSY 为移动终端做了大量适配和优化，让你的程序在全终端均能流畅运行。

<!--more-->
# 引入KISSY
1、引入线上kissy。
```
<script src="http://g.alicdn.com/kissy/k/1.4.7/seed.js"></script>
```

2、引入本地kissy。
```
<script src="./lib/kissy-1.3.2/build/seed.js"></script>
```

# hello world
1、启动：Hello World!
```
KISSY.ready(function(S){
    alert('Hello World!');
});
```

2、DOM操作：获取一个className叫continue的button，并将它的内容改为"Hello Kissy"。

```
KISSY.use('node',function(S,Node){
    Node.one('button.continue').html('Hello Kissy!');
});
```

3、事件处理：点击一个id为click-me的button，显示#banner-msg的内容。
```
KISSY.use('node',function(S,Node){
    Node.one('#click-me').on('click',function(e){
        Node.one('#banner-msg').show();
    });
});
```

4、Ajax：请求本地数据/data/ajax-data.json，带入参数zipcode，将结果显示在#weather-con中。
```
KISSY.use('io,node',function(S,io,Node){
    io({
        url:'./data/ajax-data.json',
        data:{
            zipcode:10010
        },
        success:function(data){
            Node.one('#weather-con').html('<em>' + data.weather + '</em> 摄氏度');
        }
    });
});
```


# 源码分享
https://github.com/voidking/kissy-start.git


# 书签
KISSY - A Powerful JavaScript Framework
http://docs.kissyui.com/

KISSY项目地址
https://github.com/kissyteam/kissy

KISSY 指导手册
http://docs.kissyui.com/1.4/docs/html/guideline/get-started.html

jQuery - KISSY Rosetta Stone
http://cyj.me/jquery-kissy-rosetta/

KISSY 模块定义规范（KMD）
http://docs.kissyui.com/1.4/docs/html/guideline/kmd.html



