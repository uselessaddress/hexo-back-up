title: 将文件夹取消与SVN关联
date: 2014-11-25 21:47:03
tags: SVN
categories: 精华转载
---

转载自[sznmail](http://sznmail.iteye.com/blog/347638)

```
Windows Registry Editor Version 5.00
[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Folder\shell\DeleteSVN]
@="删除该目录下面.svn文件"
[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Folder\shell\DeleteSVN\command]
@="cmd.exe /c \"TITLE Removing SVN Folders in %1 && COLOR 9A && FOR /r \"%1\" %%f IN (.svn) DO RD /s /q \"%%f\" \"" 
```

（1）把上面这段文字保存为“取消SVN文件夹.reg”文件。

（2）双击这个文件,导入到注册表。

（3）右键一个文件夹的时候，会多出来一个菜单"删除该目录下面.svn文件"，执行该命令即可。