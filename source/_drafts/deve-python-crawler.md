---
title: Python爬虫
toc: true
date: 2017-01-17 11:21:00
tags:
- python
- 爬虫
categories: 设计开发
---
# 前言
Python天生适合用来开发网页爬虫，
1）抓取网页本身的接口
相比与其他静态编程语言，如java，c#，C++，python抓取网页文档的接口更简洁；相比其他动态脚本语言，如perl，shell，python的urllib2包提供了较为完整的访问网页文档的API。（当然ruby也是很好的选择）
此外，抓取网页有时候需要模拟浏览器的行为，很多网站对于生硬的爬虫抓取都是封杀的。这是我们需要模拟user agent的行为构造合适的请求，譬如模拟用户登陆、模拟session/cookie的存储和设置。在python里都有非常优秀的第三方包帮你搞定，如Requests，mechanize

2）网页抓取后的处理
抓取的网页通常需要处理，比如过滤html标签，提取文本等。python的beautifulsoap提供了简洁的文档处理功能，能用极短的代码完成大部分文档的处理。

其实以上功能很多语言和工具都能做，但是用python能够干得最快，最干净。Life is short， u need python.

# 后记

# 书签
为什么python适合写爬虫？
http://www.cnblogs.com/benzone/p/5854084.html

如何学习Python爬虫[入门篇]？
https://zhuanlan.zhihu.com/p/21479334?refer=passer

你需要这些：Python3.x爬虫学习资料整理
https://zhuanlan.zhihu.com/p/24358829?refer=passer

如何入门 Python 爬虫？
https://www.zhihu.com/question/20899988


