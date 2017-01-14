title: NotePad++高亮Markdown
date: 2015-05-29 23:30:22
tags:
- Notepad++
- Markdown
categories: 设计开发
---

### 前言

编程三年，最喜欢的语言，是Java；最喜欢的IDE，是Eclipse；最喜欢编辑器，那就非Notepad++莫属了！平时写博客，都是用这个编辑器。新换了系统，安装好Notepad++。突然想到，很多语言Notepad++都可以高亮，而且它还支持自定义语言，那么，理论上应该可以支持高亮Markdown。经过查找资料，果然有办法！

### 解决办法
自定义语言。
自己来定义，当然可以。小编的做法是，直接使用雷锋写好的。在此分享Edditoria的作品给大家：
http://yunpan.cn/cwqMP9TNJnTCy  访问密码 d019
解压后，拷贝userDefineLang.xml到Notepad++的安装目录（小编的是`D:\Program Files\Notepad++`），重启Notepad++。可以看到，在语言选项中，多出了Markdown。


### 没有效果？
上面给出的压缩包中，有两个userDefineLang.xml，当我从一个更换另一个的时候，惊奇地发现，无论我重启Notepad++多少次，格式都不会发生变化！百思不得解，在仔细研究过Nodepad++的特性后，发现了这么一个文件夹：
`C:\Users\Administrator\AppData\Roaming\Notepad++`，里面存放的，应该是一些缓存文件。其中一个文件，正是userDefineLang.xml。真相大白了，直接在这个文件夹中替换userDefineLang.xml，再次打开一个.md文件查看，格式果然出现了变化。
所以，如果你要替换自定义的语言格式，建议你同时替换上述两个文件夹下的文件。
<!--more-->

### 参考文档
How to use markdown in notepad++
http://superuser.com/questions/586177/how-to-use-markdown-in-notepad
https://github.com/Edditoria/markdown_npp_zenburn

------

notepad++安装markdown插件
http://www.360doc.com/content/14/1225/12/4565_435634657.shtml

------
[写了一个Notepad++的markdown插件](http://blog.gclxry.com/%E5%86%99%E4%BA%86%E4%B8%80%E4%B8%AAnotepad%E7%9A%84markdown%E6%8F%92%E4%BB%B6/)

------
让Markdown成为日常文本记录语言 
http://www.cnblogs.com/purediy/archive/2013/01/10/2855397.html

------


