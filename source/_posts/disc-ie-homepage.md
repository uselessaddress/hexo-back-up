title: IE首页被篡改解决办法
date: 2013-05-16 11:41:34
tags: IE
categories: 点滴发现
---

解决办法1:使用360。

解决办法2:
1.起始页的修改。展开注册表到HKEY_LOCAL_MACHINE\Software\Microsoft\Internet Explorer\Main，在右半部分窗口中将"Start Page"的键值改为"about:blank"即可。

同理，展开注册表到HKEY_CURRENT_USER\Software\Microsoft\Internet Explorer\Main，在右半部分窗口中将"Start Page"的键值改为"about:blank"即可。

注意:有时进行了以上步骤后仍然没有生效，估计是有程序加载到了启动项的缘故，就算修改了，下次启动时也会自动运行程序，将上述设置改回来，解决方法如下:
运行注册表编辑器Regedit.exe，然后依次展开HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run主键，然后将下面的"registry.exe"子键(名字不固定)删除，最后删除硬盘里的同名可执行程序。退出注册编辑器，重新启动计算机，问题就解决了。

2.默认主页的修改。运行注册表编辑器，展开HKEY_LOCAL_MACHINE\Software\Microsoft\Internet Explorer\Main\，将Default-Page-URL子键的键值中的那些恶意网站的网址改正，或者设置为IE的默认值。

注:如果无法修改,请进入安全模式。

