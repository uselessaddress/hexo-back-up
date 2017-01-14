---
title: Web安全学习（三）
toc: false
date: 2016-09-23 11:00:36
tags:
- 安全
- 黑客
categories: 设计开发
---
# 漏洞扫描
漏洞扫描器可以快速帮助我们发现漏洞，例如，SQL注入漏洞（SQL injection）、跨站脚本攻击（cross site scripting）、缓冲区溢出（buffer overflow）。一个好的漏洞扫描器在渗透测试中是至关重要的，可以说是渗透成功或者失败的关键点。

<!--more-->

一款优秀的漏洞扫描器会使渗透测试变得轻松，但对于一些漏洞，自动化软件是无法识别的，例如，逻辑性漏洞，及其隐蔽的XSS漏洞或者SQL注入漏洞。所以，在进行漏扫时，必须要与人工渗透相结合。

漏洞扫描也属于信息探测的一种，扫描器可以帮助我们发现非常多的问题。

本章主要介绍了几款工具的使用，小编更加喜欢在应用中学习，因此，不再摘抄书中内容，仅记录一下三款优秀的工具，具体用法自行百度。
1、Burp Suite
在sectools.org Web扫描模块位居第一名。

2、AWVS
AWVS（Acunetix Web Volunerability Scanner）是一个自动化的Web应用程序安全测试工具，它可以扫描任何可通过Web浏览器访问的和遵循HTTP/HTTPS规则的Web站点和Web应用程序。

也有很多人喜欢把AWVS改为WVS称呼，两者是同一款工具。

WVS可以快速扫描跨站脚本攻击（XXS）、SQL注入攻击、代码执行、代码执行、目录遍历攻击、文件入侵、脚本源代码泄漏、CRLF注入、PHP代码注入、XPath注入、LDAP注入、Cookie操纵、URL重定向、应用程序错误消息等。

官网：http://www.acunetix.com/

3、AppScan
AppScan是IBM公司出品的一个领先的Web应用安全测试工具，曾以Watchfire AppScan的名称享誉业界。AppScan可自动化Web应用的安全漏洞评估工作，能扫描和检测所有常见的Web应用安全漏洞，例如，SQL注入、跨站点脚本攻击、缓冲区溢出及最新的Flash/Flex
应用和Web2.0应用暴露等方面的安全漏洞扫描。

其他国外工具：
HP Webinspect、Owasp Zap、Nikto、Owasp WebScarab、w3af、Netsparker。

其他国内工具：
JSKY、Safe3 Web Vul Scanner、安恒信息明鉴Web应用弱点扫描器、极光远程安全评估系统。