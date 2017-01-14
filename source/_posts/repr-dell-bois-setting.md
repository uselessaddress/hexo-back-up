title: Dell的BIOS设置
date: 2013-11-26 18:19:23
tags: [系统,操作系统]
categories: 精华转载
---
对于戴尔预装了 Microsoft Windows 8 的笔记本电脑，BIOS 中的 Boot （启动）选项，在默认情况下的设置为

| 选项  |  值  |
| --------   | :----:  |
| Secure Boot     | [Enabled] | 
| Load Legacy Option Rom       |   [Disabled]   | 
| Boot List Option       |    [UEFI]   | 
如下图所示：
![](http://voidking.qiniudn.com/@/imgs/os/初始设置.jpg)

在这种设置下，如果使用者希望从光驱或其它USB设备启动，有时候会发生无法引导的情况。此时，需要对 BIOS 的选项做如下修改。

| 选项  |  值  |
| --------   | :----:  |
| Secure Boot	| [Disabled] |
| Load Legacy Option Rom  |    	[Enabled] |
| Boot List Option	| [Legacy] |
<!--more-->
修改后，能看到 Boot 选项中列出了 USB 存储与光驱等设备。
![](http://voidking.qiniudn.com/@/imgs/os/设置.jpg)

BIOS 修改好后保存退出，重启电脑的时候按键盘的 F12，重新进入引导菜单，此时就能够从连接的 USB 设备或光驱进行引导。
另外请注意：
对于支持 UEFI 启动的机型，如果开机无法进入系统，提示 “internal hard disk drive not found” 错误，如下图所示。
![](http://voidking.qiniudn.com/@/imgs/os/无法识别.jpg)

请进入 BIOS 检查硬盘在 BIOS 中是否能够被正常识别。如果可以，进入 Boot 页面，务必将 Secure Boot 改为[Disable]。
此情况经常发生在用户删除了原厂预装的 Windows 8 并装为 Windows 7 或其它系统之后。因为非 Windows 8 操作系统，不能支持 Secure Boot。

