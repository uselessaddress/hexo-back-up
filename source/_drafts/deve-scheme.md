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

# 安装mit-scheme
1、进入[mit-scheme官网](https://www.gnu.org/software/mit-scheme/)，下载对应平台的mit-scheme（下文以windows为例）。

2、安装下载的mit-scheme（下文简称scheme）。

3、双击scheme快捷方式，如果出现如下界面，表明安装成功。
![](setup.jpg)

PS：如果打开scheme的时候报错“Requested allocation is too large. Try with smaller argument to –heap”。那么，在程序的快捷方式上右键，属性，在“目标”里加上`-heap 512`。 

4、把安装路径加入环境变量。
例如，我的scheme安装路径为`C:\Program Files (x86)\MIT-GNU Scheme`，那么，在path中加入`C:\Program Files (x86)\MIT-GNU Scheme\bin`。同时，新建系统变量MITSCHEME_LIBRARY_PATH，值为`C:\Program Files (x86)\MIT-GNU Scheme\lib`。

5、测试scheme。
打开命令行，输入`mit-scheme`，弹出如下界面，表明安装配置成功。
![](config.jpg)

6、退出scheme。
在scheme命令窗口输入`(exit)`，选择y，退出scheme命令窗口。

# hello world
在scheme命令窗口中输入代码非常麻烦，光标不能回退和上下移动，所以比较简单的方法就是运行已经写完的文件。

1、新建hello.scm文件，内容如下：
```
;The first program
(begin
    (display "Hello, World!")
    (newline)
)
```

2、运行hello.scm。
`mit-scheme -load hello.scm`，效果如下：
![](run.jpg)

# 基本语法
## 数据类型
Scheme中的简单数据类型包含 boolean（布尔类型），number（数字类型），character（字符类型） 和 symbol（标识符类型）。

复合数据类型是以组合的方式通过组合其它数据类型数据来获得。包括string、vector、dotted pair、list等。

数据类型详解：
http://www.kancloud.cn/wizardforcel/teach-yourself-scheme/147165

## 代码结构
函数式编程只用“表达式”，不用“语句”。

“表达式”（expression）是一个单纯的运算过程，总是有返回值；“语句”（statement）是执行某种操作，没有返回值。函数式编程要求，只使用表达式，不使用语句。也就是说，每一步都是单纯的运算，而且都有返回值。

原因是函数式编程的开发动机，一开始就是为了处理运算（computation），不考虑系统的读写（I/O）。"语句"属于对系统的读写操作，所以就被排斥在外。
当然，实际应用中，不做I/O是不可能的。因此，编程过程中，函数式编程只要求把I/O限制到最小，不要有不必要的读写行为，保持计算过程的单纯性。

基本格式：
`(function_name arg0,arg1,...)`

示例：
```
(display "Hello, World!")
(newline)
(exit)
(number? 42)
(eqv? 42 42)
(>= 4.5 3)
```

迄今为止我们提供的Scheme示例程序都是s-表达式。这对所有的Scheme程序来说都适用：程序是数据。

代码结构详解：
http://www.kancloud.cn/wizardforcel/teach-yourself-scheme/147166


# 书签
函数式编程初探
http://www.ruanyifeng.com/blog/2012/04/functional_programming.html

函数式编程入门教程
http://www.ruanyifeng.com/blog/2017/02/fp-tutorial.html

Lisp教程
http://www.yiibai.com/lisp/

Scheme语言简明教程
http://www.kancloud.cn/wizardforcel/teach-yourself-scheme/147161



