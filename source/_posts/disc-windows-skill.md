title: windows的一些技巧
date: 2014-06-07 15:19:07
tags: 
- windows
- 问题
- 优化
- 加密
- 密码
categories: 点滴发现
---
1、realtek高清晰音频管理器，关闭图标
开始>运行>输入msconfig>启动>取消勾选"realtek高清晰音频管理器"的开机自启动项>OK>重启生效

2、菜单栏消失
控制面板-外观和个性化-文件夹选项-查看-始终显示菜单

3、删除用户
开始-控制面板-性能和维护-管理工具-计算机管理-本地用户和组-用户删掉你不想要的用户。

4、切换管理员账户
计算机右击-管理-本地用户和组-用户-administrator-帐户已禁用去掉勾-关机旁的切换用户

<!--more-->

5、设置自动关机
进入命令提示符界面，输入“at 18:30 shutdown -s”，表示设置在18:30自动关机。
输入“at”可以查看当前任务。取消自动关机，“at /del”或者“at 1 /del”。

6、修改盘符
计算机右击，管理，磁盘管理，选中需要修改的卷，右击，更改驱动器号和路径。

7、去除用户账户控制
控制面板，用户账户（大图标），更改用户账户控制设置，将按钮调到最低（从不通知）。

8、隐藏NVIDIA 控制面板
双击（或者右击）打开“NVIDIA控制面板”，（不行的话就在控制面板里面打开）然后点“桌面”，去掉那些勾。有个右下角的，还有个桌面的，去掉就可以了。

9、移动桌面
C盘，Users（或者用户），当前用户文件夹（一般是Administrator），找到桌面文件夹。右键，属性，位置，把“C:\Users\Administrator\Desktop”改为你想存放的位置，比如“D:\桌面”，然后点移动就OK。

10、能上QQ不能打开网页
DNS服务器配置错误。

11、win7修改文件名不能使用搜狗输入法
修改完hosts文件后，出现这个问题。
解决办法：重启。

12、ISO镜像制作
使用UltraISO

13、pagefile.sys转移
右击计算机，属性，高级系统设置，高级，性能设置。性能选项窗口中，高级，虚拟内存更改。

14、hiberfil.sys删除
命令提示符下：powercfg -h off

15、右击文件夹出现Runtime Error
winmount这款软件的问题，卸载了就OK了，或者设置去掉右键菜单。迷你版的mini winmount，官方介绍说功能跟标准版一样。

16、在cmd里结束进程
tasklist
taskkill /f /t /im firefox.exe

17、网络管理命令
ping/?
nslookup

18、使用SYSTEM超级管理员账户（最高权限）
taskkill /f /im explorer.exe
at 20:00 /interactive %systemroot%\explorer.exe

19、批量修改文件名
dir /b>rename.xls
使用wps打开rename.xls，在B1单元格输入1.jpg，其余行自动生成。
在C1单元格输入公式="ren "&A1&" "&B1，其余行自动生成。
复制C列，粘贴到记事本，重命名为.bat文件。
双击.bat文件。

20、利用系统加密文件夹(适用XP)
md f:\my..\
start f:\my..\

21、win7库的使用
计算机，库，音乐库，点击蓝色的n个位置。
计算机，库，右击音乐库，属性。

22、豆丁文档免费下载。
百度快照或者豆丁文档下载器

23、捉弄人的高招
截屏，设置为桌面

24、如何在ppt中批量插入图片
插入，图片，分页插入图片

25、校园视频网破解
云窗(校园视频网)破解版，安徽师范大学2010级计算机王宇辉开发。
http://wangyuhui.com.cn

26、文件合成
`copy /b test.jpg + test.rar finish.jpg`









