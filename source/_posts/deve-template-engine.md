---
title: JavaScript模板引擎
toc: true
date: 2016-03-21 10:37:03
tags:
- JavaScript
- 前端
- 模板引擎
- 渲染
categories: 设计开发
---
# 前言
后端渲染和前端渲染，分类依据在于浏览器到底做了什么事情。

后端渲染HTML的情况下，浏览器会直接接收到经过服务器计算之后的呈现给用户的最终的HTML字符串，这里的计算就是服务器经过解析存放在服务器端的模板文件来完成的，在这种情况下，浏览器只进行了HTML的解析，以及通过操作系统提供的操纵显示器显示内容的系统调用在显示器上把HTML所代表的图像显示给用户。

前端渲染就是指浏览器会从后端得到一些信息，这些信息或许是适用于题主所说的angularjs的模板文件，亦或是JSON等各种数据交换格式所包装的数据，甚至是直接的合法的HTML字符串。这些形式都不重要，重要的是，将这些信息组织排列形成最终可读的HTML字符串是由浏览器来完成的，在形成了HTML字符串之后，再进行显示。

根据前后端渲染的不同，模板引擎也分为两种，后端模板引擎和前端模板引擎。而前端模板引擎，就是本文要说的JavaScript模板引擎。

<!--more-->

# 构建自己的JavaScript模板小引擎
## html部分
```
<!doctype html>
<html>
<head>
   <meta charset=utf-8>
   <title>Simple Templating</title>
</head>
<body>
    <div class="result"></div>

<script type="template" id="template">
    <h2>
      <a href="{{href}}">
        {{title}}
      </a>
    </h2>
    <img src="{{imgSrc}}" alt="{{title}}">
</script>

</body>
</html>
```

## 仿造ajax获取的数据
```
var data = [
    {
      title: "Knockout应用开发指南",
      href: "http://www.cnblogs.com/TomXu/archive/2011/11/21/2257154.html",
      imgSrc: "http://images.cnblogs.com/cnblogs_com/TomXu/339203/o_knockout2.jpg"
    },
    {
      title: "微软ASP.NET站点部署指南",
      href: "http://www.cnblogs.com/TomXu/archive/2011/11/25/2263050.html",
      imgSrc: "http://images.cnblogs.com/cnblogs_com/TomXu/339203/o_vs.jpg"
    },
    {
      title: "HTML5学习笔记简明版",
      href: "http://www.cnblogs.com/TomXu/archive/2011/12/06/2277499.html",
      imgSrc: "http://images.cnblogs.com/cnblogs_com/TomXu/339203/o_LearningHtml5.png"
    }
];
```

## 变量替换方法一
```
template = document.querySelector('#template').innerHTML,
result = document.querySelector('.result'),
i = 0, len = data.length,
fragment = '';
 
for ( ; i < len; i++ ) {
    fragment += template
      .replace( /\{\{title\}\}/, data[i].title )
      .replace( /\{\{href\}\}/, data[i].href )
      .replace( /\{\{imgSrc\}\}/, data[i].imgSrc );
}
 
result.innerHTML = fragment;
```

## 变量替换方法二
```
template = document.querySelector('#template').innerHTML,
result = document.querySelector('.result'),
attachTemplateToData;
 
// 将模板和数据作为参数，通过数据里所有的项将值替换到模板的标签上（注意不是遍历模板标签，因为标签可能不在数据里存在）。
attachTemplateToData = function(template, data) {
    var i = 0,
        len = data.length,
        fragment = '';
 
    // 遍历数据集合里的每一个项，做相应的替换
    function replace(obj) {
        var t, key, reg;
 　　　　　　
　　　　//遍历该数据项下所有的属性，将该属性作为key值来查找标签，然后替换
        for (key in obj) {
            reg = new RegExp('{{' + key + '}}', 'ig');
            t = (t || template).replace(reg, obj[key]);
        }
 
        return t;
    }
 
    for (; i < len; i++) {
        fragment += replace(data[i]);
    }
 
    return fragment;
};
 
result.innerHTML = attachTemplateToData(template, data);
```

# 参考文档
最简单的JavaScript模板引擎
http://www.cnblogs.com/dolphinX/p/3489269.html

大叔手记（7）：构建自己的JavaScript模板小引擎
http://www.cnblogs.com/TomXu/archive/2011/12/15/2284752.html

JavaScript Micro-Templating
http://ejohn.org/blog/javascript-micro-templating/

JavaScript模板引擎原理，几行代码的事儿
http://www.cnblogs.com/hustskyking/p/principle-of-javascript-template.html

全球最快的JS模板引擎
https://cnodejs.org/topic/5490e8ca4823a0234c9e1767

Arttemplate by aui
http://aui.github.io/artTemplate/

laytpl官网
http://laytpl.layui.com/

后端渲染html、前端模板渲染html，jquery的html，各有什么区别？
http://www.zhihu.com/question/28725977/answer/42077482