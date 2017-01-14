---
title: artTemplate
toc: true
date: 2016-03-21 11:37:03
tags:
- JavaScript
- 前端
- 模板引擎
categories: 设计开发
---
# 前言
在开发过程中，常常要实现这样一种效果：获取数据，并且插入到当前页面。最基本的做法是把获取到的数据拼接到一个字符串中，然后使用html()或者append()函数插入到页面中。这种做法，拼接字符串时很麻烦。本文中，小编要介绍一下ArtTemplate，一个超快的前端模板引擎。

<!--more-->

# 快速上手
## 编写模板
```
<script id="test" type="text/html">
<h1>{{title}}</h1>
<ul>
    {{each list as value i}}
        <li>索引 {{i + 1}} ：{{value}}</li>
    {{/each}}
</ul>
</script>
```

## 渲染模板
```
var data = {
    title: '标签',
    list: ['文艺', '博客', '摄影', '电影', '民谣', '旅行', '吉他']
};
var html = template('test', data);
document.getElementById('content').innerHTML = html;
```

# 模板语法
注意：简洁语法和原生语法引入的js文件不同。
## 简洁语法
```
{{if admin}}
    {{include 'admin_content'}}

    {{each list}}
        <div>{{$index}}. {{$value.user}}</div>
    {{/each}}
{{/if}}
```

## 原生语法
```
<%if (admin){%>
    <%include('admin_content')%>

    <%for (var i=0;i<list.length;i++) {%>
        <div><%=i%>. <%=list[i].user%></div>
    <%}%>
<%}%>
```

# 参考文档
Arttemplate by aui
http://aui.github.io/artTemplate/

前端模版artTemplate的介绍及使用
http://blog.csdn.net/playboyanta123/article/details/45536501

