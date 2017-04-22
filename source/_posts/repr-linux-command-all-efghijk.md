title: Linux命令大全——EFGHIJK
date: 2013-07-16 09:00:00
tags: Linux
categories: 精华转载
toc: true
---

本文摘自《Linux/UNIX指令范例速查手册》。
## E
### e2fsck
> e2fsck: ext2 file system check，检查ext2和ext3文件系统

`e2fsck /dev/sda1`，检查/dev/sda1的状态是否正常。如果发现异常，则会询问是否修复。

### e2label
> e2label: ext2 label，设置ext2和ext3文件系统卷标

`e2label /dev/sda1 Boot`，将文件系统/dev/sda1的卷标设置为Boot。

`e2label /dev/sda1`，显示文件系统/dev/sda1的卷标。

### echo
> echo: echo，显示文字

`echo "This is a test"`，将字符串This is a test显示到屏幕上。

`echo "Test: \t example1\nTest: \t example2"`，将字符串进行格式化的编排。

`w`，`echo "This is a test" > /dev/pts/1`，将字符串This is a test显示到其他终端机/dev/pts/1上。

### ed
> ed: editor，文本编辑

`ed file`，编辑文件file。

ed不常用，一般使用vi。

### edquota
> edquota: edit quota，编辑账号或组所能使用的硬盘容量。

`edquota karen`，修改账号karen的quota用量。

`edquota -p karen john`，将karen的设置套用在john上。

### egrep
> egrep: grep -e，查找文件中的特定字符串

`egrep 127.0 /etc/*`，列出/etc下包含127.0字符串的所有文件。

### eject
> eject: eject，光驱的弹出与收回

`eject`，弹出光驱。

`eject -j`，收回光驱。

`eject /dev/cdrom1`，弹出指定光驱。

### emerge
> emerge: emerge，软件包安装与管理命令

`emerge --sync`，同步目前最新软件包名称。

`emerge -pv apache`，`emerge -u apache`，将apache升级到最新版本。

`emerge -u world`，将所有软件包升级到最新版本。

### enable
> enable: enable，启动或关闭shell的默认命令

`enable -a`，显示当前shell的所有启动的命令。

`enable -n cd`，关闭shell内置的命令cd。

### eval
> eval: evaluate，运算求出参数的内容

```
a1 = "This is a book"
a2 = \$a1
echo $a2
eval echo $a2
```

### ex
> ex: vi in execution mode，文件编辑

`ex file1`，编辑文件file1。

ex相当于vi -e。

### exit
> exit: exit，退出当前shell

`exit`，退出并关闭当前的窗口。

### export
> export: export，设置环境变量

`export exp=2.71828`，`echo $exp`，将变量exp设置为2.71828。

`export`，列出当前的环境变量。

### expr 
> expr: expression，求表达式变量的值

`expr length "this is a test"`，计算字符串长度。 

`expr 14 % 9`，计算余数。

`expr substr "this is a test" 3 5`，从位置处抓取字串。

`expr index "testforthegame" e`，计算第一个e出现的位置。


## F
### fc
> fc: first command，修改或使用曾经使用的命令

`fc -l`，列出运行过的指令。

`fc -e vi`，用vi修改最后运行的指令，修改完自动运行。

### fdisk
> fdisk: formatted disk，设置硬盘分区

`fdisk -l`，列出第一块SCSI硬盘上的分区表。

`fdisk /dev/sda`，进入分区管理。

- 输入 m 显示所有命令提示。
- 输入 p 显示硬盘分割情形。
- 输入 a 设定硬盘启动区。
- 输入 n 设定新的硬盘分割区。输入 e 硬盘为[延伸]分割区(extend)，输入 p 硬盘为[主要]分割区(primary)。
- 输入 t 改变硬盘分割区属性。
- 输入 d 删除硬盘分割区属性。
- 输入 q 结束不存入硬盘分割区属性。
- 输入 w 结束并写入硬盘分割区属性。

### fg
> fg: front ground，将进程放到前台运行

`tail -f /var/log/maillog &`，`fg tail`，将该进程放到前台运行。

### fgrep
> fgrep: grep -f，查找文件中的字符串

`fgrep 127.0 /etc/*`，列出/etc下文件中包含字符串127.0的所有文件。

### file
> file: file，显示文件类型

`file /etc/hosts`，显示一般文件。

`file /etc/view`，显示连接文件。

### filefrag
> filefrag: file fragment，显示文件的破碎状态

`filefrag -v backupfile`，检查文件backupfile的破碎状态。

### find
> find: find，查找特定的文件或目录名称

`find . -name *.c`，将目前目录及其子目录下所有扩展名是.c的文件列出来。

`find / -name mysql.sock`，在整个系统中查找mysql.sock文件。
　　
`find . -type f`，将目前目录其其下子目录中所有一般文件列出。 
　　
`find . -ctime -20`，将目前目录及其子目录下所有最近20分钟内更新过的文件列出。　 

### findfs
> findfs: find file system，用标签或UUID查找文件系统

`findfs LABEL=/`，查找名为/的文件系统。

`findfs LABEL=SWAP-sda6`，查找名为SWAP-sda6的文件系统。

### finger
> finger: finger，远程查询主机上的账号信息

`finger scfeng@localhost`，查询本机账号scfeng的状态。

finger是早期远程查询命令，近年来由于安全考虑，几乎没有用户安装finger软件包。

### fixfiles
> fixfiles: fix files SELinux context，修正文件的SELinux标签

`fixfiles restore /etc/vsftpd/*`，修正/etc/vsftpd/目录下所有文件的标签。

### fmt
> fmt: formatter，文件编排

`cat file`，`fmt -w 30 file`，进行固定宽度文件编排。

### fold
> fold: fold，修改文件的显示宽度

`cat file`，`fold -w 20 file`，进行固定宽度文件编排。

### free
> free: free，显示内存使用状况

`free`，查看内容使用状况。

`free -t`，查询内存目前的状态，并列出物理内存与虚拟内存的总和。

### fsck
> fsck: file system check，检查或修复文件系统

在ext2文件系统下，和e2fsck功能完全相同。

### ftp
> ftp: file transferring protocol，文件传输

`ftp 10.0.0.2`，`put file`，`bye`，使用ftp上传一个名为file的文件。

### ftpcount
> ftpcount: FTP count，显示当前使用FTP的人数

`ftpcount`，查看当前登录FTP的人数。

### ftpshut
> ftpshut: FTP shutdown，停止ProFTP服务器

`ftpshut -d 3`，3min后关闭FTP服务器。

### ftpwho
> ftpwho: FTP who，显示当前登录FTP的名单

`ftpwho`，查看当前登录FTP的名单。

### fuser
> fuser: file and process user，通过文件或sockets确认进程

`fuser -l`，列出可以使用的系统信号。

`fuser -km /home`，删除与/home有关的程序。

## G
### gcc
> gcc: GNU cc complier，C语言编译工具

`gcc count.c`，将文件count.c编译为可执行文件。

`./a.out`，运行a.out。

`gcc count.c -o cc`，将文件count.c编译为可执行文件，并指定可执行文件的名称为cc。

### getsebool
> getsebool: get SELinux boolean，显示SELinux的布尔值

`getsebool ftp_home_dir`，显示是否允许通过vsftpd连接到账号的家目录。

`getsebool httpd_enable_cgi`，显示是否允许httpd执行cgi script。

### gpasswd
> gpasswd: group password，管理/etc/group的高级工具

`gpasswd elex`，修改elex组的组密码。

`gpasswd -a feng users`，将账号feng到users组中。

`gpasswd -d feng users`，将feng从users组中删除。

`gpasswd -A feng users`，将feng设为users组中管理员。

### gpm
> gpm: graphic cut and paste manager，设置鼠标的粘贴功能

`gpm -t ps2`，启动PS/2鼠标。

### grep
> grep: global search regular expression，查找文件中的字符串

`grep -c sum count.c`，显示count.c中包含字符串sum的行数。

`grep -v sum count.c`，显示count.c中不含字符串sum的行。

`grep -f file1 file2`，搜寻file2中与file1有相同字符串的内容。

### groupadd
> groupadd: group add，新建组

`groupadd admin`，新建名为admin的组。

`groupadd -r super`，新建一个名为super的系统组。

`groupadd -g 566 spot`，新建一个组号为566，名为spot的组。

### groupdel
> groupdel: group del，删除组

`groupdel admin`，删除名为admin的组。

### groupmod
> groupmod: group mode，修改组的高级内容

`groupmod -n admin super`，将组super的名称改成admin。

`groupmod -g 666 spot`，将组spot的组号改为666。

### groups
> groups: groups，显示账号所属的组

`groups admin`，显示账号admin所属的组名称。

### grpconv
> grpconv: group convert，转换为组投影密码

### gunzip
> gunzip: GNU un-zip，解压缩gz文件

`gunzip -l /var/log/* .gz`，显示目录/var/log下所有的gz文件的信息。

`gunzip -c file.gz > file2`，将file.gz解压缩，并保留原压缩文件。

`gunzip -r /home/mark`，将/home/mark下的所有gz文件全部解压缩。

`gunzip -v file.gz`，将file.gz解压缩，并显示过程。

### gzexe
> gzexe: GNU zip execution，运行压缩文件

`gzexe -d a.out`，运行已压缩可执行文件的a.out。

### gzip
> gzip: GNU zip，压缩gz的文件

`gzip -v output`，压缩output，并显示压缩过程。

`gzip h*`，压缩当前目录下所有文件名以h开头的文件。

`gzip -9 backupfile1`，指定压缩率压缩文件。

`gzip -l /var/log/*.gz`，显示目录/var/log/下所有gz文件的信息。

## H
### halt
> halt: halt，关闭系统

`halt -p`，关闭系统并关闭电源。

`halt -d`，关闭系统，并且不记录在日志文件/var/log/wtmp中。

`halt -n`，不将内存数据写入硬盘，直接关闭系统。

### hash
> hash: hash table

`hash -l`，显示记忆的命令。

`hash -t cat`，列出命令cat的路径。

### hdparm
> hdparm: hard disk parameter，显示或设置硬盘参数

`hdparm -t /dev/sda`，评估硬盘的读取效率。

`hdparm -d 1 /dev/sda`，启动硬盘的DMA模式。

`hdparm -I /dev/sda`，侦测硬盘的规格。

`hdparm -C /dev/sda`，侦测IDE硬盘电源管理模式。

### head
> head: head of file，输出文件内容前面的内容

`head -n 3 install.log`，显示前3行内容。

`head -c 30 install.log`，显示前30字节的内容。

### help
> help: help，shell内置命令说明

`help alias`，显示alias命令的说明。

### history
> history: history，列出使用过的命令

`history 5`，列出5个最近使用过的命令。

### host
> host: host，查询主机使用的域名

`host www.taobao.com 61.139.2.69`，在DNS服务器上61.139.2.69上查询地址www.taobao.com。

`host -t mx 126.com 61.139.2.69`，在机器61.139.2.69上查询网域126.com的邮件记录。

### hostid
> hostid: host id，显示主机ID

`hostid`，显示主机的识别码。

### hostname
> hostname: host name，显示或设置主机名

`hostname`，显示当前的主机名称。

`hostname -d`，显示当前的网域名称。

`hostname -i`，查询主机名对应的IP地址。

### htpasswd
> htpasswd: httpd passwd，设置Apache的账户密码

`htpasswd -c /etc/htpasswd jack`，新建一个Apache登录账号jack。

### httpd
> httpd: HTTP deamon，管理Apache网页服务器。

`httpd -v`，显示当前的apache详细信息。

`httpd -f /opt/httpd.conf`，使用指定的配置文件启动httpd。

`httpd -t`，测试配置文件的语法是否正确。

`httpd -l`，显示httpd编译时所包含的模块。

### hwclock
> hwclock: hardware clock，显示或设置硬件时间

`hwclock`，显示硬件日期与时间。

`hwclock --set --date="5/1/11 12:15:01"`，将硬件时钟修改为2011年5月1日12点15分01秒。


## I
### iconv
> iconv: internet conversion，字符集的转换

`iconv -l`，列出所有支持的格式。

### id
> id: identity，显示账号与组的ID

`id -a`，显示所有的账号信息。

`id -g`，显示账号所属的主组代码。

`id -u`，显示账号代码。

### ifconfig
> ifconfig: interface configuration，设置或查看网络配置

`ifconfig`，显示当前的网络设备及其状态。

`ifconfig eth0 192.168.1.5 netmask 255.255.255.0`，将IP地址设置为192.168.1.5，子网掩码设置为255.255.255.0。

`ifconfig eth0`，显示eth0的状态。

`ifconfig eth0 down`，将eth0停用。

### info
> info: information，显示在线帮助信息

`info kill`，查看kill的在线帮助信息。

### init
> init: initial，改变系统的运行等级

`init 0`，关闭计算机。

`init 6`，重新开机。

`init 1`，进入单用户模式。

### insmod
> insmod: insert module，价值模块

`insmod brdcom.ko`，加载模块brdcom.ko。

### ip
> ip: internet protocol，显示或设置网络设备的路由策略

- ip link：网络设备设置。
- ip address：IP地址的管理。
- ip route：路由表的管理。
- ip neighbour：邻近地址与ARP表的管理。
- ip tunnel：IP通道设置。
- ip maddr：组广播地址的管理。
- ip rule：组广播地址的管理。
- ip mroute：列出组路由地址。

`ip address show`，显示当前网络地址的设置。

`ip route show`，显示当前的路由列表。

`ip route add 172.16.1.0/24 via 192.168.1.1`，多重路由的设置：发往172.16.1.0/24的数据包，一律通过192.168.1.1传送。

### ipcrm
> ipcrm: interprocess communication remove，删除指定ID的IPC进程。

`ipcs`，`ipcrm -m 262149`，显示内部程序目前的状态，并将其中的共享内存删除。

### ipcs
> ipcs: interprocess communication status，显示IPC的状态

`ipcs`，显示内部程序目前的状态。

### iptab
> iptab: IP table，显示子网掩码的种类

`iptab`，显示子网掩码的种类。

### iptables
> iptables: IP tables，数据包处理与安全管理

`iptables -L`，显示当前iptables的设置。

`iptables -F`，`iptables -X`，将iptables中过滤表格的规则清楚。

```
echo "1" > /proc/sys/net/ipv4/ip_forwarding
iptables -t nat -A POSTROUTING -o eth0 -s 10.1.1.1/24 -j MASQUERADE
```
开启NAT功能，设置10.1.1.1~10.1.1.254可通过本机连接到互联网。

```
iptables -A INPUT -p tcp --dport 25 -j ACCEPT
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -p tcp -i eth0 -j DROP
```
仅开启SMTP与HTTP的连接，关闭其他端口的连接。

`iptables -A input -d 140.111.1.1 -p tcp -j DROP`，不得连到IP地址140.111.1.1。

### iptables-save
> iptables-save: IP tables save，保存当前iptables的规则

`iptables-save`，保存当前iptables的规则。

### isosize
> isosize: ISO size，显示iso9600格式的文件系统大小

`isosize /dev/hdc`，显示当前光盘的容量。

## J
### jobs
> jobs: job status，显示正在后台运行的任务

`jobs`，显示在后台运行的任务。

`jobs -p`，仅列出在后台运行的任务的PID。

### join
> join: join，合并两个文件中相同的区域

`join -t ':' /etc/passwd /etc/shadow`，将两个文件结合，以冒号作为字符串的分隔符。

## K
### kill
> kill: kill，传送信息给进程

`kill -l`，列出所有的信号与代码。

`ps -ef | grep mysql`，`kill -9 6887`，查看mysql的PID，并且结束该PID。

### killall
> killall: kill all，根据给定名称终止进程

`killall -9 ntop`，将所有关于ntop命令的程序删除。