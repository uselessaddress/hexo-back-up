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

 
　　例子.1:
　　传讯息给 Rollaend，此时 Rollaend 只有一个连线:
　　write Rollaend 


　　接下来就是将讯息打上去,结束请按 ctrl+c 

　　例子.2 :传讯息给 Rollaend,Rollaend 的连线有 pts/2,pts/3:
　　write Rollaend pts/2

　　接下来就是将讯息打上去,结束请按 ctrl+c 
　　注意:若对方设定 mesg n,则此时讯席将无法传给对方 

### 名称：kill 
　　使用权限：所有用户 
　　使用方式： 
　　kill [ -s signal | -p ] [ -a ] pid ... 
　　kill -l [ signal ] 
　　说明：kill 送出一个特定的信号 (signal) 给行程 id 为 pid 的行程根据该信号而做特定的动作, 若没有指定, 预设是送出终止 (TERM) 的信号 
　　把计: 
　　-s (signal):其中可用的讯号有 HUP (1), KILL (9), TERM (15), 分别代表着重跑, 砍掉, 结束; 详细的信号可以用 kill -l 
　　-p:印出 pid , 并不送出信号 
　　-l (signal):列出所有可用的信号名称 
　　范例： 
　　将 pid 为 323 的行程砍掉 (kill):
　　kill -9 323 
　　将 pid 为 456 的行程重跑 (restart):
　　kill -HUP 456 


### 名称：nice 
　　使用权限：所有用户 

　　使用方式：nice [-n adjustment] [-adjustment] [--adjustment=adjustment] [--help] [--version] [command [arg...]] 
　　说明：以更改过的优先序来执行程序, 如果未指定程序, 则会印出目前的排程优先序, 内定的 adjustment 为 10, 范围为 -20 (最高优先序) 到 19 (最低优先序) 
　　把计: 
　　-n adjustment, -adjustment, --adjustment=adjustment 皆为将该原有优先序的增加 adjustment 
　　--help 显示求助讯息 
　　--version 显示版本资讯 
　　范例： 
　　将 ls 的优先序加 1 并执行:
　　nice -n 1 ls 

　　将 ls 的优先序加 10 并执行:
　　nice ls将 ls 的优先序加 10 并执行 

　　注意:优先序 (priority) 为作业系统用来决定 CPU 分配的参数,Linux 使用『回合制(round-robin)』的演算法来做 CPU 排程,优先序越高,所可能获得的 CPU时间就越多。 

### 名称：ps 
　　使用权限：所有用户 
　　使用方式：ps [options] [--help] 
　　说明：显示瞬间行程 (process) 的动态 
　　参数： 
　　ps 的参数非常多, 在此仅列出几个常用的参数并大略介绍含义 
　　-A 列出所有的行程 
　　-w 显示加宽可以显示较多的资讯 
　　-au 显示较详细的资讯 
　　-aux 显示所有包含其他用户的行程 

　　au(x) 输出格式:

　　USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND 
　　USER: 行程拥有者 
　　PID: pid 
　　%CPU: 占用的 CPU 使用率 
　　%MEM: 占用的记忆体使用率 
　　VSZ: 占用的虚拟记忆体大小 
　　RSS: 占用的记忆体大小 
　　TTY: 终端的次要装置号码 (minor device number of tty) 
　　STAT: 该行程的状态: 
　　D: 不可中断的静止 (通悸□□缜b进行 I/O 动作) 
　　R: 正在执行中 
　　S: 静止状态 
　　T: 暂停执行 
　　Z: 不存在但暂时无法消除 
　　W: 没有足够的记忆体分页可分配 
　　<: 高优先序的行程 
　　N: 低优先序的行程 
　　L: 有记忆体分页分配并锁在记忆体内 (即时系统或捱A I/O) 
　　START: 行程开始时间 
　　TIME: 执行的时间 
　　COMMAND:所执行的指令 

　　范例： 

　　ps 
　　PID TTY TIME CMD 
　　2791 ttyp0 00:00:00 tcsh 
　　3092 ttyp0 00:00:00 ps 
　　% ps -A 
　　PID TTY TIME CMD 
　　1 ? 00:00:03 init 
　　2 ? 00:00:00 kflushd 
　　3 ? 00:00:00 kpiod 
　　4 ? 00:00:00 kswapd 
　　5 ? 00:00:00 mdrecoveryd 
　　....... 
　　% ps -aux 
　　USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND 
　　root 1 0.0 0.7 1096 472 ? S Sep10 0:03 init [3] 
　　root 2 0.0 0.0 0 0 ? SW Sep10 0:00 [kflushd] 
　　root 3 0.0 0.0 0 0 ? SW Sep10 0:00 [kpiod] 
　　root 4 0.0 0.0 0 0 ? SW Sep10 0:00 [kswapd] 
　　........ 


### 名称：pstree 
　　使用权限：所有用户 
　　使用方式： 
　　pstree [-a] [-c] [-h|-Hpid] [-l] [-n] [-p] [-u] [-G|-U] [pid|user] 
　　pstree -V 
　　说明：将所有行程以树状图显示, 树状图将会以 pid (如果有指定) 或是以 init 这个基本行程为根 (root) ,如果有指定用户 id , 则树状图会只显示该用户所拥有的行程 
　　参数： 
　　-a 显示该行程的完整指令及参数, 如果是被记忆体置换出去的行程则会加上括号 
　　-c 如果有重覆的行程名, 则分开列出 (预设值是会在前面加上 * 
　　范例： 

　　pstree 

　　init-+-amd 
　　|-apmd 
　　|-atd 
　　|-httpd---10*[httpd] 
　　%pstree -p 
　　init(1)-+-amd(447) 
　　|-apmd(105) 
　　|-atd(339) 
　　%pstree -c 
　　init-+-amd 
　　|-apmd 
　　|-atd 
　　|-httpd-+-httpd 
　　| |-httpd 
　　| |-httpd 
　　| |-httpd 
　　.... 



### 名称：renice 
　　使用权限：所有用户 

　　使用方式：renice priority [[-p] pid ...] [[-g] pgrp ...] [[-u] user ...] 

　　说明：重新指定一个或多个行程(Process)的优先序(一个或多个将根据所下的参数而定) 
　　把计: 
　　-p pid 重新指定行程的 id 为 pid 的行程的优先序 
　　-g pgrp 重新指定行程群组(process group)的 id 为 pgrp 的行程 (一个或多个) 的优先序 
　　-u user 重新指定行程拥有者为 user 的行程的优先序 
　　范例： 
　　将行程 id 为 987 及 32 的行程与行程拥有者为 daemon 及 root 的优先序号码加 1:
　　renice +1 987 -u daemon root -p 32 
　　注意:每一个行程(Process)都有一个唯一的 (unique) id 


### 名称：top 
　　使用权限：所有用户 
　　使用方式：top [-] [d delay] [q] [c] [S] [s] [i] [n] [b] 
　　说明：即时显示 process 的动态 
　　把计: 
　　d:改变显示的更新速度,或是在交谈式指令列( interactive command)按 s 
　　q:没有任何延迟的显示速度,如果用户是有 superuser 的权限,则 top 将会以最高的优先序执行 
　　c:切换显示模式,共有两种模式,一是只显示执行档的名称,另一种是显示完整的路径与名称S:累积模式,会将己完成或消失的子行程 ( dead child process ) 的 CPU time 累积起来 
　　s:安全模式,将交谈式指令取消, 避免潜在的危机 
　　i:不显示任何闲置 (idle) 或无用 (zombie) 的行程 
　　n:更新的次数,完成后将会退出 top 
　　b:批次档模式,搭配 "n" 参数一起使用,可以用来将 top 的结果输出到文件内 
　　范例： 
　　显示更新十次后退出 ; 
　　top -n 10 

　　用户将不能利用交谈式指令来对行程下命令:
　　top -s 

　　将更新显示二次的结果输入到名称为 top.log 的文件里:
　　top -n 2 -b < top.log 

### 名称：skill 
　　使用权限：所有用户 
　　使用方式： skill [signal to send] [options] 选择程序的规则 
　　说明： 

　　送个讯号给正在执行的程序,预设的讯息为 TERM (中断) , 较常使用的讯息为 HUP , INT , KILL , STOP , CONT ,和 0 
　　讯息有三种写法:分别为 -9 , -SIGKILL , -KILL , 可以使用 -l 或 -L 已列出可使用的讯息。 
　　一般参数： 
　　-f 快速模式/尚未完成 
　　-i 互动模式/ 每个动作将要被确认 
　　-v 详细输出/ 列出所选择程序的资讯 
　　-w 智能警告讯息/ 尚未完成 
　　-n 没有动作/ 显示程序代号 
　　参数：选择程序的规则可以是, 终端机代号,用户名称,程序代号,命令名称。 
　　-t 终端机代号 ( tty 或 pty ) 
　　-u 用户名称 
　　-p 程序代号 ( pid ) 
　　-c 命令名称 可使用的讯号: 
　　以下列出已知的讯号名称,讯号代号,功能。 

　　名称 (代号) 功能/ 描述 
　　ALRM 14 离开 
　　HUP 1 离开 
　　INT 2 离开 
　　KILL 9 离开/ 强迫关闭 
　　PIPE 13 离开 
　　POLL 离开 
　　PROF 离开 
　　TERM 15 离开 
　　USR1 离开 
　　USR2 离开 
　　VTALRM 离开 
　　STKFLT 离开/ 只适用于i386, m68k, arm 和 ppc 硬体 
　　UNUSED 离开/ 只适用于i386, m68k, arm 和 ppc 硬体 
　　TSTP 停止 /产生与内容相关的行为 
　　TTIN 停止 /产生与内容相关的行为 
　　TTOU 停止 /产生与内容相关的行为 
　　STOP 停止 /强迫关闭 
　　CONT 从新启动 /如果在停止状态则从新启动,否则忽略 
　　PWR 忽略 /在某些系统中会离开 
　　WINCH 忽略 
　　CHLD 忽略 
　　ABRT 6 核心 
　　FPE 8 核心 
　　ILL 4 核心 
　　QUIT 3 核心 
　　SEGV 11 核心 
　　TRAP 5 核心 
　　SYS 核心 /或许尚未实作 
　　EMT 核心 /或许尚未实作 
　　BUS 核心 /核心失败 
　　XCPU 核心 /核心失败 
　　XFSZ 核心 /核心失败 
　　范例： 
　　停止所有在 PTY 装置上的程序 
　　skill -KILL -v pts/* 
　　停止三个用户 user1 , user2 , user3 
　　skill -STOP user1 user2 user3 

　　其他相关的命令: kill 

### 名称：expr 
　　使用权限：所有用户 
　　字串长度 
　　shell>> expr length "this is a test" 
　　14 

　　数字商数 
　　shell>> expr 14 % 9 
　　5 

　　从位置处抓取字串 
　　shell>> expr substr "this is a test" 3 5 
　　is is 

　　数字串 only the first character 

　　shell>> expr index "testforthegame" e 
　　2 

　　字串真实重现 
　　shell>> expr quote thisisatestformela 
　　thisisatestformela 


### 名称: tr 
　　1.比方说要把目录下所有的大写档名换为小写档名? 

　　似乎有很多方式,"tr"是其中一种: 
　　#!/bin/sh 
　　dir="/tmp/testdir"; 
　　files=`find $dir -type f`; 
　　for i in $files 
　　do 
　　dir_name=`dirname $i`; 
　　ori_filename=`basename $i` 
　　new_filename=`echo $ori_filename | tr [:upper:] [:lower:]` > /dev/null; 
　　#echo $new_filename; 
　　mv $dir_name/$ori_filename $dir_name/$new_filename 
　　done 

　　2.自己试验中...lowercase to uppercase 

　　tr abcdef...[del] ABCDE...[del] 
　　tr a-z A-Z 
　　tr [:lower:] [:upper:] 

　　shell>> echo "this is a test" | tr a-z A-Z > www 
　　shell>> cat www 
　　THIS IS A TEST 

　　3.去掉不想要的字串 
　　shell>> tr -d this ### 去掉有关 t.e.s.t 
　　this 

　　man 
　　man 
　　test 
　　e 

　　4.取代字串 
　　shell>> tr -s "this" "TEST" 
　　this 
　　TEST 
　　th 
　　TE 

### 名称：clear 
　　用途：清除萤幕用。 
　　使用方法：在 console 上输入 clear。 

### 名称: reset, tset 
　　使用方法: tset [-IQqrs] [-] [-e ch] [-i ch] [-k ch] [-m mapping] [terminal] 
　　使用说明: 
　　reset 其实和 tset 是一同个命令,它的用途是设定终端机的状态。一般而言,这个命令会自动的从环境变数,命令列或是其它的组态档决定目前终端机的型态。如果指定型态是 ? 的话,这个程序会要求用户输入终端机的型别。 

　　由于这个程序会将终端机设回原始的状态,除了在 login 时使用外,当系统终端机因为程序不正常执行而进入一些奇怪的状态时,你也可以用它来重设终端机o 例如不小心把二进位档用 cat 指令进到终端机,常会有终端机不再回应键盘输入,或是回应一些奇怪字元的问题。此时就可以用 reset 将终端机回复至原始状态。选项说明: 
　　-p 
　　将终端机类别显示在萤幕上,但不做设定的动作。这个命令可以用来取得目前终端机的类别。
　　-e ch 
　　将 erase 字元设成 ch 
　　-i ch 
　　将中断字元设成 ch 
　　-k ch 
　　将删除一行的字元设成 ch 
　　-I 
　　不要做设定的动作,如果没有使用选项 -Q 的话,erase,中断及删除字元的目前值依然会送到萤幕上。 
　　-Q 
　　不要显示 erase,中断及删除字元的值到萤幕上。 
　　-r 
　　将终端机类别印在萤幕上。 
　　-s 
　　将设定 TERM 用的命令用字串的型式送到终端机中,通常在 .login 或 .profile 中用 
　　范例: 
　　让用户输入一个终端机型别并将终端机设到该型别的预设状态。 
　　# reset ? 

　　将 erase 字元设定 control-h 
　　# reset -e ^B 

　　将设定用的字串显示在萤幕上 
　　# reset -s 
　　Erase is control-B (^B). 
　　Kill is control-U (^U). 
　　Interrupt is control-C (^C). 
　　TERM=xterm; 

### 名称：compress 
　　使用权限：所有用户 
　　使用方式：compress [-dfvcV] [-b maxbits] [file ...] 
　　说明： 
　　compress 是一个相当古老的 unix 文件压缩指令,压缩后的文件会加上一个 .Z 延伸档名以区别未压缩的文件,压缩后的文件可以以 uncompress 解压。若要将数个文件压成一个压缩档,必须先将文件 tar 起来再压缩。由于 gzip 可以产生更理想的压缩比例,一般人多已改用 gzip 为文件压缩工具。
　　参数： 
　　c 输出结果至标准输出设备（一般指荧幕） 
　　f 强迫写入文件,若目的档已经存在,则会被覆盖 (force) 
　　v 将程序执行的讯息印在荧幕上 (verbose) 
　　b 设定共同字串数的上限,以位元计算,可以设定的值为 9 至 16 bits 。由于值越大,能使用的共同字串就 越多,压缩比例就越大,所以一般使用预设值 16 bits (bits) 
　　d 将压缩档解压缩 
　　V 列出版本讯息 
　　范例： 
　　将 source.dat 压缩成 source.dat.Z ,若 source.dat.Z 已经存在,内容则会被压缩档覆盖。 
　　compress -f source.dat 

　　将 source.dat 压缩成 source.dat.Z ,并列印出压缩比例。 
　　-v 与 -f 可以一起使用 
　　compress -vf source.dat 

　　将压缩后的资料输出后再导入 target.dat.Z 可以改变压缩档名。
　　compress -c source.dat > target.dat.Z 

　　-b 的值越大,压缩比例就越大,范围是 9-16 ,预设值是 16 。 
　　compress -b 12 source.dat 

　　将 source.dat.Z 解压成 source.dat ,若文件已经存在,用户按 y 以确定覆盖文件,若使用 -df 程序则会自动覆盖文件。由于系统会自动加入 .Z 为延伸档名,所以 source.dat 会自动当作 source.dat.Z 处理。 

　　compress -d source.dat 
　　compress -d source.dat.Z 

### 名称： lpd 
　　使用权限： 所有用户
　　使用方式：lpd [-l] [#port] 
　　lpd 是一个常驻的印表机管理程序,它会根据 /etc/printcap 的内容来管理本地或远端的印表机。/etc/printcap 中定义的每一个印表机必须在 /var/lpd 中有一个相对应的目录,目录中以 cf 开头的文件表示一个等待送到适当装置的印表工作。这个文件通常是由 lpr 所产生。 

　　lpr 和 lpd 组成了一个可以离线工作的系统,当你使用 lpr 时,印表机不需要能立即可用,甚至不用存在。lpd 会自动监视印表机的状况,当印表机上线后,便立即将文件送交处理。这个得所有的应用程序不必等待印表机完成前一工作。 
　　参数： 
　　-l: 将一些除错讯息显示在标准输出上。 
　　#port: 一般而言,lpd 会使用 getservbyname 取得适当的 TCP/IP port,你可以使用这个参数强迫 lpd 使用指定的 port。 
　　范例： 
　　这个程序通常是由 /etc/rc.d 中的程序在系统启始阶段执行。 

### 名称 lpq 
　　-- 显示列表机贮列中未完成的工作 用法 
　　lpq [l] [P] [user] 
　　说明 
　　lpq 会显示由 lpd 所管理的列表机贮列中未完成的项目。 
　　范例 
　　范例 1. 显示所有在 lp 列表机贮列中的工作 
　　# lpq -PlpRank Owner Job Files Total Size1st root 238 (standard input) 1428646 bytes 

　　相关函数 
　　lpr,lpc,lpd 

### 名称：lpr 
　　使用权限： 所有用户 
　　使用方式：lpr [ -P printer ] 
　　将文件或是由标准输入送进来的资料送到印表机贮列之中,印表机管理程序 lpd 会在稍后将这个文件送给适当的程序或装置处理。lpr 可以用来将料资送给本地或是远端的主机来处理。参数：
　　-p Printer: 将资料送至指定的印表机 Printer,预设值为 lp。
　　范例： 
　　将 www.c 和 kkk.c 送到印表机 lp。 
　　lpr -Plp www.c kkk.c 

### 名称: lprm 
　　-- 将一个工作由印表机贮列中移除 用法 
　　/usr/bin/lprm [P] [file...] 
　　说明 
　　尚未完成的印表机工作会被放在印表机贮列之中,这个命令可用来将常未送到印表机的工作取消。由于每一个印表机都有一个独立的贮列,你可以用 -P 这个命令设定想要作用的印列机。如果没有设定的话,会使用系统预设的印表机。 
　　这个命令会检查用户是否有足够的权限删除指定的文件,一般而言,只有文件的拥有者或是系统管理员才有这个权限。 
　　范例 
　　将印表机 hpprinter 中的第 1123 号工作移除 
　　lprm -Phpprinter 1123 

　　将第 1011 号工作由预设印表机中移除 
　　lprm 1011 

### 名称： fdformat 
　　使用权限： 所有用户 
　　使用方式：fdformat [-n] device 
　　使用说明:
　　对指定的软碟机装置进行低阶格式化。使用这个指令对软碟格式化的时候,最好指定像是下面的装置： 

　　/dev/fd0d360 磁碟机 A: ,磁片为 360KB 磁碟 
　　/dev/fd0h1440 磁碟机 A: ,磁片为 1.4MB 磁碟 
　　/dev/fd1h1200 磁碟机 B: ,磁片为 1.2MB 磁碟 
　　如果使用像是 /dev/fd0 之类的装置,如果里面的磁碟不是标准容量,格式化可能会失败。在这种情况之下,用户可以用 setfdprm 指令先行指定必要参数。 
　　参数： 
　　-n 关闭确认功能。这个选项会关闭格式化之后的确认步骤。 
　　范例： 
　　fdformat -n /dev/fd0h1440 
　　将磁碟机 A 的磁片格式化成 1.4MB 的磁片。并且省略确认的步骤。

### 名称： mformat 
　　使用权限： 所有用户 
　　使用方式： 
　　mformat [-t cylinders] [-h heads] [-s sectors] [-l volume_label] [-F] [-I fsVer-sion] [-S sizecode] [-2 sectors_on_track_0] [-M software_sector_size] [-a] [-X] [-C] [-H hidden_sectors] [-r root_sectors] [-B boot_sector] [-0 rate_on_track_0] [-A rate_on_other_tracks] [-1] [-k] drive: 

　　在已经做过低阶格式化的磁片上建立 DOS 文件系统。如果在编译 mtools 的时候把 USE_2M 的参数打开,部分与 2M 格式相关的参数就会发生作用。否则这些参数（像是 S,2,1,M）不会发生作用。
　　参数： 
　　-t 磁柱（synlider）数 
　　-h 磁头（head）数 
　　-s 每一磁轨的磁区数 
　　-l 标签 
　　-F 将磁碟格式化为 FAT32 格式,不过这个参数还在实验中。 
　　-I 设定 FAT32 中的版本号。这当然也还在实验中。 
　　-S 磁区大小代码,计算方式为 sector = 2^(大小代码+7) 
　　-c 磁丛（cluster）的磁区数。如果所给定的数字会导致磁丛数超过 FAT 表的限制,mformat 会自动放大磁区数。 
　　-s 
　　-M 软体磁区大小。这个数字就是系统回报的磁区大小。通常是和实际的大小相同。 
　　-a 如果加上这个参数,mformat 会产生一组 Atari 系统的序号给这块软碟。 
　　-X 将软碟格式化成 XDF 格式。使用前必须先用 xdfcopy 指令对软碟作低阶格式化的动作。 
　　-C 产生一个可以安装 MS-DOS 文件系统的磁碟影像档（disk image）。当然对一个实体磁碟机下这个参数是没有意义的。 
　　-H 隐藏磁区的数目。这通常适用在格式化硬碟的分割区时,因为通常一个分割区的前面还有分割表。这个参数未经测试,能不用就不用。 
　　-n 磁碟序号 
　　-r 根目录的大小,单位是磁区数。这个参数只对 FAT12 和 FAT16 有效。 
　　-B 使用所指定的文件或是设备的开机磁区做为这片磁片或分割区的开机磁区。当然当中的硬体参数会随之更动。 
　　-k 尽量保持原有的开机磁区。 
　　-0 第 0 轨的资料传输率 
　　-A 第 0 轨以外的资料传输率 
　　-2 使用 2m 格式 
　　-1 不使用 2m 格式 
　　范例： 
　　mformat a: 
　　这样会用预设值把 a: （就是 /dev/fd0）里的磁碟片格式化。 

### 名称： mkdosfs 
　　使用权限： 所有用户 
　　使用方式： mkdosfs [ -c | -l filename ] 
　　[ -f number_of_FATs ] 
　　[ -F FAT_size ] 
　　[ -i volume_id ] 
　　[ -m message_file ] 
　　[ -n volume_name ] 
　　[ -r root_dir_entry ] 
　　[ -s sector_per_cluster ] 
　　[ -v ] 
　　device 
　　[ block_count ] 
　　说明： 建立 DOS 文件系统。 device 指你想要建立 DOS 文件系统的装置代号。像是 /dev/hda1 等等。 block_count 则是你希望配置的区块数。如果 block_count 没有指定则系统会自动替你计算符合该装置大小的区块数。 
　　参数： 
　　-c 建立文件系统之前先检查是否有坏轨。 
　　-l 从得定的文件中读取坏轨记录。 
　　-f 指定文件配置表（FAT , File Allocation Table）的数量。预设值为 2 。目前 Linux 的 FAT 文件系统不支援超过 2 个 FAT 表。通常这个不需要改。 
　　-F 指定 FAT 表的大小,通常是 12 或是 16 个位元组。12 位元组通常用于磁碟片,16 位元组用于一般硬碟的分割区,也就是所谓的 FAT16 格式。这个值通常系统会自己选定适当的值。在磁碟片上用 FAT16 通常不会发生作用,反之在硬碟上用 FAT12 亦然。 
　　-i 指定 Volume ID。一般是一个 4 个位元组的数字,像是 2e203a47 。如果不给系统会自己产生。 
　　-m 当用户试图用这片磁片或是分割区开机,而上面没有作业系统时,系统会给用户一段警告讯息。这个参数就是用来变更这个讯息的。你可以先用文件编辑好,然后用这个参数指定,或是用 
　　-m - 
　　这样系统会要求你直接输入这段文字。要特别注意的是,文件里的字串长度不要超过 418 个字,包括展开的跳栏符号（TAB）和换行符号（换行符号在 DOS 底下算两个字元！） 
　　-n 指定 Volume Name,就是磁碟标签。如同在 DOS 底下的 format 指令一样,给不给都可以。没有预设值。 
　　-r 指定根目录底下的最大文件数。这里所谓的文件数包括目录。预设值是在软碟上是 112 或是 224 ,在硬碟上是 512。没事不要改这个数字。 
　　-s 每一个磁丛（cluster）的磁区数。必须是 2 的次方数。不过除非你知道你在作什么,这个值不要乱给。 
　　-v 提供额外的讯息
　　范例： 
　　mkdosfs -n Tester /dev/fd0 将 A 槽里的磁碟片格式化为 DOS 格式,并将标签设为 Tester