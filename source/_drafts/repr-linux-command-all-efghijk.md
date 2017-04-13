title: Linux命令大全——EFGHIJK
date: 2013-07-16 09:00:00
tags: Linux
categories: 精华转载
toc: true
---

本文摘自《Linux/UNIX指令范例速查手册》。
### expr 
> expr: expression

`expr length "this is a test"`，计算字符串长度。 

`expr 14 % 9`，计算余数。

`expr substr "this is a test" 3 5`，从位置处抓取字串。

`expr index "testforthegame" e`，计算第一个e出现的位置。

### find
> find: find

`find . -name *.c`，将目前目录及其子目录下所有扩展名是.c的文件列出来。

`find / -name mysql.sock`，在整个系统中查找mysql.sock文件。
　　
`find . -type f`，将目前目录其其下子目录中所有一般文件列出。 
　　
`find . -ctime -20`，将目前目录及其子目录下所有最近20分钟内更新过的文件列出。　 




