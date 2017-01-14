title: jQuery本地无法加载解决办法
date: 2014-09-12 20:18:36
tags:
- jQuery
- 问题
categories: 精华转载
---
 
环境：
win7+apache2.2+php5.2


症状：
jquery包文件(jquery-1.4.2.min.js)在本地测试的时候加载失败，通过firebug查看，发现文件被加载了两次，但每次都加载不完全。
直接引用外部的jquery包就OK，比如http://ajax.googleapis.com/ajax/libs/jquery/1.4.2/jquery.min.js
<!--more-->

解决办法：
将httpd.conf中的
```
#EnableMMAP off
#EnableSendfile off
```
注释去掉，重启apache，在firefox下马上就生效了，在ie下需要删除下缓存。


这两个参数在httpd.conf有作了一些注释
```
#
# EnableMMAP and EnableSendfile: On systems that support it, 
# memory-mapping or the sendfile syscall is used to deliver
# files.  This usually improves server performance, but must
# be turned off when serving from networked-mounted 
# filesystems or if support for these functions is otherwise
# broken on your system.
#
```

参考文章：http://forum.jquery.com/topic/can-t-load-jquery-on-apache
