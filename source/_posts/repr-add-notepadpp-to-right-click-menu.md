title: 将Notepad++添加到右键菜单
date: 2014-11-05 09:06:20
tags: Notepad++
categories: 精华转载
---
转载自：http://www.noanylove.com/2012/04/notepad-is-added-to-the-context-menu/

Notepad++是一款功能强大的代码编辑器。美中不足的是，它居然没有提供集成到右键菜单的功能，不过没关系，我们简单的通过一个注册表文件就可以添加这个功能。

## 方法一
将下面的内容保存为notepad.reg: 
```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\*\Shell\用 Notepad++ 打开]
[HKEY_CLASSES_ROOT\*\Shell\用 Notepad++ 打开\Command]
@="D:\\编程工具\\Notepad++\\notepad++.exe \"%1\""
```
将 D:\\编程工具\\Notepad++\\notepad++.exe 修改为你的Notepad++的安装路径。注意，这里的文件路径用的是双斜线而不是单斜线。然后双击notepad.reg将其添加到注册表即可。

这样之后，我们在选中任意类型的文件上单击右键菜单，都可以看到“用 Notepad++ 打开”的选项了。如图一所示。
![图一：右键菜单][1]
<!--more-->
## 方法二
另外一种方法，是将Notepad++的快捷方式保存到“发送到”的目录中。在使用时，选中你想要打开的文件，将其发送到Notepad++的快捷方式，就可以通过Notepad++打开这个文件。“发送到”的目录可以通过在文件浏览器的路径中输入 shell:sendto ，然后敲击回车键直接进入。这种方法不仅适用于Notepad++，你还可以在“发送到”目录中添加更多程序的快捷方式，反正我是把PEID，IDA，还有一些其他的编辑器都一股脑的塞进去了，如图二所示。
![图二：“发送到”菜单][2]

两种方法各有优缺点，在这里因为我使用Notepad++的频率非常高，第一种方式显然更加高效一点，所以我选择了第一种方法。


  [1]: http://voidking.qiniudn.com/@/imgs/%E5%9B%BE%E4%B8%80%EF%BC%9A%E5%8F%B3%E9%94%AE%E8%8F%9C%E5%8D%95.png
  [2]: http://voidking.qiniudn.com/@/imgs/%E5%9B%BE%E4%BA%8C%EF%BC%9A%E2%80%9C%E5%8F%91%E9%80%81%E5%88%B0%E2%80%9D%E8%8F%9C%E5%8D%95.png