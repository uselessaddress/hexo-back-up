title: 在CentOS上搭建Node环境
toc: true
date: 2016-05-14 18:37:00
tags: 
- centos
- nodejs
- node
- nvm
- 毕设
categories: 设计开发
---
# 前言
毕设进入到了最后阶段，基本功能都完成了，接下来就是一些功能的完善和bug的修改。以及，好长好长的论文要写。。。压力有点大哇！

为了方便在答辩的时候装逼，小编决定把毕设上线到阿里云服务器。

<!--more-->

# 步骤

## 安装node
1、使用xshell登陆到阿里云服务器。
2、`yum -y install gcc make gcc-c++ openssl-devel wget`
3、`wget http://nodejs.org/dist/v6.1.0/node-v6.1.0.tar.gz`
4、`tar -zvxf node-v6.1.0.tar.gz`
5、`cd node-v6.1.0`
6、`./configure`
7、`make && make install`
8、`node -v`

## 安装node方法二
第一种安装node的方法，小编失败了，因为编译出错。那么，试试第二种方法。
1、`yum install epel-release`
2、`yum install nodejs`
3、`yum install npm`
4、`node -v`
安装成功！但是版本有点低，0.12.0，因为我本地使用的5.6.0，版本不同，估计会出问题哇！

## 安装mongodb
1、`wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.0.6.tgz `
2、`tar -zxvf mongodb-linux-x86_64-3.0.6.tgz`
3、`mv  mongodb-linux-x86_64-3.0.6/ /usr/local/mongodb`
4、`export PATH=<mongodb-install-directory>/bin:$PATH`，其中`<mongodb-install-directory>`为`/usr/local/mongodb`。
5、`mkdir -p /data/db`
6、`cd /usr/local/mongodb/bin`，`mongod`，至此，mongodb已经安装成功并且启动
7、使用另一个shell，`cd /usr/local/mongodb/bin`，`mongo`，连接成功，可以进行数据库的操作了。

## 开机自启动mongodb
`vim /etc/rc.d/rc.local`，插入一行如下：
```
/usr/local/mongodb/bin/mongod
```


## 安装git
`yum install git`

## 下载项目
把本地项目上传到github
`git clone https://github.com/voidking/nodeforum.git`

## 安装需要的依赖
1、`npm install`
2、`npm install bower -g`
3、`bower install`
有报错，整了半天没整好，后来无缘无故好了，大写的蛋疼。

## 运行
`node app.js`，报错，主要是connect-mongo版本问题。大哥，出问题才正常好不，毕竟node版本相差太大。还是想办法先把node版本统一了，这才是正途！

## 安装node方法三
1、`wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.31.1/install.sh | bash`
2、`source ~/.bash_profile`
3、`nvm list-remote`
4、`nvm install v5.6.0`
5、`nvm list`
6、`nvm use v5.6.0`
7、`nvm alias default v5.6.0`

## 再次运行
`node app.js`，哈哈，之前的报错没有啦！
然后，换了一个新的报错，无法连接到mongodb。使用`mongo`命令检查一下，原来是服务关掉了。为什么会关掉呢？因为我们之前的mongodb启动方法不对！在shell里启动，肯定不靠谱哇！
好吧，暂时先勉强用着，在shell里再次启动mongodb。`node app.js`，启动成功！
打开http://139.129.28.10:3000 ，看到了熟悉的界面，上线成功！

## mongodb添加进服务

1、`mkdir -p /data/db`，`mkdir -p /data/log`，在`/data/log`下新建文件mongodb.log。
2、在`/usr/local/mongodb`下新建文件mongod.conf，内容如下：
```
dbpath = /data/db #数据文件存放目录
logpath = /data/log/mongodb.log #日志文件存放目录
port = 27017  #端口
fork = true  #以守护程序的方式启用，即在后台运行
```
3、在`/ect/rc.d/init.d`下新建文件mongod，内容如下：
```
#!/bin/sh
#
# mongodb init file for starting up the MongoDB server
#
# chkconfig: - 20 80
# description: Starts and stops the MongDB daemon that handles all \
# database requests.
# Source function library.
. /etc/rc.d/init.d/functions
exec="/usr/local/mongodb/bin/mongod"
prog="mongod"
logfile="/data/log/mongodb.log"
options=" -f /usr/local/mongodb/mongod.conf"
[ -e /etc/sysconfig/$prog ] && . /etc/sysconfig/$prog
lockfile="/var/lock/subsys/mongod"
start() {
[ -x $exec ] || exit 5
echo -n $"Starting $prog: "
daemon --user root "$exec --quiet $options run >> $logfile 2>&1 &"
retval=$?
echo
[ $retval -eq 0 ] && touch $lockfile
return $retval
}
stop() {
echo -n $"Stopping $prog: "
killproc $prog
retval=$?
echo
[ $retval -eq 0 ] && rm -f $lockfile
return $retval
}
restart() {
stop
start
}
reload() {
restart
}
force_reload() {
restart
}
rh_status() {
# run checks to determine if the service is running or use generic status
status $prog
}
rh_status_q() {
rh_status >/dev/null 2>&1
}

case "$1" in
start)
rh_status_q && exit 0
$1
;;
stop)
rh_status_q || exit 0
$1
;;
restart)
$1
;;
reload)
rh_status_q || exit 7
$1
;;
force-reload)
force_reload
;;
status)
rh_status
;;
condrestart|try-restart)
rh_status_q || exit 0
restart
;;
*)
echo $"Usage: $0 {start|stop|status|restart|condrestart|try-restart|reload|force-reload}"
exit 2
esac
exit $?
```

4、`chmod 777 mongod`，给mongod文件增加执行权限。
5、`service mongod start`，mongodb服务成功启动。

## node守护进程
1、安装nohup，`yum provides */nohup`，`yum install coreutils`
2、启动服务，`nohup node app.js &`
3、关闭服务，`bg`，`fg`，Ctrl+C。

后来发现一个问题，`nohup node app.js &`，断开连接后有时会关闭进程，或者输入命令后两次回车也会关闭进程。
查看nohup.out文件，奥，原来是代码出问题了！如果使用命令`nohup node app.js > myout.file 2>&1 &`，就查看myout.file文件。

还有一点需要注意，退出xshell的时候，不要直接关闭，最好使用`exit`命令，否则有时也会导致进程关闭。

# 后记
查看CentOS版本命令：`cat  /etc/redhat-release`
查看端口占用命令：`netstat -apn | grep 27017`
查看进程命令：`ps -ef | grep mongodb`，`ps aux | grep mongodb`，`top`
结束进程命令：`kill -9 [PID]`




# 参考文档
Centos 安装 NodeJS
http://www.cnblogs.com/hamy/p/3632574.html

Node Downloads
https://nodejs.org/en/download/current/

如何在CentOS 7安装Node.js
http://www.linuxidc.com/Linux/2015-02/113554.htm

Linux平台安装MongoDB
http://www.runoob.com/mongodb/mongodb-linux-install.html

CentOS6.5源码安装nodejs4.4
http://www.centoscn.com/image-text/install/2016/0314/6839.html

在CentOS 7上安装Node.js的4种方法
https://www.vmvps.com/4-ways-to-install-node-js-on-centos-7-servers.html

nvm项目
https://github.com/creationix/nvm

MongoDB在CentOS6下的安装以及服务启动
http://www.swmemo.com/2136.html

Linux 守护进程的启动方法
http://www.ruanyifeng.com/blog/2016/02/linux-daemon.html
