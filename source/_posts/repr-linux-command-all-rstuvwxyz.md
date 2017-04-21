title: Linux命令大全——RSTUVWXYZ
date: 2013-07-16 11:00:00
tags: Linux
categories: 精华转载
toc: true
---

本文摘自《Linux/UNIX指令范例速查手册》。

## R

### raidstart
> raidstart: RAID start，启动软件的硬盘阵列

### raidstop
> raidstop: RAID stop，关闭软件的硬盘阵列

### rc-status
> rc-status: runlevel command status，显示服务器的启动状态

`rc-status`，列出当前运行等级下的服务状态。

`rc-status boot`，列出运行等级为boot下的服务状态。

### rc-update
> rc-update: runlevel command update，显示与控制服务器的启动状态

### rcp
> rcp: remote copy，远程复制文件或目录

### reboot
> reboot: reboot，重新启动系统

`reboot`，重新启动系统。

### renice
> renice: renice，调整正在运行进程的优先级

`renice -1 1772`，将进程号码为1772的进程优先级改为-1。

### repquota
> repquota: report of quota，检查硬盘容量限制

### resize2fs
> resize2fs: resize file system，调整文件系统的大小

```
df -h
umount /dev/sda2
e2fsck -f /dev/sda2
resize2fs /dev/sda2 100M
mount /dev/sda2 /home
df -h 
```
将文件系统/dev/sda2的大小调整为100MB。

### restore
> restore: restore，回存dump所产生的数据

`restore -r -f dump.txt`，将文件dump.txt还原到dump所备份的位置。

### rlogin
> rlogin: remote login，远程登录主机

`rlogin 10.1.1.3`，默认以当前root用户登录10.1.1.3主机。

`rlogin 10.1.1.3 -l mark`，使用mark身份登录10.1.1.3主机。

### rm
> rm: remove，删除文件或目录

`rm -i *.c`，删除所有.c文件，删除前逐一询问确认。
　　 
`rm -rf finished`，将 finished 目录及子目录中所有文件删除，不再确认。
　　 

### rmdir
> rmdir: remove directory，删除目录

`rmdir AAA`，将当前目录下名为 AAA 的目录删除。
　　 
`rmdir -p BBB/Test`，在当前目录下的 BBB 目录中，删除名为 Test 的子目录。若 Test 删除后，BBB 目录成为空目录，则 BBB 也删除。 

### rmmod
> rmmod: remove modules，删除加载的模块

### route
> route: route，显示或设置路由

`route add -net 192.168.0.0 netmask 255.255.255.0 dev eth0`，通过设备eth0在网段192.168.0.0中增加一个路由。

```
route del default gw 10.1.1.1
route add default gw 10.1.1.2
```
将原网关地址10.1.1.1改为10.1.1.2。

### rpm
> rpm: Red Had package management，管理RPM软件包

`rpm -ivh postfix-2.5.6-i386.rpm`，使用rpm命令安装postfix。

`rpm -qi wget`，显示wget详细的安装信息。

`rpm -e sendmail`，移除sendmail。

### rsh
> rsh: remote shell，远程登录的shell

`rsh matt@10.1.1.5 /bin/ls`，使用matt账号登录10.1.1.5并运行/bin/ls。

### runlevel
> runlevel: run level，显示目前的运行等级

## S
### scp
> scp: secure copy，使用加密连接复制文件

`scp testfile john@10.1.1.2:/home/john/`，将本机的testfile文件以john的身份复制到10.1.1.2（默认端口为22）上。

### screen
> screen: screen，多重窗口管理进程

`screen`，使用screen命令在同一个终端机下打开两个窗口，并运行不同的进程。

### sed
> sed: stream editor，文件内容修改

`sed '2,4d' testfile`，将testfile的第2~4行删除。

`sed 's/is/error/' testfile`，将testfile中的每行第一个is字符串换成error。

`sed 's/is/error/g' testfile`，将testfile中的所有is字符串换成error。

### service
> service: service，打开或关闭服务

`service --status-all`，显示服务器目前的状态。

`service postfix start`，启动postfix服务器。

### sestatus
> sestatus: SELinux status，显示SELinux的状态

`sestatus`，显示SELinux的当前状态。

`sestatus -b`，显示SELinux中布尔值的状态。

### set
> set: set，查看和设置环境变量

`set`，查看系统默认的环境变量。

`set SHELL "/bin/csh"`，将变量名称SHELL设为/bin/csh。

### setenforce
> setenforce: set enforce，启用或取消SELinux的限制

`setenforce 0`，取消SELinux的所有限制。

`setenforce 1`，打开SELinux的限制。

### setsebool
> setsebool: set SELinux boolean，设置SELinux的布尔值

`setsebool samba_enable_home_dirs 1`，允许samba共享账号的家目录。

`setsebool ftp_home_dir 1`，允许账号ftp进入自己的家目录。

### showmount
> showmount: show mount，显示NFS文件挂载的状态

`showmount -e 172.20.11.1`，显示172.20.11.1这台NFS服务器所共享的目录。

### shutdown
> shutdown: shut down，关闭系统

`shutdown -h now`，立即关机。

`shutdown -h 0`，立即关机。

`shutdown 5 "system will be off in 5 mins"`，5分钟后关机，并在每个终端机窗口提示。

### sleep 
> sleep: sleep，暂停计时

`date;sleep 10s;date`，显示目前时间后延迟10秒，之后再次显示时间。

### sln
> sln: static link，新建文件间的软连接

`sln /etc/hosts newhost`，建立一个静态的连接。

### slogin
> slogin: SSH login，远程加密的连接

### slocate
> slocate: security enhanced locate，寻找文件或目录

### smbpasswd
> smbpasswd: samba password，改变samba账号的密码

`smbpasswd -a devin`，在samba服务器上新建一个devin账号。

`smbpasswd -d devin`，暂停账号devin对samba服务器的使用权限。

### smbstatus
> smbstatus: samba status，显示samba服务器的状态

### sort
> sort: sort，将文本文件的内容重新排序

`sort -t: /etc/passwd`，将passwd中的内容按照账户的字母顺序排序。

`ps -ef | sort`，对于进程依照运行的账户排序。

### split 
> split: split，分割文件

`split -b 1000 vsftpd.conf`，将vsftpd.conf分割为小文件，每个文件最大为1000Byte。

`split -l 30 -d vsftpd.conf`，将vsftpd.conf分割为小文件，每个小文件的行数最多为30行，且小文件的文件名以数字来区别。

### ssh
> ssh: secure shell，远程加密的连接

`ssh 10.1.1.2`，通过ssh连接到主机10.1.1.2。

`ssh 172.20.11.1 -p 12345 -l macro`，连接到172.20.11.1，使用端口12345，并使用marco账户。

### stat
> stat: status，显示文件或文件系统的状态

`stat vsftpd.conf`，显示文件vsftpd.conf的状态。

`stat -f /dev/sda1`，显示文件系统/dev/sda1的状态。

### su
> su: substitute user，切换用户

`su`，默认切换root账号。

`su voidking`，切换为voidking账号。

### sudo
> sudo: substitute user to do something，使用指定的账号权限运行进程

`sudo -u www vi /etc/httpd/conf/httpd.conf`，使用身份www编辑/etc/httpd/conf/httpd.conf。

### sum
> sum: sum，计算并显示文件的标识符

`sum squid.conf`，显示文件的标识符。

### suspend
> suspend: suspend，暂停当前所使用的shell

`suspend -f`，停止目前的shell。

### swapoff
> swapoff: swap off，关闭交换区空间

`swapoff /dev/sda2`，关闭定义在/dev/sda2上的交换区空间。

`swapoff -a`，关闭所有定义在/dev/fstab上的交换区空间。

### swapon
> swapon: swap on，挂载交换区空间

`swapon /dev/sda2`，挂载/dev/sda2上的交换区空间。

`swapon -a`，挂载所有定义在/dev/fstab上的交换区空间。

### sync
> sync: synchronize，将内存中的数据存回硬盘

`sync`，将内存中的数据写回硬盘。

### sysctl
> sysctl: system control，设置内核参数

`sysctl -a`，列出正在使用的内核参数。

`sysctl -w net.ipv4.ip_forward=1`，将net.ipv4.ip_forward设为1。

## T
### tac
> tac: 颠倒的cat，从文件内容从尾到头显示

`tac file.txt`，将文件从尾到头以反序显示。

### tail
> tail: tail，显示文件后面的部分

`tail -n 10 file.txt`，显示文件最后10行。

`tail -f /var/log/maillog`，持续监控文件/var/log/maillog，只要文件有新内容，就在屏幕输出。

### tar
> tar: tape archive，打包文件

`tar -cvf mail.tar /var/spool/mail`，将账号的邮件文件打包为mail.tar。

`tar -xvf mail.tar`，将一个打包文件解压缩到当前目录之下。

`tar -xvf mail.tar -C ./tmp/`，将打包文件解压缩到/tmp下。

`tar -zcvf conf.tar.gz /etc/*.conf`，使用tar打包文件，并只用gzip压缩该文件。

`tar -zxvf conf.tar.gz -C ./tmp/`，将打包文件解压缩到/tmp下。

`tar -jcvf conf.tar.bz2 /etc/*.conf`，使用tar打包文件，并使用bzip2压缩该文件。

`tar -jxvf conf.tar.bz2 -C ./tmp/`，将打包文件解压缩到/tmp下。

### tcpdump
> tcpdump: TCP dump，显示网络上TCP的状态

`tcpdump -i eth0`，显示网络设备eth0上的数据包状态。

### tee
> tee: tee，读取文件并输出

`tee -a file`，接着输入附加内容，ctrl+C跳出。

### telinit
> telinit: tell init，切换系统目前的运行等级

```
runlevel
telinit 3
runlevel
```
将目前的运行等级改为3。

### telnet
> telnet: tel net，远程连接程序

`telnet 172.20.11.1`，连接到172.20.11.1上的telnet服务。

`telnet localhost 25`，连接到本机的25端口（邮件服务器使用的端口）。

### tftp
> tftp: trivial FTP，文件传输

```
# tftp localhost
> help
> quit
```
连接到本机tftp服务器。

### time
> time: time，统计时间消耗
 
`time ps -aux`，获得执行`ps -aux`的结果和所花费的系统资源。

### top
> top: top，查看目前的进程状态

`top`，显示所有的进程与统计信息。

`top -u smmsp`，显示账号smmsp所运行的进程与统计信息。

### touch
> touch: touch，更改文件的时间标记

`touch file`，新建一个名为file的文件。

`touch vsftpd.conf`，更改已存在文件的时间标记。

```
touch -d "6:03pm" file 
touch -d "05/06/2000" file 
touch -d "6:03pm 05/06/2000" file 
```
将 file 的时间记录改成 5 月 6 日 18 点 3 分，公元两千年。
时间可以使用 am，pm 或是 24 小时的格式，日期可以使用其他格式如 6 May 2000。

### tr
> tr: translation，转换或更改文件中的字符

`cat testfile | tr line abcd`，将line字符串换成abcd（l
换成a，i换成b，n换成c，e换成d）。

`echo "this is a test" | tr a-z A-Z > test.txt`，转换“this is a test”为大写，并且存入test.txt文件。

`cat test.txt | tr -d this`，去掉字符串中的t、h、i、s四个字符。

`tr -s "this" "TEST"`，字符串中的t换成T、h换成E，i换成S，s换成T。

### tracepath
> tracepath: trace path，追踪网络连接的路径

`tracepath 210.75.20.211`，追踪连接到210.75.20.211的路径。

### traceroute
> traceroute: traceroute，追踪连接所经过的路由器

`traceroute 210.75.20.211`，显示连接到210.75.20.211所经过的路由器。

### tune2fs
> tune2fs: tune ext2 file system，调整文件系统的参数

`tune2fs -l /dev/sda1`，列出/dev/sda1的相关信息。

`tune2fs -j /dev/sda1`，将文件系统由ext2调整为ext3。

## U
### ulimit
> ulimit: user limit，控制系统资源

`ulimit -a`，显示当前的系统资源使用限制。

`ulimit -l 102400`，设置所有用户所能使用的内存为102400KB。

### umask
> umask: user file creation mode mask，设置新建文件的屏蔽权限

`umask`，显示默认的文件权限。

`umask -S`，用较易阅读的方式显示文件权限。

### umount
> umount: un-mount，卸载文件系统

`umount /cdrom`，卸载已挂载的的目录/cdrom。

`umount -a`，卸载所有定义在/etc/mtab中的文件系统。

### unalias
> unalias: un-alias，删除别名设置

`unalias ll`，将已定义的别名ll删除。

### uname
> uname: UNIX name，显示系统信息

`uname -a`，显示所有信息。

`uname -i`，显示硬件平台。

### uncompress
> uncompress: un-compress，解压缩Z格式的压缩文件

`uncompress -v test.Z`，解压缩test.Z文件并且显示详细信息。

### uniq
> uniq: unique，删除文件中重复的行

`uniq testfile`，将文件唯一化（重复的行显示一行）。

`uniq -d testfile`，显示文件中重复出现的行。

### unset
> unset: un-set，删除变量设置

`unset color`，将变量color的设置删除。

### unzip
> unzip: un-zip，解压缩zip文件

`unzip -l file.zip`，显示压缩文件file.zip中的文件列表。

`unzip file.zip`，解压缩file.zip。

### uptime
> uptime: up time，显示系统已经运行的时间

`uptime`，显示系统已运行的时间。

### useradd
> useradd: user add，新建账号

`useradd -m max`，新增一个用户max。

`useradd -u 1001 -d /opt/jerry -m jerry`，新增一个用户jerry，并指定该用户UID为1001，且家目录为/opt/jerry。

### userdel
> userdel: user del，删除账号

`userdel peter`，删除用户peter。不加任何参数，仅删除该用户，不会删除该用户的家目录。

### usermod
> usermod: user mode，修改账号设置

`usermod -d /data/john john`，将用户john的登录目录改为/data/john。

`usermod -e 01/31/2018 alex`，将用户alex的有效期限设为2018年1月31日。

`usermod -l alex peter`，将账号peter改为alex。

`usermod -g users alex`，将用户alex的群组改为users。

### users
> users: user status，显示登录账号

`users`，显示登录的用户名称。

## V
### vi
> vi: view，文本编辑

`vi install.log`，编辑文件install.log。

[vi/vim编辑器常用命令与用法总结](http://www.cnblogs.com/jiayongji/p/5771444.html)

### view
> view: view，文本编辑

view是一个连接到vi的连接文件，与vi完全相同。

### vim
> vim: vi improved，文本编辑

vim是vi的升级版，用法参考vi。

### vlock
> vlock: virtual lock，锁定虚拟控制台的使用权

`vlock`，将目前的窗口锁定。

### vmstat
> vmstat: virtual memory status，显示虚拟内存的状态

`vmstat`，显示虚拟内存目前的使用状况。

`vmstat -d`，显示磁盘的使用状况。

`vmstat -s`，显示多样的数据与统计信息。

## W
### w
> w: who，显示目前登录的账号信息

`w`，显示所有用户的数据。

### wait
> wait: wait，等待程序传回信息

`wait 3305`，等待进程为3305的程序传回值。

### wall 
> wall: wall，广播信息

`wall hi`，传讯息"hi"给每一个用户。

### watch
> watch: watch，全屏输出命令的运行结果

`watch -n 5 tail /var/log/messages`，每个5s运行一次tail /var/log/messages。

### wc
> wc: word count，计算文件的字节数、字数或行数

`wc -c install.log`，显示install.log的字符数。

`wc -l install.log`，显示install.log的行数。

### wget
> wget: WWW get，从指定的网站下载文件

`wget http://apache.mirror.com/httpd/httpd-2.2.8.tar.gz`，下载httpd-2.2.8.tar.gz。

`wget -t 5 http://apache.mirror.com/httpd/httpd-2.2.8.tar.gz`，下载httpd-2.2.8.tar.gz，最多尝试5次。

### whatis
> whatis: what is，查找在线帮助的位置

`whatis kill`，寻找命令kill的在线帮助所在的位置。

### whereis
> whereis: where is，查找相关的文件

`whereis kill`，查找命令kill的相关文件。

`whereis hosts.allow`，查找host.allow的相关文件。

### which
> which: which，查找指定的文件

`which cat`，寻找文件cat。

### who
> who: who，显示目前登录的账号信息

`who`，显示当前登录系统的账户。

`who -q`，显示当前登录的用户名称及总人数。

`who -r`，显示当前的运行等级。

### whoami
> whoami: who am i，显示账号名称

`whoami`，显示自己的用户名称。

### write
> write: write，发送信息给其他账号

`write Rollaend`，传讯息给Rollaend，此时 Rollaend 只有一个连线。接下来就是将讯息打上去，结束按 ctrl+c。

`write Rollaend pts/2`，传讯息给 Rollaend，Rollaend 的连线有 pts/2，pts/3。
注意：若对方设定 mesg n，则此时讯息将无法传给对方。

## X
### xauth
> xauth: X-authentication，编辑X服务器的授权信息

`xauth`，进入命令行互动模式。

### xhost
> xhost: X-host，管理存取X服务器的权限

`xhost`，显示目前的xhost状态。

`xhost +192.168.0.1`，允许地址192.168.0.1对本机的X服务器有访问权限。

### xset
> xset: X-set，设置X-window的参数

`xset s on`，打开屏幕保护功能。

`xset q`，显示目前状态。

## Y
### yes
> yes: yes，响应相同的字符串

`yes hello`，利用yes命令重复响应字符串hello。

### yum
> yum: yellowdog updater modified，RPM软件包的高级管理

`yum check-update`，列出所有可升级的软件包。

`yum install lynx`，安装lynx软件包。

`yum update webalizer`，升级webalizer软件包。

```
# yum shell
> update vino
> run
> exit
```
使用shell升级vino软件包。

## Z
### zcat
> zcat: zip concatenate，列出压缩文件中的文件内容

`zcat file.zip`，查看压缩文件中的内容，只能列出压缩文件内的第一个文件的内容。

### zgrep
> zgrep: gz grep，查找gz或Z文件中特定的字符串

`zgrep line3 file.Z`，用zgrep命令查找压缩文件中包含line3的字符串。

`zgrep line3 file.gz`，用zgrep命令查找压缩文件中包含line3的字符串。

### zip
> zip: zip，将文件压缩为zip格式

`zip file.Z file*`，将文件名file开头的文件压缩为file.Z。

`zip -d file.Z file1.zip`，承接上例，移除压缩文件file.Z中的file1.zip。

### zipgrep
> zipgrep: zip grep，查找zip文件中特定的字符串

`zipgrep Th file1.zip`，寻找压缩文件file1.zip中包含字符串Th的文件，并列出该行。

### zipinfo
> zipinfo: zip information，显示zip压缩文件的信息

`zipinfo file.Z`，显示文件file.Z的压缩内容。

### zless
> zless: zip less，显示gz或Z文件的内容

`zless file.gz`，查看压缩文件的内容。

### znew
> znew: Z new compression，将Z文件重新压缩为gz文件

`znew file.Z`，将文件file.Z转换为file.gz。若Z文件中包含多个文件，将无法使用这个命令。






