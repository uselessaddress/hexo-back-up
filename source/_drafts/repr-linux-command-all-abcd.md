title: Linux命令大全——ABCD
date: 2013-07-16 08:00:00
tags: Linux
categories: 精华转载
toc: true
---

本文摘自《Linux/UNIX指令范例速查手册》。
## A
### adduser
> adduser: add user，新建系统上的账号

`adduser -D`，显示新建账号时的默认值。

`adduser -m jos`，新建名为jos的账号（使用系统默认值）。

adduser位于/usr/sbin/之下，是/usr/sbin/useradd的连接。也就是说，adduser和useradd实际上是同一个命令。

<!--more-->

### alias
> alias: alias，定义命令及参数的别名

`alias`，列出现有的别名设置。

`alias ua='uname -a'`，将uname -a的别名设置为ua。

alias的优先级高于path（系统搜寻的路径）。

### apachectl
> apachectl: apache controller，管理Apache网页服务器

`apachectl -l`，列出编入apache的模块。

`apachectl restart`，重启apache。

### apt-get
> apt-get: advanced package tool get，APT软件包管理工具。

`apt-get install mailx`，安装mailx软件包。

apt-get是Linux发行商Debian与Ubuntu上的软件包管理工具，其他版本Linux无法使用。

### ar
> ar: archives，打包和解压缩文件

`ar -rv afile a*`，将以a开头的文件打包为afile文件。

`ar -t afile`，列出打包文件中的成员文件。

`ar -p afile anaconda-ks.cfg`，显示打包文件中某一文件的内容。

ar命令已被tar所取代，目前已很少使用。

### arch
> arch: architecture，列出处理器的类型

`arch`，列出处理器的类型。

### arp
> arp: address resolution protocol，网卡地址的对应

`arp`，列出arp的信息。

`arp -s 10.1.1.10 00:0F:26:2A:BF:77`，将10.1.1.10强制对应到网卡号00:0F:26:2A:BF:77。

`arp -d 10.1.1.10`，删除IP地址与网卡号的对应。

### arping
> arping: ARP ping，网卡地址的测试命令

`arping 172.20.11.1`，对172.20.11.1的IP地址进行网卡地址测试。

若不在同一个网络，arping不会有回应，这时需要用ping命令。

### at 
> at: at，在指定的时间运行命令

`at 5pm + 3 days /bin/ls`，三天后的下午 5 点执行 `/bin/ls`。
　　 
`at 5pm + 3 weeks /bin/ls`，三个星期后的下午 5 点执行 `/bin/ls`。
　　 
`at 17:20 tomorrow /bin/date`，明天的 17:20 执行 `/bin/date`。
　　
`at 23:59 12/31/1999 echo the end of world !`，在1999年的最后一天的最后一分钟印出 the end of world !  

`at -l`，列出将要运行的工作。

`at -c 1`，显示工作编号为1的工作。

`at -d 1`，删除编号为1的工作。

### awk
> awk: Alfred Aho, Peter Weinberger, and Brian Kernighan(作者名)，文字数据的高级处理。

`awk '{print}' /etc/passwd`，显示/etc/passwd中内容，和cat命令结果相同。

`awk -F":" '{print $1 $3 $6}' /etc/passwd`，将/etc/passwd中的内容以冒号分隔，并取出第1位、第3位和第6位。

`awk -F":" '{print $1 "\t" $3 "\t" $6}' /etc/passwd`，将/etc/passwd中的内容以冒号分隔，并取出第1位、第3位和第6位，并用Tab作为字段间的分隔符。

`awk -F":" '{print "ID=" $1 "\t 家目录=" $6}' /etc/passwd`，将/etc/passwd中的内容以冒号分隔，并取出第1位和第6位，并用Tab作为字段间的分隔符，在第1位前加上“ID=”，第6位前加上“家目录=”。

------

## B
### badblocks
> badblocks: bad blocks，检查硬盘中损坏的区块

`badblocks -v /dev/sda1`，检查损坏的区块，并显示详细信息。

适用于ext2和ext3文件系统。

### batch
> batch: batch，运行批次作业

`batch -f com.txt`，运行文件com.txt中的命令。

### bc
> bc: arbitrary precision calculator，文字型计算器

`bc`，进入计算器。可以做四则运算，也可以定义变量并做运算。

### bg
> bg: background，将进程放到后台运行

`cat /var/log/messages | more`，然后ctrl+z暂时中断程序。再运行`bg 1`，其中1为工作编号。

将正在运行的进程移到后台运行，其效果与运行命令后面加上&效果相同。

### bind
> bind: bind，显示或设置键盘配置

`bind -l | grep kill`，列出与kill有关的所有功能名称。

`bind -m vi -v`，列出vi的按键配置与使用的变量名称。

### blockdev
> blockdev: block device，查询区块设备

`blockdev -v --getss /dev/sda1`，列出/dev/sda1的区块大小。

`blockdev -v --getsize /dev/sda1`，获取/dev/sda1的区块容量。

### bunzip2
> bunzip2: Burrows-Wheeler un-zip file，解压缩bz2格式的压缩文件。

`bunzip2 -k afile.bz2`，解压afile.bz2文件，不删除原来的压缩文件。

`bunzip2 -s afile.bz2`，用较少的内存解压afile.bz2文件。

bunzip2是bzip -d的功能连接。

### bzgrep
> bzgrep: Burrows-Wheeler zip file grep，查找bz2文件中特定的字符串

`bzgrep router ip.txt.bz2`，寻找ip.txt.bz2压缩文件中的router字符串。

### bzip2
> bzip2: Burrows-Wheeler zip file，将文件压缩为bz2文件

`bzip2 afile`，压缩文字文件afile为afile.bz2，压缩后afile文件消失。

`bzip2 -l pic.png`，压缩一般的png图像文件。

`bzip2 -d pic.png.bz2`，解压文件。

### bzip2recover
> bzip2recover: Burrows-Wheeler zip file recover，修复损坏的bz2文件

`bzip2recover text.bz2`，当bz2文件发生问题无法解压缩时，尝试此命令来还原文件。

### bzless
> bzless: Burrows-Wheeler zip file less，列出bz2文件的内容

`bzless afile.bz2`，列出压缩文件afile.bz2中的内容。

------

## C
### cal 
> cal: calendar，显示日历

`cal`，显示本月的月历。

`cal 2000`，显示2000年年历。

`cal 5 2001`，显示2000年5月月历。

`cal -m`，以星期一为每周的第一天方式，显示本月的月历。 

`cal -jy`，以一月一日起的天数显示今年的年历。

### cat 
> cat: catenate，列出文件内容

`cat -n textfile1 > textfile2`，把textfile1的内容加上行号后，转存为textfile2。

`cat -b textfile1 textfile2 >> textfile3`，把textfile1和textfile2的内容加上行号（空白行不加）之后，将内容附加到textfile3的最后。

### cd 
> cd: change directory，切换目录

`cd /usr/bin`，进入`/usr/bin/`目录。

`cd ~`，回到home directory。

`cd ../..`，跳到目前目录的上上两层:

### cfdisk
> cfdisk: curses formatted disk，设置硬盘分区

`cfdisk`，进入分区界面。

`cfdisk -P S /dev/sda`，按照扇区排序，显示第一块硬盘的分割情况。

cfdisk是传统命令fdisk的进化版。

### chage
> change: change user password expiry info，改变密码的有效期

`cat /etc/shadow | grep sherry`，`chage -E 2018-12-31 sherry`，设置sherry账号的密码设置在2018年12月31日失效。

`chage -M 5 sherry`，要求账号sherry必须在5天内变更密码。

`chage -l sherry`，显示账号的密码设置。

### chattr
> chattr: change attributes，改变文件属性

`chattr +a file1`，`lsattr file1`，增加文件的属性，使之可以附加数据，而无法被修改。

`chattr +i file1`，改变文件属性，无法修改和删除。

### chcon
> chcon: change security context，修改SELinux标签

`chcon -R -t httpd_sys_content_t www/`，将www目录类型改为httpd_sys_content_t。

### chgrp
> chgrp: change group，改变文件或目录所属的组

`chgrp users afile`，修改afile的组为users。

`chgrp -h users tt`，修改符号连接tt的组为users。

可以使用chmod实现同样的效果，因此chgrp使用频率较低。

### chkconfig
> chkconfig: check configurate，设置系统在不同运行等级下的服务。

`chkconfig --list sendmail`，列出sendmail在不同运行等级下的状态。

`chkconfig --level 35 named on`，使DNS服务器在运行等级为3和5时启动。

`chkconfig --level 0123456 vsftpd on`，使FTP服务器在所有等级下启动。

`chkconfig --lis | grep 3:启用`，列出runlevel3中所有开启的服务。

### chmod
> chmod: change mode，改变文件或目录的权限

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
> chown: change owner，改变文件或目录的拥有者或组

`chown jessie:users file1.txt `，将文件file1.txt的拥有者设为users群体的用户jessie。
　　
`chmod -R lamport:users *`，将当前目录下的所有文件与子目录的拥有者皆设为users群体的用户lamport。

### chroot
> chroot: change root，切换根目录所在的路径

`chroot /mnt/disk /bin/bash`，将根目录切换到/mnt/disk，并将/bin/bash作为使用的shell。

### chsh
> chsh: change shell，改变账号登录系统时所使用的shell

`chsh -l`，列出所有可用的shell。

`chsh`，然后指定使用的shell。

`chsh -s /bin/bash peter`，指定peter账号的shell。

### clear 
> clear: clear，清除画面

`clear`，清屏。 

### clock
> clock: clock，调整RTC（Real Time Clock）时间

`clock`，显示目前硬件时钟的时间。

`clock --set --data="2/27/11 22:15"`，将目前硬件时钟的时间设置为2011年2月27日22:15。

`clock --hctosys`，让系统时间和硬件时钟一致。

`clock --systohc`，将系统时间写入硬件时钟。

### cmp
> cmp: compare，对比两个文件的差异

`cmp test.txt text.txt`，对比两个文件。

一般使用diff命令来进行文本内容比较，cmp使用较少。

### col
> col: column，过滤特殊字符

`col -f < testfile`，过滤testfile中的RLF字符。

`man kill | col -b > kill.txt`，过滤所有控制字符（RLF和HRLF）。

### colrm
> colrm: column remove，删除指定的列

`cat file | colrm 7`，删除第6列以后的字符。

`cat file | colrm 2 5`，删除第2~5列的字符。

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

### cp 
> cp: copy file，复制文件或目录

`cp aaa bbb`，将文件aaa复制命名为 bbb。

`cp *.c finished`，将所有的.c文件复制到finished目录中。


### cpio
> cpio: copy in, copy out，文件备份

`ls | cpio -o -O ./backupfile`，将目录下的所有文件（不包含子目录）备份到backupfile。

`cpio -t -v -I backupfile`，查看备份文件backupfile中的文件信息。

### crontab
> crontab: cron table，设置计划任务

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

### csplit
> csplit: content split，分割文件

`csplit -n 3 vsftpd.log 3000`，以3000行为界分割为两个文件，并指定列出的文件名位数为3。

`csplit -f file vsftpd.log 3000`，以3000行为界分割为两个文件，且指定分割的文件名以file开头。

`csplit vsftpdlog 1000 {7}`，以1000行为界分割为7个文件。

### ctrlaltdel
> ctrlaltdel: control alt del，设置Ctrl+Alt+Del快捷键。

`ctrlaltdel hard`，设置为不保存数据立即重启。
`ctrlaltdel soft`，设置为保存数据、停止服务、卸载文件后重启。

### cut
> cut: cut，截取文本内容的指定范围

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

------

## D
### date
> date: date，显示或修改日期时间

`date`，显示当前日期和时间。

`date +%B%d`，显示月份与日数。

### dd
> dd: standard input, standard output，转换并列出数据

`dd if=file.txt of=/dev/fd0`，将文件file.txt写入到软盘。

`dd if=boot.img of=/dev/fd0 bs=1440k`，制作启动盘，其中，boot.img为开机的镜像文件。

`dd if=test.txt of=out.txt conv=ucase`，将文件test.txt中的英文字母全部转换为大写后，存储为out.txt。

### debugfs
> debugfs: debug file system，ext2和ext3的文件系统改错工具

`debugfs /dev/sda7`，`dump install.log /root/bkp.txt`，将/dev/sda7下的install.log文件导出一份放到/root/bkp.txt中。

### declare
> declare: declare，声明环境变量

`declare`，显示当前的shell变量。

`declare -x`，显示所有的环境变量。

`declare -i number=100+200`，`echo $number`，如果不加-i，系统会以字符串方式来处理100+200。

declare命令与export命令相比，区别在于declare声明的是shell变量，export声明的是环境变量。shell变量只能给shell只用，环境变量可以给shell以及外部命令使用。declare加上-x参数，则与export的作用相同。

### depmod
> depmod: dependence of module，分析可加载模块的关联性

`depmod -a`，检测模块的关联性。

### df
> df: display file system，显示文件系统的使用情况

`df`，显示当前文件系统的使用状况。

`df -m`，以MB为单位来显示当前文件系统的使用状况。

`df -a`，显示所有文件系统的使用状况。

`df -h`，以较易读取的方式显示文件系统的使用状况。

`df -i`，显示系统inode的状态。

### diff
> diff: diffrence，比较并显示文件差异

`diff file1 file2`，对比file1和file2。

`diff -c file1 file2`，对比file1和file2，并列出文件的异同。

`diff -y file1 file2`，对比file1和file2，并以并列的方式显示对比结果。

`diff -B file1 file2`，对比file1和file2，不对比空白行。

`diff /etc/mail/ mail/`，比较两个目录的差异。

### diffstat
> diffstat: diffrence statistics，根据diff的比较结果显示统计数字

`diff /etc/mail/ mail/ | diffstat`，对比两个目录的差异，并通过diffstat命令列出。

### dig
> dig: dig，显示域名的高级信息

`dig sina.com`，查询域名sina.com。

`dig 163.com -t MX`，查询163.com的邮件名称记录（MX record）。

### dir
> dir: directory，列出目录或文件名

`dir`，列出当前目录的文件。

`dir -l`，以长列表列出当前的文件。

dir命令和ls命令的功能完全相同。

### dirname
> dirname: directory name，列出当前路径下的路径名称

`dirname /opt/httpd`，显示/opt/httpd下的路径名称。

`dirname file.txt`，显示file.txt文件的路径名称。

### dpkg
> dpkg: Debian package，Debian软件包管理工具

`dpkg -L postfix`，列出postfix安装的文件。

`dpkg -i ./unzip_6.0-1_i386.deb`，安装当前路径下的unzip_6.0-1_i386.deb。

dpkg是Debian和Ubuntu上的软件包安装指令，类似于RedHat与Fedora上的rpm，但一般较常使用apt-get。

### du
> du: display units，显示目录或文件的大小

`du`，显示当前目录的使用情况。

`du -sk /var/*`，显示/var目录下所有文件的容量，仅显示总和，默认以KB为单位。

`du -sh /*`，以可读性高的方式显示根目录下的目录容量。

`du --max-depth=2 /var`，显示/var目录下两层子目录所占用的空间。

`du -b backupfile`，显示文件占用的空间。

### dump
> dump: dump，文件系统的备份

`dump -0 -f /opt/backup /boot`，将/boot下的数据备份到/opt/backup中，并更新/etc/dumpdates中的记录。

`cat /etc/dumpdates`，查看更新后的记录。

`restore -r -f /opt/backup`，还原backup到备份的位置。

dump命令常用来备份ext2和ext3文件系统。

restore命令是dump命令的逆命令。