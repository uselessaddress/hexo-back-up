title: Linux命令大全
date: 2013-07-16 15:28:54
tags: Linux
categories: 精华转载
toc: true
---
### at 
> at: at

`at 5pm + 3 days /bin/ls`，三天后的下午 5 点执行 `/bin/ls`。
　　 
`at 5pm + 3 weeks /bin/ls`，三个星期后的下午 5 点执行 `/bin/ls`。
　　 
`at 17:20 tomorrow /bin/date`，明天的 17:20 执行 `/bin/date`。
　　
`at 23:59 12/31/1999 echo the end of world !`，在1999年的最后一天的最后一分钟印出 the end of world !  

### cal 
> cal: calendar

`cal`，显示本月的月历。

`cal 2000`，显示2000年年历。

`cal 5 2001`，显示2000年5月月历。

`cal -m`，以星期一为每周的第一天方式，显示本月的月历。 

`cal -jy`，以一月一日起的天数显示今年的年历。

### cat 

> cat: catenate

`cat -n textfile1 > textfile2`，把textfile1的内容加上行号后，转存为textfile2。

`cat -b textfile1 textfile2 >> textfile3`，把textfile1和textfile2的内容加上行号（空白行不加）之后，将内容附加到textfile3的最后。

### cd 

> cd: change directory

`cd /usr/bin`，进入`/usr/bin/`目录。

`cd ~`，回到home directory。

`cd ../..`，跳到目前目录的上上两层:
　　 
<!--more-->

### chmod
> chmod: change mode

`chmod ugo+r file1.txt `，将file1.txt设为所有人可读取。

`chmod a+r file1.txt `，将file1.txt设为所有人可读取。
　　
`chmod ug+w,o-w file1.txt file2.txt`，将file1.txt与file2.txt设为文件拥有者和其所属同一个群体者可写入，但其他以外的人则不可写入。
　　 
`chmod u+x ex1.py`，将ex1.py设定为只有该文件拥有者可以执行。
　　 
`chmod -R a+r * `，将目前目录下的所有文件与子目录皆设为任何人可读取。

`chmod 777 file`，三个7，分别表示User、Group及Other的权限。
r=4，w=2，x=1。
若要rwx属性则4+2+1=7；若要rw-属性则4+2=6；若要r-x属性则4+1=7。 

`chmod a=rwx file`和`chmod 777 file`效果相同。

`chmod ug=rwx,o=x file`和`chmod 771 file`效果相同。

`chmod 4755 filename`，可使此程序具有root的权限。

### chown
> chown: change owner

`chown jessie:users file1.txt `，将文件file1.txt的拥有者设为users群体的用户jessie。
　　
`chmod -R lamport:users *`，将当前目录下的所有文件与子目录的拥有者皆设为users群体的用户lamport。

### chsh
> chsh: change shell

`chsh -l`，查看所有可用的shell。

`chsh`，然后指定使用的shell。

### cp 
> cp: copy file

`cp aaa bbb`，将文件aaa复制命名为 bbb。

`cp *.c finished`，将所有的.c文件复制到finished目录中。

### crontab
> crontab: cron table

`crontab -l`，列出自己的计划任务设置。

`crontab -e`，编辑自己的计划任务。
若要在每周六运行`/usr/bin/w >> /root/login.txt`，可设置如下：
```
* * * * 6 /usr/bin/w >> /root/login.txt
```
若要改为每天23:55运行以上命令，可设置如下：
```
55 23 * * * /usr/bin/w >> /root/login.txt
```

`crontab -u adm -r`，删除adm账号的计划任务设置。

### cut
> cut: cut

`cat log1`，正常查看文件。
```
root    pts/0        2013-04-29 00:52(192.168.222.1)
root    pts/0        2013-04-29 00:52(192.168.222.1)
root    pts/0        2013-04-29 00:52(192.168.222.1)
```

`cut -b 3,10 log1`，只取出第3、10个字节。
```
op
op
op
```

`cut -b -3 log1`，取前3个字节。
```
roo
roo
roo
```

### date
> date: date

`date`，显示当前日期和时间。

`date +%B%d`，显示月份与日数。

### find
> find: find

`find . -name *.c`，将目前目录及其子目录下所有扩展名是.c的文件列出来。

`find / -name mysql.sock`，在整个系统中查找mysql.sock文件。
　　
`find . -type f`，将目前目录其其下子目录中所有一般文件列出。 
　　
`find . -ctime -20`，将目前目录及其子目录下所有最近20分钟内更新过的文件列出。　 

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

### expr 
> expr: expression

`expr length "this is a test"`，计算字符串长度。 

`expr 14 % 9`，计算余数。

`expr substr "this is a test" 3 5`，从位置处抓取字串。

`expr index "testforthegame" e`，计算第一个e出现的位置。

### tr
> tr: translation

`echo "this is a test" | tr a-z A-Z > test.txt`，转换“this is a test”为大写，并且存入test.txt文件。

`cat test.txt | tr -d this`，去掉字符串中的t、h、i、s四个字符。

`tr -s "this" "TEST"`，字符串中的t换成T、h换成E，i换成S，s换成T。


### clear 
> clear: clear

`clear`，清屏。 

### compress 
> copress: compress
 
`compress -f source.dat`，将 source.dat 压缩成 source.dat.Z，若 source.dat.Z 已经存在，内容则会被压缩档覆盖。 
　　 
`compress -vf source.dat`，将 source.dat 压缩成 source.dat.Z ，并列印出压缩比例。

`compress -c source.dat > target.dat.Z `，指定压缩档名。
　　
`compress -b 12 source.dat `，-b 的值越大，压缩比例就越大，范围是 9-16 ，预设值是 16 。 
　　
```
compress -d source.dat 
compress -d source.dat.Z 
```
由于系统会自动加入 .Z 为延伸档名，所以 source.dat 会自动当作 source.dat.Z 处理。

将 source.dat.Z 解压成 source.dat ，若文件已经存在，用户按 y 以确定覆盖文件，若使用 -df 程序则会自动覆盖文件。