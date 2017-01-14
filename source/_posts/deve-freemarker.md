title: freemarker入门
toc: true
date: 2016-03-02 21:43:53
tags:
- freemarker
categories: 设计开发
---
# 前言
昨天，小编介绍了一款前端模板引擎artTemplate。今天，介绍一款后端模板引擎freemarker。
> Apache FreeMarker is a template engine: a Java library to generate text output (HTML web pages, e-mails, configuration files, source code, etc.) based on templates and changing data. 

<!--more-->

# 基本语法
## 定义和输出
```
<#assign answer=42/>
${answer}
```
结果为：42

## 分支语句
```
<#assign age=23>
<#if (age>60)>老年人
<#elseif (age>40)>中年人
<#elseif (age>20)>青年人
<#else> 少年人
</#if>
```

## 循环语句
```
<#list books as book>
    <tr>
        <td>${book.name}</td>
        <td>作者:${book.author}</td>
    </tr>
</#list>
```

## 运算和函数
```
<#assign x=5>
${ (x/2)?int }
${ 1.1?int }
${ 1.999?int }
${ -1.1?int }
${ -1.999?int }
```
结果为:2 1 1 -1 -1

# 源码分享
https://github.com/voidking/freemarker.git

# 参考文档
FreeMarker Java Template Engine
http://freemarker.org/

Download / Maven - Apache FreeMarker
http://freemarker.org/freemarkerdownload.html

Index (FreeMarker 2.3.24-incubating API)
http://freemarker.org/docs/api/index-all.html

模板引擎freemarker的简单使用教程
http://blog.csdn.net/stormwy/article/details/26172353

FreeMarker使用详解
http://www.open-open.com/lib/view/open1394860597743.html

Freemarker由浅入深01-环境搭建、测试
http://blog.csdn.net/it_wangxiangpan/article/details/17526869

ftl的使用  
http://blog.163.com/liuweiyoung@126/blog/static/1731310452012112755018198/

【FreeMarker】【模板文件FTL】模板自定义指令 macro
http://blog.csdn.net/robinjwong/article/details/40427977

freemarker的使用心得
http://blog.csdn.net/walkcode/article/details/40707997

Freemarker中Configuration的setClassForTemplateLoading方法参数问题
http://www.cnblogs.com/fangjian0423/p/freemarker-templateloading-question.html

Servlet 3.0笔记之使用Freemarker替代JSP，更快更轻更高效
http://www.blogjava.net/yongboy/archive/2010/07/04/346224.html


