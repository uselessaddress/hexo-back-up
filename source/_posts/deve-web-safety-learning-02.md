---
title: Web安全学习（二）
toc: true
date: 2016-09-22 17:00:36
tags:
- 安全
- 黑客
categories: 设计开发
---
# 信息探测
在进行安全测试之前，最重要的一步就是信息探测。信息探测时应该搜集哪些资料呢？其实最主要的就是与服务器的配置信息和网站的信息，包括网站注册人、目标网站系统、目标服务器系统、目标网站相关子域名、目标服务器开放的端口和服务器存放网站等。
<!--more-->

# Nmap
Nmap是一个开源的网络连接端扫描软件，用来扫描计算机开放的网络连接端，确定哪些服务运行在哪些连接端，并且推断计算机运行哪个操作系统。另外，它也用于评估网络系统安全。

## 常用命令
1、扫描主机列表
`nmap -sL 192.168.199.1/24`

2、扫描特定主机上的特定端口
`nmap -p 80,21,23 192.168.199.1`

3、探测主机操作系统
`nmap -O 192.168.199.1`

4、全面的系统探测
`nmap -v -A 192.168.199.1`

5、穿透防火墙的进行扫描
`nmap -Pn -A 192.168.199.1`

6、使用文件。如果你有一个ip地址列表，将这个保存为一个txt文件，和命令行执行路径相同，扫描这个txt内的所有主机。
`nmap -iL target.txt`

PS：target.txt内容
```
127.0.0.1
www.baidu.com
```

7、保存扫描结果
`nmap -O 127.0.0.1 -oN result.txt`

8、windows扫描本机
`nmap -sT 127.0.0.1`

## 脚本引擎
在Nmap安装目录下存在Script文件夹，在Script文件夹中存在许多以“.nse”后缀结尾的文本文件，即Nmap自带的脚本引擎。

1、扫描Web敏感目录
`nmap -p 80 --script=http-enum.nse 192.168.199.1`

2、扫描SqlInjection
`nmap -p 80 --script=sql-injection.nse 192.168.199.1`

3、使用通配符扫描
`nmap --script="http-*" 192.168.199.1`

4、使用所有的脚本进行扫描
`nmap --script all 192.168.199.1`

# DirBuster
在渗透测试中，探测Web目录结构和隐藏的敏感文件是必不可少的一部分。通过探测可以了解网站的结构，获取管理员的一些敏感信息，比如网站的后台管理页面、文件上传界面，有事甚至可能扫描出网站的源代码。而DirBuster就是完成这些功能的一款优秀的资源探测工具。

图形化界面，熟悉下即可。

# 指纹识别
此处的指纹识别并非一些门禁指纹识别、财务指纹识别、汽车指纹识别等，而是针对计算机或计算机系统的某些服务的指纹识别。

Namp的`nmap -O`命令，御剑的指纹识别，AppPrint等。

# 书签
Nmap: the Network Mapper - Free Security Scanner
https://nmap.org/

DirBuster download | SourceForge.net
https://sourceforge.net/projects/dirbuster/

Nmap scan produces all “unknown”
http://security.stackexchange.com/questions/21544/nmap-scan-produces-all-unknown

Zenmap prits all states as unknown
http://stackoverflow.com/questions/30289512/zenmap-prits-all-states-as-unknown