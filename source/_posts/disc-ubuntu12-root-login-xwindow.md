title: Ubuntu12.10设置root用户登录图形界面
date: 2013-09-11 20:51:27
tags: 
- Linux
- Ubuntu
categories: 点滴发现
---

1、先设定一个root的密码
`sudo passwd root`

2、root 登陆
`su root`

3、备份一下lightgdm
`cp -p /etc/lightdm/lightdm.conf /etc/lightdm/lightdm.conf.bak`

4、编辑lightdm.conf
`sudo gedit /etc/lightdm/lightdm.conf`
<!--more-->
5、加：
```
greeter-show-manual-login=true
```
修改后为：
```
[SeatDefaults]
greeter-session=unity-greeter
user-session=ubuntu
greeter-show-manual-login=true
```

重启登陆即可。已经可以输入root了。

注意：如果root登陆后还没声音，又查了查，如下方法:
Ubuntu root登录没有声音这个问题的根本原因是使用root登录后pulseaudio没有启动。
将root加到pulse-access组：
`sudo usermod -a -G pulse-access root`
然后修改配置文件/etc/default/pulseaudio，将PULSEAUDIO_SYSTEM_START设为1。