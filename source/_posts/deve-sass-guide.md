title: SASS使用说明
toc: true
date: 2016-02-01 20:44:26
tags:
- 前端
- sass
- css
categories: 设计开发
---
# 前言
> Sass，Syntactically Awesome StyleSheets。
> Sass 是对 CSS 的扩展，让 CSS 语言更强大、优雅。 它允许你使用变量、嵌套规则、 mixins、导入等众多功能， 并且完全兼容 CSS 语法。 Sass 有助于保持大型样式表结构良好， 同时也让你能够快速开始小型项目， 特别是在搭配 Compass 样式库一同使用时。

<!--more-->

# 安装
1、下载安装[ruby](http://rubyinstaller.org/downloads)
2、安装好ruby后，再ruby命令行中输入`gem install sass`

# 语法
## 变量
sass中可以定义变量，方便统一修改和维护。
```
$fontStack:    Helvetica, sans-serif;
$primaryColor: #333;

body {
  font-family: $fontStack;
  color: $primaryColor;
}
```

## 嵌套
sass可以进行选择器的嵌套，表示层级关系，看起来很优雅整齐。
```
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li { display: inline-block; }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none`;
  }
}`。

```

## 导入
sass中如导入其他sass文件，最后编译为一个css文件，优于纯css的`@import`。
```
// reset.scss

html,
body,
ul,
ol {
  margin: 0;
  padding: 0;
}
```

```
@import 'reset';

body {
  font-size: 100% Helvetica, sans-serif;
  background-color: #efefef;
}
```

## mixin
sass中可用mixin定义一些代码片段，且可传参数，方便日后根据需求调用。从此处理css3的前缀兼容轻松便捷。
```
@mixin box-sizing ($sizing) {
    -webkit-box-sizing:$sizing;     
       -moz-box-sizing:$sizing;
            box-sizing:$sizing;
}
.box-border{
    border:1px solid #ccc;
    @include box-sizing(border-box);
}
```

## 扩展/继承
sass可通过`@extend`来实现代码组合声明，使代码更加优越简洁。
```
.message {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.success {
  @extend .message;
  border-color: green;
}

.error {
  @extend .message;
  border-color: red;
}

.warning {
  @extend .message;
  border-color: yellow;
}
```

## 运算
sass可进行简单的加减乘除运算等
```
.container { width: 100%; }

article[role="main"] {
  float: left;
  width: 600px / 960px * 100%;
}

aside[role="complimentary"] {
  float: right;
  width: 300px / 960px * 100%;
}
```

## 颜色
sass中集成了大量的颜色函数，让变换颜色更加简单。
```
$linkColor: #08c;
a {
    text-decoration:none;
    color:$linkColor;
    &:hover{
      color:darken($linkColor,10%);
    }
}
```

## &表示当前元素
伪类和伪元素在CSS中是常用的一种方式，比如最常见的是链接的伪类或者说伪元素:after和:before的使用。大家常看到的就是清除浮动的clearfix:
```
.clearfix:before,
.clearfix:after {
    content:"";
    display:table;
}
.clearfix:after {
    clear:both;
    overflow:hidden;
}
.clearfix {
    *zoom: 1;
}
```

那么在Sass中，使用&会变得更简单，更方便：
```
$lte-ie: true !default;

.clearfix {
    @if $lte-ie {
        *zoom: 1;
    }

    &:before,
    &:after {
        content: "";
        display: table;
    }
    &:after {
        clear: both;
        overflow: hidden;
    }
}
```

&不止用于和伪类的结合，还可以用于多类选择器、后代选择器、相邻兄弟选择器、媒体查询中的嵌套等。

# 2016.07.30补
## Sass编码错误
ruby环境sass编译中文出现Syntax error: Invalid GBK character。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/sass-error.jpg)
解决办法如下：打开ruby中sass的安装目录，在其中找到engine.rb。在engine.rb中所有的`require XXX`之后，添加

```
Encoding.default_external = Encoding.find('utf-8')
```

PS:小编的engine.rb的路径为`D:\Program Files\Ruby23-x64\lib\ruby\gems\2.3.0\gems\sass-3.4.22\lib\sass\engine.rb`

## scss自动编译为css
如果希望某一个scss文件或者相应的文件夹下面文件修改后，自动进行编译，那么可以使用侦听命令。
1、侦听文件
```
sass --watch --style compressed style.scss:style.css
```

2、侦听文件夹
```
sass --watch --style compressed scss:css
```

3、封装为脚本
为了避免每次运行都敲命令，我们把上述命令分装为脚本scss.bat。
```
sass --watch --style compressed scss:css
```

海哥哥封装了不一样的scss-wap.bat，其中的参数至今没有看懂，可以实现同样的效果。
```
@echo off
sass -C -t compressed --watch scss:css
rem  sass -C -t compact --watch scss:css
pause
```

该命令的含义为：
# 书签
sass安装
http://www.w3cplus.com/sassguide/install.html

SASS用法指南
http://www.ruanyifeng.com/blog/2012/06/sass.html

sass入门
http://www.w3cplus.com/sassguide/

SASS 初学者入门
http://www.oschina.net/translate/the-absolute-beginners-guide-to-sass

Sass参考手册
http://sass.bootcss.com/docs/sass-reference/

SassMeister在线调试
http://www.sassmeister.com/

[Sass中连体符（&）的运用](http://www.w3cplus.com/preprocessor/use-ampersand-in-selector-name-with-Sass.html)

ruby环境sass编译中文出现Syntax error: Invalid GBK character错误解决方法 
http://www.tuicool.com/articles/f2YVRv
http://www.cnblogs.com/zhidong123/p/3902270.html

SASS的安装和转换为CSS的方法
http://www.cnblogs.com/52css/archive/2012/08/19/sass-how-to-install-and-use.html

前端之Sass/Scss实战笔记
http://www.tuicool.com/articles/iERVJbB
http://segmentfault.com/a/1190000003742313