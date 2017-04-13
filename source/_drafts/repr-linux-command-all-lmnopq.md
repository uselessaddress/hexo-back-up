title: Linux命令大全——LMNOPQ
date: 2013-07-16 10:00:00
tags: Linux
categories: 精华转载
toc: true
---

本文摘自《Linux/UNIX指令范例速查手册》。

### less 
> less: less

`less ezhttp.log`，分页查看ezhttp.log文件。
使用pgup和pgdn上下翻页，使用q退出。

`ps -ef | less`，ps查看进程信息并通过less分页显示。

### ln
> ln: link

`ln -s yy zz`，将文件yy产生一个符号链接zz。 
　　 
`ln yy xx`，将文件yy产生一个硬链接zz。

### locate
> locate: locate

`locate mysql.sock`，在整个系统中查找mysql.sock的文件。 

`locate -n 100 a.out`，在整个系统中查找a.out文件，但最多只显示100个。

`locate -u`，建立资料库。

系统如果找不到locate命令，需要先安装locate，`yum install mlocate`，然后更新locate的数据库，`updatedb`。

### ls 
> ls: list

`ls -ltr s*`，列出当前工作目录下所有名称是s开头的文件，新的排后面。
　　
`ls -lR /home`，将 /home 目录以下所有目录及文件详细资料列出。
　　 
`ls -AF`，列出目前工作目录下所有文件及目录。目录名称后会加 "/"，可执行文件名称后会加"*"，链接文件后会加"@"。

### more 
> more: more

`more -s testfile`，逐页显示 testfile 的文件内容，如有连续两行以上空白行则以一行空白行显示。

`more +20 testfile`，从第 20 行开始显示 testfile 之文件内容。

### mv
> mv: move
 
`mv aaa bbb`，将文件 aaa 更名为 bbb。
　　 
`mv *.c finished`，将所有的.c文件移动到 finished 目录中。