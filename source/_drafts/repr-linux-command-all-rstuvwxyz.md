title: Linux命令大全——RSTUVWXYZ
date: 2013-07-16 11:00:00
tags: Linux
categories: 精华转载
toc: true
---

本文摘自《Linux/UNIX指令范例速查手册》。

### rm
> rm: remove

`rm -i *.c`，删除所有.c文件，删除前逐一询问确认。
　　 
`rm -rf finished`，将 finished 目录及子目录中所有文件删除，不再确认。
　　 

### rmdir
> rmdir: remove directory

`rmdir AAA`，将当前目录下名为 AAA 的目录删除。
　　 
`rmdir -p BBB/Test`，在当前目录下的 BBB 目录中，删除名为 Test 的子目录。若 Test 删除后，BBB 目录成为空目录，则 BBB 也删除。 

### split 
> split: split

`split -b 1000 vsftpd.conf`，将vsftpd.conf分割为小文件，每个文件最大为1000Byte。

`split -l 30 -d vsftpd.conf`，将vsftpd.conf分割为小文件，每个小文件的行数最多为30行，且小文件的文件名以数字来区别。

### touch
> touch: touch

`touch file`，新建file。

```
touch -d "6:03pm" file 
touch -d "05/06/2000" file 
touch -d "6:03pm 05/06/2000" file 
```
将 file 的时间记录改成 5 月 6 日 18 点 3 分，公元两千年。
时间可以使用 am，pm 或是 24 小时的格式，日期可以使用其他格式如 6 May 2000。 
　　
### sleep 
> sleep: sleep

`date;sleep 10s;date`，显示目前时间后延迟10秒，之后再次显示时间。

### time
> time: time
 
`time ps -aux`，获得执行`ps -aux`的结果和所花费的系统资源。


### uptime
> uptime: up time

`uptime`，显示系统已运行的时间。

### last 
> last: last

`last`，显示所有曾登录的账号。

### passwd 
> passwd: password

`passwd`，一般账号修改密码。

`passwd mark`，修改账号mark的密码。

`passwd -l peter`，将peter账号停用。

`passwd -u peter`，将peter账号启用。

### who
> who: who

`who`，显示当前登录系统的账户。

`who -q`，显示当前登录的用户名称及总人数。

`who -r`，显示当前的运行等级。

### mesg
> mesg: message

`mesg`，查看其他人对当前终端机的写入权限。

`mesg n`，关闭其他人对当前终端机的写入权限。

### mail
> mail: mail

`mail -s "test mail" voidking@qq.com`，信件主题为“test mail”，然后输入右键内容。信件结束时，输入一个点并按enter键。然后输入发件人的email地址，没有就按enter键。 

如果没有mail命令，需要先安装mailx，`yum install mailx`。

### wall 
> wall: wall

`wall hi`，传讯息"hi"给每一个用户。

### write
> write: write

`write Rollaend`，传讯息给Rollaend，此时 Rollaend 只有一个连线。接下来就是将讯息打上去，结束按 ctrl+c。

`write Rollaend pts/2`，传讯息给 Rollaend，Rollaend 的连线有 pts/2，pts/3。
注意：若对方设定 mesg n，则此时讯息将无法传给对方。

### kill 
> kill: kill

`ps -ef | grep test.sh`，`kill -9 6887`，将 pid 为 6887 的进程强制终止。
　　 
`kill -HUP 456`，将 pid 为 456 的行程重启（restart）。

### nice
> nice: nice

`nice -n 1 ls`，将 ls 的优先级加 1 并执行。
　　
注意:优先序（priority）为作业系统用来决定CPU分配的参数，Linux使用轮询调度（round-robin）算法来做CPU排程，优先级越高，所可能获得的CPU时间就越多。 

### ps 
> ps: process

`ps`，显示当前账号所运行的进程。 

`ps -ef`，完整地列出所有账号的进程。

`ps -aux`，列出所有账号的进程，以及该进程所有的CPU与内存比例。

### pstree 
> pstree: process tree

`pstree`，以树状表示当前目前的进程运行状况。

### renice 
> reince: renice
 
`renice -1 1771`，将进程号码为1772的进程的优先级改为-1。

注意：renice命令可以用来调整进程的优先级，范围为-20（最高）到19（最低）。

### top 
> top: top

`top`，显示所有的进程与统计信息。

`top -u smmsp`，显示账号smmsp所运行的进程与统计信息。

### tr
> tr: translation

`echo "this is a test" | tr a-z A-Z > test.txt`，转换“this is a test”为大写，并且存入test.txt文件。

`cat test.txt | tr -d this`，去掉字符串中的t、h、i、s四个字符。

`tr -s "this" "TEST"`，字符串中的t换成T、h换成E，i换成S，s换成T。

