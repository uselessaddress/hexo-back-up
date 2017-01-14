title: "Sublime Text3"
toc: true
date: 2016-01-07 14:21:36
tags:
- 前端
categories: 设计开发
---
# 前言
> Sublime Text is a sophisticated text editor for code, markup and prose.
You'll love the slick user interface, extraordinary features and amazing performance.

<!--more-->

# 常用快捷键
- ctrl+D：选择单词，重复可增加选择下一个相同的单词。
- ctrl+L：选择行，重复可依次增加选择下一行。
- shift+ctrl键组合+↑↓。可实现类似鼠标选中之后移动的效果。
- ctrl+P：搜索项目中的文件，模糊匹配文件名。
- ctrl+R：前往 method。
- ctrl+F：查找字符串。
- ctrl+shift+F：在整个项目中查找字符串。如果快捷键不可用，则Find->Find in Files...。
- ctrl+H：查找并替换。
- ctrl+shift+P：打开命令面板。输入`set syntax:css`，可以设置语法为css。
- ctrl+shift+[：折叠代码段。
- ctrl+shift+V：粘贴并格式化。
- ctrl+enter：在当前行后插入一行。
- ctrl+shift+enter：在当前行前插入一行。
- ctrl+shift+D：快速复制光标所在的一整行，并复制到该行之前。
- ctrl+shift+K：删除一行。
- ctrl+shift+上下键：可替换行。
- ctrl+/：注释当前行。
- ctrl+shift+/：当前位置插入注释。
- ctrl+shift+A：选中标签内的内容不包括标签，继续按会继续往上一层选择。
- f11：全屏。
- shift+f11：全屏免打扰模式，只编辑当前文件。
- esc：退出各种面板。

# 安装插件
## 简单安装：
从菜单 View - Show Console 或者 ctrl + ~ 快捷键，调出 console。将以下 Python 代码粘贴进去并 enter 执行，不出意外即完成安装。以下提供 ST3 和 ST2 的安装代码：
Sublime Text 3：
```
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```

Sublime Text 2：
```
import urllib2,os; pf='Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler( ))); open( os.path.join( ipp, pf), 'wb' ).write( urllib2.urlopen( 'http://sublime.wbond.net/' +pf.replace( ' ','%20' )).read()); print( 'Please restart Sublime Text to finish installation')
```

## 手动安装：

可能由于各种原因，无法使用代码安装，那可以通过以下步骤手动安装Package Control：

1.点击Preferences > Browse Packages菜单

2.进入打开的目录的上层目录，然后再进入Installed Packages/目录

3.下载 [Package Control.sublime-package](https://sublime.wbond.net/Package%20Control.sublime-package) 并复制到Installed Packages/目录

4.重启Sublime Text。

Package Control 主文件下载地址：https://github.com/wbond/sublime_package_control

## 使用方法：

快捷键 Ctrl+Shift+P（菜单 – Tools – Command Paletter），输入 install 选中Install Package并回车，输入或选择你需要的插件回车就安装了（注意左下角的小文字变化，会提示安装成功）。

## 查看已安装插件
ctrl+shift+P，输入package，选择list packages。

## 卸载已安装插件
ctrl+shift+P，输入package，选择remove packages。

# 必装插件
## Soda
传说中完美的编码主题，官网：http://buymeasoda.github.io/soda-theme/

## Emmet
HTML/CSS代码快速编写神器，项目地址：https://github.com/sergeche/emmet-sublime#readme

## Javascript Completions
测试了多个js插件，这个是最好用的，项目地址：https://github.com/pichillilorenzo/JavaScript-Completions

## jQuery
提供了额外的语法高亮和几乎所有jQuery方法的片段，项目地址：https://github.com/SublimeText/jQuery/

## All Autocomplete
传统的Sublime Text自动补全插件仅仅在当前文件下工作。AllAutocomplete 可以搜索全部打开的标签页，这将极大的简化开发进程。

## SublimeREPL
SublimeREPL 可以直接在编辑器中运行一个解释器，支持很多语言。

## MarkdownEditing
可能是Markdonw最好的插件了：语法高亮，缩略词，自动补全，配色方案。

## SideBar Enhancements
改进了侧边栏，增加了许多功能。

## sublimelinter
语法检查插件，安装sublimelinter和sublimelinter-\*，\*为所用的语言，例如sublimelinter-php。

# 后记
工欲善其事，必先利其器。sublime，前端工程师的紫金装备。

# 参考文档
Sublime Text 3
http://www.sublimetext.com/3

Sublime text 2/3 中 Package Control 的安装与使用方法
http://www.imjeff.cn/blog/62/

Package Control 
https://packagecontrol.io/installation#st3

前端开发必备！Emmet使用手册
http://www.w3cplus.com/tools/emmet-cheat-sheet.html

如何将Emmet安装到到 Sublime text 3？
http://www.cnblogs.com/tinyphp/p/3217457.html

Sublime Text无法使用Emmet插件（附带手动安装）
http://www.jianshu.com/p/763d48bd9c02

全栈开发必备的10款Sublime Text 插件
http://www.php100.com/html/it/focus/2014/1128/7935.html

推荐！Sublime Text 最佳插件列表
http://blog.jobbole.com/79326/



