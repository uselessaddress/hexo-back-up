title: jQuery学习笔记——概述篇
date: 2014-11-15 08:04:52
tags: [JavaScript,jQuery]
categories: 设计开发
toc: true
---

### jQuery是什么
jQuery是一个兼容多浏览器的Javascript库，类似于C语言中的"*.h"文件和Java中的"*.jar" 文件。

核心理念是write less,do more(写得更少,做得更多)。

jQuery是免费、开源的，使用MIT许可协议。jQuery的语法设计可以使开发者更加便捷，例如操作文档对象、选择DOM元素、制作动画效果、事件处理、使用Ajax以及其他功能。除此以外，jQuery提供API让开发者编写插件。其模块化的使用方式使开发者可以很轻松的开发出功能强大的静态或动态网页。

### jQuery能够做什么
1、简化编程。jQuery最大的优点，是简化了Javascript编程。本来需要很多行代码才能完成的功能，使用jQuery，时常一两行就够了。

2、跨平台。使用Javascript开发，是一件痛苦的事情，因为你不得不考虑到各种浏览器的兼容问题。而使用jQuery，你编写的程序可以很容易地实现跨浏览器平台。
<!--more-->
### jQuery简史

jQuery在2006年1月由美国人John Resig在纽约的barcamp发布，吸引了来自世界各地的众多JavaScript高手加入，由Dave Methvin率领团队进行开发。

如今，jQuery已经成为最流行的javascript库，在世界前10000个访问最多的网站中，有超过55%在使用jQuery。

在写这篇文章的时候，在谷歌搜索jQuery，返回大约 85,900,000条结果。jQuery每天都有新的官方插件和第三方插件产生，它们不断扩展jQuery的核心功能。


### hello voidking
C语言写的第一个程序是helloworld，今天，void自恋一把，第一个程序就和自己打招呼了！

1、官网下载jQuery文件（这里我使用的是jquery-1.10.2.js）。

2、新建index.html文件，内容如下：
```
<html>
<head>	
	<script type="text/javascript" src="jquery-1.10.2.js"></script>
	<script type="text/javascript">
		$(function(){
			alert("hello voidking");
			$("p").css("color","red");
		});
	</script>
</head>
<body>
	<p>
		welcome to jQuery world !
	</p>
</body>
</html>
```

3、打开浏览器，看到弹出的对话框了吧？大功告成！下面的内容，会对此程序作出解释。

### $
jQuery的一切功能都源自"$"对象，即一个美元符号对象（或美元符号方法），它可以用"jQuery"来代替。

美元符号既是一个对象，也是一个方法。这是因为它具有很多成员属性和方法可以调用，同时可以把它当成一个函数来调用。

### 延迟加载
我们在hellovoidking的代码中，使用$(function(){});进行首尾包裹，那么为什么要包裹这段代码呢？原因是，jQuery库文件是在body元素之前加载的，我们必须等待所有的DOM元素加载后，延迟支持DOM操作，否则就无法获取到。

为了延迟等待加载，JavaScript提供了一个事件为load，方法如下：
```
window.onload = function(){}; 
```
而jQuery提供的方法如下：
```
$(document).ready(function(){}); 
```
什么东东？和hello voidking中的代码不一样啊？原来，jQuery的延迟加载方法可以简写为：
```
$(function(){});
```

### jQuery选择器

jQuery最核心的组成部分就是：选择器引擎。用iPhone式的格言来说，“让选择器完成一切”就是jQuery的座右铭。

在使用jQuery的任何方法时，首先要做的就是选择页面中要操作的那些元素。

jQuery选择器继承了CSS的语法，可以对DOM元素的标签名、属性名、状态等进行快速准确的选择，并且不必担心浏览器的兼容性。jQuery选择器实现了CSS1~CSS3的大部分规则之外，还实现了一些自定义的选择器，用于各种特殊状态的选择。

选择器部分内容很多，我会在下一篇文章中详细研究探讨。

### 事件处理
对于事件的概念，我在《Javascript学习笔记——基础篇》中已经解释过，这里不再赘述。
jQuery的事件处理，内容也很多，我会在接下来的文章中探究。

### Ajax
jQuery的Ajax也是重点，必须的展开。我靠，肿么这么多！别吐槽了帅哥，写教程的void都快哭了，赶着去搞XMPP呢！

### 调试
个人喜欢的工具是：火狐浏览器 + firebug。

### 结束语
jQuery博大精深，还有什么单元测试、插件、特效制作啥的，void就不多说了，感兴趣的小伙伴自己查找资料吧！


### 参考文档
李炎恢的jQuery视频教程
《jQuery攻略（作者B.M.Harwani）》
《jQuery实战（作者Bear Bibeault、Yehuda Katz）》
《jQuery高级编程（作者 Cesar Otero、Rob Larsen）》
《jQuery Javascript 与CSS开发入门经典（作者Richard York）》