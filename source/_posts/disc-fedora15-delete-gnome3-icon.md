title: Fedora15删除GNOME3面板中无用的图标
date: 2013-11-22 20:36:00
tags: 
- Linux
- Fedora
categories: 点滴发现
---
以前在Fedora15中用wine安装了一个CS和迅雷7，但运行不太流畅，所以想要删掉面板上相关的图标。
具体命令如下。

1、cd   ~/.local/share/applications/wine/Programs/
2、ls 查看目录
3、rm -r *递归删除所有的项目。