---
title: Python基础
toc: true
date: 2017-01-12 21:00:52
tags:
- python
categories: 设计开发
---
# 前言
Python，是龟叔在1989年为了打发无聊的圣诞节而编写的一门编程语言，特点是优雅、明确、简单，现今拥有丰富的标准库和第三方库。
Python适合开发Web网站和各种网络服务，系统工具和脚本，作为“胶水”语言把其他语言开发的模块包装起来使用，科学计算等等。

小编学习Python的理由有三个：
为了爬取需要的各种数据，不妨学习一下Python。
为了分析数据和挖掘数据，不妨学习一下Python。
为了做一些好玩有趣的事，不妨学习一下Python。

<!--more-->
# 准备工作
1、在Python官网下载安装喜欢的版本，小编使用的，是当前最新版本3.6.0。
2、打开IDLE，这是Python的集成开发环境，尽管简单，但极其有用。IDLE包括一个能够利用颜色突出显示语法的编辑器、一个调试工具、Python Shell，以及一个完整的Python3在线文档集。

# hello world
1、在IDLE中，输入`print('hello world');`，回车，则打印出hello world。

2、使用sublime新建文件hello.py，内容如下：
```
print('hello world');
```
在Windows下，shift+右键，在此处打开命令窗口，执行`python hello.py`，回车，则打印出hello world。

3、使用sublime新建文件hello.py，内容如下：
```
#!/usr/bin/env python
print('hello world');
```
在Linux或Mac环境下，可以直接运行脚本。首先添加执行权限`chmod a+x hello.py`，然后执行`./hello.py`。当然，也可以和Windows一样，使用`python hello.py`来执行脚本。

# 引入模块
1、新建name.py，内容如下：
```
name='voidking';
```
2、执行`python name.py`。
3、进入python shell模式，执行`import name`，`print(name.name)`，则打印出voidking。

# 语法
常用函数（print）、数据类型、表达式、变量、条件和循环、函数。和其他语言类似，不再展开。

# 书签
Python官网
https://www.python.org/

Python入门
http://www.imooc.com/learn/177

如何学习Python爬虫[入门篇]？
https://zhuanlan.zhihu.com/p/21479334?refer=passer

你需要这些：Python3.x爬虫学习资料整理
https://zhuanlan.zhihu.com/p/24358829?refer=passer