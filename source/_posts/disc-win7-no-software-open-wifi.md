title: Win7不用软件开wifi热点
date: 2013-11-27 18:01:37
tags: [win7,wifi]
categories: 点滴发现
---
## 标准设置流程
首先**win+R**，输入**cmd**，打开命令符提示界面，然后用以下命令来完成设置

```
netsh wlan set hostednetwork mode=allow
netsh wlan set hostednetwork ssid="ihelloworld"
netsh wlan set hostednetwork key="helloworld"
netsh wlan refresh hostednetwork key
netsh wlan start hostednetwork 
```
如果有小伙伴不会做，请直接下载文件[wifi.bat](http://pan.baidu.com/s/1o66wcMe),然后双击即可。
然后**必须执行**的一步是：
```
打开网络和共享中心——更改适配器设置——右击你正在使用的网络连接——属性——共享——然后把两个勾都勾上
```
至此，理论上你已经成功开启了名为“ihelloworld”，密码为“helloworld”的wifi。如果有什么问题，欢迎留言或直接联系我。

## 常用命令集锦
有时候通过以上命令无法成功启动无线，就需要下面的命令来调试。

显示支持的承载网络
> netsh wlan show drivers

显示你的无线承载网络的信息
> netsh wlan show hostednetwork

启动无线承载网络
> netsh wlan start hostednetwork

停止你的无线承载网络
> netsh wlan stop hostednetwork
<!--more-->
更改网络名称
> netsh wlan set hostednetwork ssid="你的名称"

更改密码
> netsh wlan set hostednetwork key="你的密码"

如果想密码立即生效可以用
> netsh wlan refresh hostednetwork key

最后就是你真的不用这个无线网络了，你停止之后，也不想看到多出来的那块无线网卡，那么就执行
> netsh wlan set hostednetwork mode=

就再也看不到那块网卡了，当然你想重新开启的话就执行
> netsh wlan set hostednetwork mode=allow



**PS：如何设置才能让它开机自动启动？**
在win7系统下可以这样设置，打开记事本，输入netsh wlan start hostednetwork，保存（文件名为”启动wifi.bat”，保存类型选所有文件）。打开开始-所有程序-附件-系统工具-任务计划程序-创建基本任务-填写名称（自己命名）-下一步-选择计算机启动时-下一步-浏览-找你建的那个文件选定-下一步-完成。OK了,下次在你开机的时候就会自己启动了。
