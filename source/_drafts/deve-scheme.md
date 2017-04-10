---
title: 函数式编程之Scheme
toc: true
date: 2017-01-01 00:00:00
tags:
- 函数式编程
- lisp
- scheme
categories: 设计开发
---
# 前言
Lisp是Fortran语言之后第二古老的高级编程语言，自成立之初已发生了很大变化。如今，最广为人知的通用的是Lisp方言：Common Lisp和Scheme。

Common Lisp和Scheme有什么不同呢？个人认为，Common Lisp和Scheme，就像狮子和家犬。狮子更加强大，难以驯服；家犬更加小巧，容易驯服。而选择哪一种，取决于实际需要。鉴于小编只是需要学习一下函数式编程的思想，所以Scheme足够了。

PS：MIT 的两本著名教材 SICP（Structure and Interpretation of Computer Programs）和  HTDP（How to Design Programs）都是以Scheme为基础的。

<!--more-->

# 安装scheme
1、进入[scheme官网](https://www.gnu.org/software/mit-scheme/)，下载对应平台的scheme（下文以windows为例）。

2、安装下载的scheme。

3、双击scheme快捷方式，如果出现如下界面，表明安装成功。
![](setup.jpg)

PS：如果打开scheme的时候报错“Requested allocation is too large. Try with smaller argument to –heap”。那么，在程序的快捷方式上右键，属性，在“目标”里加上`-heap 512`。 

4、把安装路径加入环境变量。
例如，我的scheme安装路径为`C:\Program Files (x86)\MIT-GNU Scheme`，那么，在path中加入`C:\Program Files (x86)\MIT-GNU Scheme\bin`。同时，新建系统变量MITSCHEME_LIBRARY_PATH，值为`C:\Program Files (x86)\MIT-GNU Scheme\lib`。

5、测试scheme。
打开命令行，输入`mit-scheme`，弹出如下界面，表明安装配置成功。
![]()

# hello world
1、新建hello.scm文件，内容如下：
```
;The first program
(begin
    (display "Hello, World!")
    (newline)
)
```


# 书签
函数式编程初探
http://www.ruanyifeng.com/blog/2012/04/functional_programming.html

函数式编程入门教程
http://www.ruanyifeng.com/blog/2017/02/fp-tutorial.html

Lisp教程
http://www.yiibai.com/lisp/

Scheme语言简明教程
http://www.kancloud.cn/wizardforcel/teach-yourself-scheme/147161



