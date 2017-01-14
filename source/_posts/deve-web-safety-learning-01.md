---
title: Web安全学习（一）
toc: true
date: 2016-09-10 12:55:30
tags:
- 安全
- 黑客
- 乱码
- yum源
- CentOS7 
categories: 设计开发
---
# 前言
最近在研读《Web安全深度剖析》，其中的一些实践很有意思，做下记录。

<!--more-->

# 非常规HTTP请求
## 发起HTTP请求
借助浏览器可以快速发起一次HTTP请求，如果不借助浏览器，该怎样发起HTTP请求呢？可以借助一些工具，比如curl，这个工具在Linux下是集成的，在Windows下，需要安装。下载地址：https://curl.haxx.se/download.html 。

下载成功后，我们发起一个HTTP请求：`curl www.baidu.com`，然而，返回的却是乱码。
![乱码](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/web-safety-learning/乱码.jpg)
是Windows的问题？换成Linux，依然是乱码。后来查到，从百度获取到的文件，实际上是经过压缩的，需要解压缩，换成命令：`curl www.baidu.com | gunzip`，乱码问题解决，但是中文依然是乱码。
![中文乱码](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/web-safety-learning/中文乱码.jpg)

## CentOS7解决中文乱码
1、查看当前系统语言：`echo $LANG`
2、查看安装的语言包：`locale`
3、下载安装中文语言包：`yum groupinstall chinese-support`
4、`cp /etc/locale.conf /etc/locale.conf_bak`
5、`vi /etc/locale.conf` 
修改内容如下：
```
LANG="zh_CN.GB18030"
LANGUAGE="zh_CN.GB18030:zh_CN.GB2312:zh_CN"
SUPPORTED="zh_CN.UTF-8:zh_CN:zh:en_US.UTF-8:en_US:en"
SYSFONT="lat0-sun16"
```
以上是理想情况，然而问题并没有解决。

## 换源
在下载中文安装包时，报错`group chinese-support does not exist`。我的第一个想法是，换源！下面是换源的步骤：
1、备份源：`cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo_bak`
2、转到源目录：`cd /etc/yum.repos.d/`
3、下载源：`wget http://mirrors.163.com/.help/CentOS7-Base-163.repo`
4、生成缓存：`yum clean all`，`yum makecache`
5、更新系统：`yum -y update`

PS：安装vim：`yum -y install vim*`

然而，换源之后，错误依旧，备受打击。

## 手动安装
无奈手动安装，需要的安装包去这个网站找： http://rpmfind.net/linux/RPM/index.html ，找到后使用wget命令下载。
![安装包](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/web-safety-learning/安装包.jpg)
其中，红色的就是用到的安装包，这里提供一个下载：
链接：http://pan.baidu.com/s/1o8LHKmA 密码：md3i

安装命令：
```
rpm -ivh xorg-x11-xfs-1.0.2-5.el5_6.1.x86_64.rpm chkfontpath-1.10.1-1.1.x86_64.rpm libFS-1.0.0-3.1.x86_64.rpm

rpm -ivh fonts-chinese-3.02-12.el5.noarch.rpm fonts-ISO8859-2-75dpi-1.0-17.1.noarch.rpm
```
过程中可能还会缺少其他包，yum install即可。

安装后修改/etc/locale.conf，中文依旧。但是，在命令行中输入`locale`，可以看到，确实安装好了中文。莫非locale.conf配置不对？`LANG="zh_CN.GB18030"`怎么看怎么不顺眼，于是改成`LANG="zh_CN.UTF-8"`。

重新登录，依旧乱码，这是要闹哪样？不甘心失败，用xshell测试了一下，成功显示中文！
![乱码2](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/web-safety-learning/乱码2.jpg)
![正常](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/web-safety-learning/正常.jpg)


## 模拟HTTP请求
1、win+R，输入`appwiz.cpl`，回车，打开或关闭Windows功能，勾选Telnet客户端，确定。
2、打开CMD运行框，输入`telnet www.baidu.com 80`，然后利用快捷键“ctrl+]”来打开telnet回显。
3、按回车键，进入编辑状态。
4、输入`GET /index.html HTTP/1.1`，按回车键，接着输入`HOST:www.baidu.com`，再连续两次按回车键。
![telnetbaidu](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/web-safety-learning/telnetbaidu.jpg)
![telnetbaidu2](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/web-safety-learning/telnetbaidu2.jpg)
没错，Windows下中文依然是乱码，在CentOS7下试试：
![telnetbaidu3](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/web-safety-learning/telnetbaidu3.jpg)

# 截取HTTP请求
## Burp Suite Proxy初体验
Burp Suite是用于Web应用安全测试工具的集成平台，下载地址：https://portswigger.net/burp/download.html

1、配置网络代理。
双击BurpLoader.jar，选择“Proxy”选项卡，然后选择“Options”选项卡，点击Add按钮，在“Bind to port”框中输入端口号6666，“Bind to address”选择“Loopback only”，然后单击“OK”按钮。
![proxylistener](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/web-safety-learning/proxylistener.jpg)

打开火狐浏览器，选项，高级，网络，设置，手动配置代理。在HTTP代理框中输入127.0.0.1，端口号为6666，其他不用配置，单击“确定”。
![firefoxset](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/web-safety-learning/firefoxset.jpg)

2、查看拦截信息。
在火狐浏览器中，输入`www.baidu.com`，回车后发现服务器很久没有回应信息，原因是Burp把HTTP请求拦截了，此时浏览器会处于阻塞状态。
查看Burp的Intercept选项卡，可以看到请求信息。
![请求信息](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/web-safety-learning/请求信息.jpg)

3、绕过前端验证。
启动wamp服务器，在火狐浏览器访问我们自己写的登录页面。发现请求页面并没有被拦截，修改火狐代理配置。
![firefoxset2](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/web-safety-learning/firefoxset2.jpg)
再次访问登录页面，页面被拦截，在Burp中点击“Forword”，跳转到下一步。
当我们输入的用户名或密码为空时，前端会出现提示。
![用户名为空](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/web-safety-learning/用户名为空.jpg)
当我们正常输入用户名和密码时，在Burp中拦截信息。
![拦截信息](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/web-safety-learning/拦截信息.jpg)
然后修改信息为：
![修改信息](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/web-safety-learning/修改信息.jpg)
点击“Forward”，前端出现登录结果。
这个请求，就绕过了前端验证，发送了空的用户名和密码给后端。

4、拦截响应
如果我们想要拦截服务器的响应信息，那么，在Proxy，Options选项卡中，勾选Intercept response based on the following rules即可。
![拦截响应](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/web-safety-learning/拦截响应.jpg)

## Fiddler
Fiddler是一款优秀的Web调试工具，它可以记录所有的浏览器与服务器之间的通信信息（HTTP和HTTPS），并且允许你设置断点，修改输入/输出数据。官方下载地址：http://www.telerik.com/fiddler

1、监听HTTP请求
安装完成后，无需任何设置，Fiddler就可以监听所有浏览器和其他应用（比如360、QQ）发出的HTTP请求和响应。
![监听请求和响应](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/web-safety-learning/监听请求和响应.jpg)

2、监听HTTPS请求
Fiddler默认不记录HTTPS请求，选择Tools，Fiddler Options，HTTPS，勾选Decrypt HTTPS traffic复选框，单击OK。
![https](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/web-safety-learning/https.jpg)

3、设置断点
方法一：通过Rules，Automatic Breakpoints，选择断点的插入点，三个选项分别是请求之前、响应之后和不拦截。插入断点之后，会应用到所有的请求和响应。

方法二：通过命令进行断点设置。例如，`bpu www.baidu.com`，会拦截所有发往`www.baidu.com`的请求；`bpafter www.baidu.com`，会拦截所有来自`www.baidu.com`的响应；`bpu`和`bpa`，会清除断点。

设置完断点后，我们就可以和使用Burp一样，来拦截HTTP请求并且修改请求信息了。

4、不要误滑滚轮
我们可以利用Fiddler自己定义返回给浏览器的内容，非常方便。但是，只要点击“Break on Response”，并且滑动滚轮，自定义内容就默认设置为一张图片，你可以选择其他自定义返回的内容，但是，无法再正常返回。
所以，切记不要误滑滚轮！！！

5、中文乱码
在response信息中，经常可以看到乱码。
因为Fiddler是默认按照UTF-8编码解码的，但是很多网站采用的是GB2312/GBK/GB18030编码，却没有在HEADER中指明编码方式。

解决办法：打开注册表编辑器，找到`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fiddler2\`，添加字符串值`HeaderEncoding`，value为`GB18030`，然后重启Fiddler。
![注册表](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/web-safety-learning/注册表.jpg)
但是这样一来，UTF-8编码并且没有指明编码方式的文件，不就变成了乱码了么？所以，建议不要修改。

6、发起HTTP请求
Fiddler，可以代替HttpRequester，用来测试接口。
![接口测试](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/web-safety-learning/接口测试.jpg)

## WinSock Expert
Winsock Expert是一个用来监视和修改网络发送和接收数据的程序，可以用来帮助渗透测试人员调试网络应用程序。
然而，WSE在Win8下的兼容性并不是很好，所以，建议换用WireShark。
推荐另外几款抓包工具：MiniSinffer、Iptool和Sniffer。

# 搜索引擎劫持
症状1：直接输入域名可以访问自己的网站，但是通过百度、谷歌等搜索引擎搜索关键字看到自己的网站后，再打开却跳转到其他的网站。
原理：攻击目标服务器，获取到网站源代码。然后修改后端代码，当HTTP请求头的Referer含有`baidu`、`google`等关键字时，控制跳转到其他网站。

症状2：无论通过哪种方式访问自己的网站，都会跳转到其他网站。
原理：攻击目标服务器，获取到网站源代码。然后修改后端代码，控制跳转到其他网站；或者修改前端代码，控制跳转到其他网站。

# 参考书籍
《Web安全深度剖析》，张炳帅

# 书签
Linux中文显示乱码？如何设置centos显示中文
http://jingyan.baidu.com/article/ab69b270de8b4f2ca7189f1d.html

解决centos7命令行中文乱码
http://www.centoscn.com/CentosBug/osbug/2015/0313/4873.html

centos/redhat中文支持安装
http://jingyan.baidu.com/article/8275fc86b7b36446a13cf648.html

CentOS更改yum源与更新系统
http://www.cnblogs.com/lightnear/archive/2012/10/03/2710952.html

CentOS 7 使用阿里云的yum源
http://blog.csdn.net/skykingf/article/details/51953700

FreeBuf.COM | 关注黑客与极客
http://www.freebuf.com/

漏洞盒子 | 互联网安全测试平台
https://www.vulbox.com/

Fiddler 高级用法：Fiddler Script 与 HTTP 断点调试
http://www.open-open.com/lib/view/open1429059806736.html

Fiddler工具使用
http://www.imooc.com/learn/37

web debugger fiddler 使用小结
http://www.cnblogs.com/forcertain/archive/2012/11/29/2795139.html

《Wireshark协议分析从入门到精通》
http://edu.51cto.com/lesson/id-62643.html


