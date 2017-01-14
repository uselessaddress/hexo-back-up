title: Ctrl+Z
date: 2013-10-18 17:02:39
tags: 
- Linux
- 命令
categories: 精华转载
---

Linux操作系统下，命令运行时使用CTRL+Z，强制当前进程转为后台，并使之停止。  

1.使进程恢复运行(后台)    
(1)使用命令bg     
Example：     
zuii@zuii-desktop:~/unp/tcpcliserv$ ./tcpserv01    
*这里使用CTRL+Z,此时serv01是停止状态*    
[1]+ Stopped ./tcpserv01     
zuii@zuii-desktop:~/unp/tcpcliserv$ bg    
[1]+ ./tcpserv01 & 
*此时serv01运行在后台*    
zuii@zuii-desktop:~/unp/tcpcliserv$      

(2)如果用CTRL+Z停止了几个程序呢？    
Example：     
zuii@zuii-desktop:~/unp/tcpcliserv$ jobs    
[1]- Running ./tcpserv01 &    
[2]+ Stopped ./tcpcli01 127.0.0.1     
zuii@zuii-desktop:~/unp/tcpcliserv$ bg %1    
bash: bg：任务1已转入后台*后台运行*    
<!--more-->
2.使进程恢复至前台运行    
Example：     
zuii@zuii-desktop:~/unp/tcpcliserv$ ./tcpserv04    
[1]+ Stopped ./tcpserv04   
zuii@zuii-desktop:~/unp/tcpcliserv$ fg     
./tcpserv04   
  
总结：     
(1) CTRL+Z停止进程并放入后台     
(2) jobs显示当前暂停的进程     
(3) bg %N使第N个任务在后台运行(%前有空格)     
(4) fg %N使第N个任务在前台运行     
默认bg,fg不带%N时表示对最后一个进程操作