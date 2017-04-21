title: Linux命令大全——LMNOPQ
date: 2013-07-16 10:00:00
tags: Linux
categories: 精华转载
toc: true
---

本文摘自《Linux/UNIX指令范例速查手册》。

## L
### last
> last: last login，显示曾登录的账号

`last`，显示曾登录的账号。

`last -x`，显示系统关机参数与运行等级。

### lastb
> lastb: last bad login，显示登录失败的账号

`lastb`，显示登录失败的账号。

`lastb -i`，显示IP地址，而不显示主机名称。

### ldd
> ldd: library dependencies，列出与文件有关的函数库

`ldd /bin/netstat`，显示/bin/netstat所使用的共享函数库。

`ldd /bin/cat`，显示/bin/cat所使用的共享函数库。

### less 
> less: less，显示文件内容

`less ezhttp.log`，分页查看ezhttp.log文件。
使用pgup和pgdn上下翻页，使用q退出。

`ps -ef | less`，ps查看进程信息并通过less分页显示。

### lilo
> lilo: LInux LOader，开机启动程序

`lilo -v -v -v`，设置完/etc/lilo.conf开机配置后，使之开机时生效，指定显示第三级模式。

### ln
> ln: link，新建文件之间的连接

`ln -s yy zz`，将文件yy产生一个符号链接zz。 
　　 
`ln yy xx`，将文件yy产生一个硬链接zz。

### lndir
> lindir: link directory，新建目录之间的连接

`lndir /etc/vsftpd/`，新建目录的连接。

### lnstat
> lnstat: linux network statistics，列出网络数据统计信息。

`lnstat -d`，列出支持的统计文件。

### locate
> locate: locate，在系统中查找包含特定字符串的文件

`locate mysql.sock`，在整个系统中查找mysql.sock的文件。 

`locate -n 100 a.out`，在整个系统中查找a.out文件，但最多只显示100个。

`locate -u`，建立资料库。

系统如果找不到locate命令，需要先安装locate，`yum install mlocate`，然后更新locate的数据库，`updatedb`。

### logname
> logname: login name，列出登录的账号

`logname`，显示最开始登录系统的账号。

### logrotate
> logrotate: log rotate，定期或定量将日志文件压缩备份

`logrotate /etc/logrotate.conf`，执行logrotate命令并采用/etc/logrotate.conf中的设置。

### logsave
> logsave: log save，将制定程序的输出存为日志文件

`logsave ps.txt ps`，将ps的输出记录到文件ps.txt中。

### lp
> lp: line printer，打印文件

`lp file1`，将file1通过默认的打印机输入。

### lpq
> lpq: line printer queue，列出正在等待打印机的队列

`lpq`，列出目前打印机的队列状态。

### lprm
> lprm: line printer remove，删除正在打印的任务

`lprm -`，取消所有的打印任务。

### ls 
> ls: list，列出目录或文件名

`ls -ltr s*`，列出当前工作目录下所有名称是s开头的文件，新的排后面。
　　
`ls -lR /home`，将 /home 目录以下所有目录及文件详细资料列出。
　　 
`ls -AF`，列出目前工作目录下所有文件及目录。目录名称后会加 "/"，可执行文件名称后会加"*"，链接文件后会加"@"。

### lsattr
> lsattr: list attribute，列出ext2或ext3系统中文件的属性

`lsattr`，列出当前文件的类别。

### lsmod
> lsmod: list module，列出内核模块的使用状态

`lsmod`，列出（部分）内核模块在RedHat与Fedora上的使用状态。

### lsusb
> lsusb: list usb，列出所有USB设备。

`lsusb`，列出目前的USB设备。

### lynx
> lynx: 由大学实验室中命名而来，文字界面上显示网页内容

`lynx www.google.com`，通过lynx命令在终端机上浏览网页。

## M
### mail
> mail: mail，收发邮件

`mail -s "test mail" voidking@qq.com`，信件主题为“test mail”，然后输入右键内容。信件结束时，输入一个点并按enter键。然后输入发件人的email地址，没有就按enter键。 

如果没有mail命令，需要先安装mailx，`yum install mailx`。

### mailstats
> mailstats: mail status，显示目前的邮件状态

`mailstats`，列出目前的邮件统计表。

### mailq
> mailq: mail queque，列出队列中的邮件

`mailq`，列出所有在队列中尚未寄出邮件。

### make
> make: make gcc program，维护或编译程序组

`make -C /etc/mail`，在RedHad下编译sendmail的配置文件。

```
cd /usr/src/linux
make
make modules_install
make install
```
运行编译内核的编译顺序。

### makemap
> makemap: make map files，产生sendmail的数据库文件

`makemap -l`，列出支持的转换文件类型。

`makemap hash /etc/mail/access.db < /etc/mail/access`，通过/etc/mail/access产生/etc/mail/access.db转换文件。

### man
> man: manual，显示在线帮助信息

`man kill`，显示kill命令说明。

`man -K kill`，显示所有与kill有关的说明。

### manpath
> manpath: manual path，显示在线帮助的搜索路径

`manpath`，显示在线帮助的搜索路径。

### md5sum
> md5sum: MD5 check sum，计算并显示MD5 sum。

`md5sum file1`，检验文件file1的MD5 sum。

`cat checktxt`，`md5sum -c check.txt`，检验check.txt中所记载的MD5 sum是否正确。

### mesg
> mesg: message，控制终端机的写入权限

`mesg`，查看其他人对当前终端机的写入权限。

`mesg n`，关闭其他人对当前终端机的写入权限。

### mkbootdisk
> mkbootdisk: make boot disk，制作启动盘

`mkbootdisk --device /dev/fd0 --verbose 2.6.33`，使用2.6.33的内核制作启动盘。

### mkdir
> mkdir: make directory，新建目录

`mkdir temp`，在当前目录下新建temp子目录。

`mkdir -p /opt/www/test`，新建所有不存在的目录和上层目录。

### mke2fs
> mke2fs: make ext2/ext3 file system，格式化为ext2、ext3或ext4的文件系统

`mke2fs /dev/sda3`，将分区格式化为ext2的文件系统。

`mke2fs -j /dev/sda3`，将分区格式化为ext3的文件系统。

`mke2fs -t ext4 /dev/sda3`，将分区格式化为ext4的文件系统。

### mkfs
> mkfs: make file system，格式化文件系统

`mkfs /dev/sda1`，将分区/dev/sda1格式化为默认的ext2文件系统。

### mkfs.xfs
> mkfs.xfs: make XFS file system，格式化为xfs的文件系统

`mkfs.xfs /dev/sdc3`，将分区格式化为xfs的文件系统。

### mkinitrd
> mkinitrd: make initial ramdisk images，建立ramdisk的镜像文件

`uname -a`，创建一个镜像文件。

`mkinitrd /boot/initrd-new.img 2.6.33-85.fc13.i686.PAE`，创建一个镜像文件。

### mkreiserfs
> mkreiserfs: make reiser file system，格式化为reiserfs的文件系统

`mkreiserfs /dev/sda1`，将分区/dev/sda1格式化为reiserfs的文件系统。

### mkswap
> mkswap: make swap，新建swap空间

`mkswap /dev/sda2`，新建一个swap空间。

### modinfo
> modinfo: module information，显示内核模块的信息

`modinfo mii`，显示mii模块的信息。

`modinfo -a snd`，只显示snd模块的作者信息。

### modprobe
> modeprobe: module probe，从内核中新建或删除模块

`modprobe -l ext*`，显示名称以ext开头的模块名称。

`modprobe --show-depends ext2`，显示与ext2有关的模块名称。

### more 
> more: more，显示文件内容

`more -s testfile`，逐页显示 testfile 的文件内容，如有连续两行以上空白行则以一行空白行显示。

`more +20 testfile`，从第 20 行开始显示 testfile 之文件内容。

### mount
> mount: mount，挂载文件系统

`mount`，显示当前的分区状态。

`mount -t xfs /dev/sda2 /opt`，将分区/dev/sda2挂载到/opt上，并指定文件系统为xfs。

`mount -t ext3 server1://data /opt`，挂载NFS服务器所共享的文件系统。

`mount -t smbfs -o username=tom,password=123 //10.1.1.1/TL /tmp`，挂载windows系统的网上邻居中所共享的文件系统。

### mtools
> mtools: MSDOS tools，显示mtools所支持的命令

`mtools`，显示所有支持MSDOS文件系统的命令。

### mutt
> mutt: mail user agent，文字界面的邮件工具

`mutt -s "A test mail" josfeng@gmail.com`，将邮件寄给 josfeng@gmail.com，信件主题为A test mail。

### mv
> mv: move，移动或重命名文件或目录
 
`mv aaa bbb`，将文件 aaa 更名为 bbb。
　　 
`mv *.c finished`，将所有的.c文件移动到 finished 目录中。

## N
### nano
> nano: Nano's another editor，文本编辑

### ncftp
> ncftp: new command line FTP，传送与接收文件

```
# ncftp -u max -p abc123 172.20.11.1
> get readme
> bye
> yes
```
使用ncftp命令下载一个readme文件。

### netstat
> netstat: net status，查询网络目前的状态

`netstat -nt`，显示目前TCP的连接状态。

`netstat -apt`，显示目前TCP应用进程所使用的端口号。

### nice
> nice: nice，更改优先级

```
nice
nice -n 1 /bin/bash
nice
```
调整shell的优先级。

### nohup
> nohup: no hup，后台运行指定的程序

`nohup script1 &`，在后台运行script1，且在脱机后仍可继续运行。

### nslookup
> nslookup: name server lookup，域名与IP地址的对应

```
# nslookup
> www.163.com
> exit
```
查询www.163.com的网站地址。

```
# nslookup
> set type=mx
> qq.com
```
查询qq.com邮件服务器的地址。

## O
### od
> od: octal dump，以八进制编码输出文件内容

`od file`，以八进制编码输出文件内容。

`od -t c file`，以ASCCII码显示文件file的内容。

## P
### passwd 
> passwd: password，修改密码

`passwd`，一般账号修改密码。

`passwd mark`，修改账号mark的密码。

`passwd -l peter`，将peter账号停用。

`passwd -u peter`，将peter账号启用。

### paste
> paste: paste，合并文件的内容

`paste file1 file2`，将两个文件按列合并。

### patch
> patch: patch，补丁更新

`patch file file.patch`，以补丁文件file.patch修补文件file。

`patch b file file.patch`，以补丁文件file.patch修补文件file，并备份原文件。

### pg
> pg: pagewise，显示文件内容

`pg aaa`，使用pg显示aaa这个文件。

### pgrep
> pgrep: process grep，根据PID显示进程

`pgrep gdm`，列出与字符串gdm有关的PID。

### pico
> pico: pine composer，文本编辑

### pidof
> pidof: process ID of something，查找进程的PID

`pidof nfs`，显示进程nfs所用的PID。

### pine
> pine: 作者命名，文字界面的邮件工具

新版Linux中，pine已被alpine所取代。

### ping
> ping: 乒乓碰撞声，用特定的数据包测试主机是否在线

`ping -c 5 www.sina.com.cn`，发送5次ICMP echo数据包，并显示统计结果。

`ping -s 120 192.168.1.1`，使用大小为120Byte的数据包进行测试。

`ping -r www.sina.com.cn`，不通过网关，直接传送数据包。

### pkill
> pkill: process kill，传送信号给指定的进程

`pkill -9 sendmail`，将正在运行且含有sendmail的进程终止。

### pmap
> pmap: process map，显示进程的内存对应

`pmap 2245`，显示进程2245的运行状态。

### postalias
> postalias: postfix aliases，产生postfix的aliases数据库文件

### postmap
> postmap: postfix map，产生postfix的access数据库文件

### postqueue
> postqueue: postfix queue，postfix队列区的控制命令

`mailq`，显示在mailq队列中的邮件。

`postqueue -f`，强制传送队列中的邮件。

### postsuper
> postsuper: postfix super，postfix邮件队列的高级管理

`mailq`，`postsuper -d B175`，删除邮件B175。

`postsuper -d ALL`，删除所有在队列中的E-mail。

### pr
> pr: print，打印前的重新排版

### ps
> ps: process，显示目前的进程

`ps`，显示当前账号所运行的进程。

`ps -ef`，完成地列出所有账号的进程。

`ps aux`，列出所有账号的进程，以及该进程所有的CUP和内存比例。

### pstree
> pstree: process tree，以树状表示目前的进程

`pstree`，以树状表示目前的进程运行状况。

### pwck
> pwck: passwod check，检查密码文件的正确性

### pwconv
> pwconv: password convert，转换为投影密码

### pwd
> pwd: print the working directory，显示当前所在的目录

`pwd`，显示当前所在的目录。

### pwunconv
> pwunconv: password convert，还原投影密码

## Q
### quota
> quota: quota，显示并限制账号的硬盘用量

`quota`，显示自己的硬盘用量。

`quota mark`，显示账号mark的硬盘用量。

### quotacheck
> quotacheck: quota check，检查账号硬盘空间的限制

### quotaoff
> quotaoff: quota off，关闭账号硬盘空间的限制

### quotaon
> quotaon: quota on，开启账号硬盘空间的限制

### quotastats
> quotastats: quota status，显示账号硬盘空间限制的统计数据










