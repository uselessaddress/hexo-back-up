title: 修改默认shell的方法
date: 2013-12-01 12:18:08
tags: 
- Linux
- Shell
categories: 点滴发现
---
1、输入which tcsh，找到tcsh所存放路径(或which ash 找到ash存放路径)，这里假设tcsh的路径为/bin/tcsh。

2、输入chsh -s /bin/tcsh，即可变更默认shell为tcsh。

3、重启，即可发现默认shell已经更改。
