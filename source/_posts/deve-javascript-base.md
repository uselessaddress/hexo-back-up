title: Javascript学习笔记——基础篇
date: 2014-11-13 10:57:02
tags: JavaScript
categories: 设计开发
toc: true
---

### 什么是Javascript
Javascript是一种基于对象的脚本语言。

基于对象（Object-Based）不提供抽象、继承、重载等有关面向对象语言的功能。而是把其他语言创建的对象统一起来，形成一个对象系统，以供使用。

脚本语言最大的特点就是不需要编译和链接。传统编程语言有四个步骤“编写->编译->链接->运行”，而脚本语言只有两个步骤“编写 -> 运行”。

脚本语言是解释执行而非编译执行。windows下，命令提示符界面，就是输入脚本语言的shell；经常见到的“*.bat”批处理文件，就是脚本文件。而在linux系统里面，脚本、脚本编程的概念更是常见。

shell，提供用户使用界面的软件。在windows中，win+R，输入cmd，出现的那个黑黝黝的窗口就是一个shell；打开任务管理器，看到的那个explorer.exe程序，也是一个shell，它叫做GUI shell。在linux中，如果不使用图形用户界面，那么，你所看到的，就是一个shell，一般的linux系统都会提供几种shell供你选择；而如果使用图形用户界面，你看到的界面，就是一个GUI shell。
<!--more-->
### 什么是对象
一切都是对象。比如“你”就是一个对象，你拥有姓名、性别、身高、体重等等属性信息，也有跳跃、奔跑、吃饭、睡觉等等方法。

Javascript中使用的对象，分为内置对象和自定义对象。

常见的内置对象有Array、String、Math、Date、Document、Form、Anchor、Link、Image、Windows、Screen、Navigator、Location、History、Frame、DOM、RegExp、StyleSheet、Event、FileSystemObject、Drive、Folder、File、XMLHttpRequest、Error……


### Javascript出现的原因

1、表单验证。早期的验证全部在服务端，比如注册时需要输入邮箱，而邮箱有固定格式，中间有一个“@”。为了验证一个邮箱格式，就需要发送请求给服务端，服务端的压力很大，多么令人蛋疼。为了减轻服务器的压力，需要一种可以运行在客户端的精小高效的语言。

2、网页交互性。随着互联网的发展，单纯的网页浏览已经无法满足用户的需求，越来越多的用户渴望与网页进行交互。为了改善用户的交互体验，需要开发一种语言，而这种语言运行在服务端是不合适的。

火药的发明，最初并没有想到可以用来打仗。Javascript的作者也没想到，Javascript能够发展到现在这么强大：
（1）在浏览器的状态栏或者警告框里，向访问者显示信息。
（2）验证表单内容。
（3）当访问者将鼠标指针移动到图像上面时，自动替换图像。
（4）创建与访问者交互的广告栏，而不仅仅是显示一幅图像。
（5）检测可用浏览器或其属性，并且只在支持它们的浏览器上运行高级功能。
（6）检测已安装的插件，并在需要某一插件时通知访问者。
（7）在不需要访问者重新加载网页的情况下，修改整个或部分网页。
（8）显示从远程服务器检索到的数据，或者与远程服务器交互数据。
……
总结一下，大致分为表单验证、网页特效、浏览器检测。

### 语法
汉语有语法，英语有语法，编程语言也有语法，Javascript也不例外。

语法，简而言之，就是语言表达的规则。你说“我真帅！”，大家理解你的意思，但是你说“帅真我！”，就没有人明白了！同样的，只有遵循一定的规则，浏览器才能明白你写的某句代码的意思。

#### 数据类型
数值型、字符串、布尔型、null、undefined、对象、数组

#### 变量和常量
和C、Java基本相同，不多解释，如果你没有学过编程，就按照数学中的理解就好了。

#### 运算符和表达式
和C、Java基本相同，不懂的同学请自行百度、读书，此处不展开了。

#### 流程
和C、Java基本相同，学过C、Java的同学请自行跳过本节。

其实所有的编程语言的执行流程都可以分为四种：
（1）顺序。排队买饭，就是一个顺序流程。

（2）条件。假设有两个窗口，窗口一卖饭，窗口二卖汤。如果你想打饭，就去窗口一；如果你想打汤，就去窗口二。这里的“想打饭”和“想打汤”就是条件。

（3）循环。排队买饭，终于排到我了，这时我发现钱不够，于是我回宿舍拿钱；之后回来排队，终于又排到我了，钱还是不够，于是我又回宿舍拿钱……这个过程，就是循环。终于有一次，我的钱拿够了，买到了饭，这个循环就结束了。

（4）其他。比如continue、break、异常处理等。
continue，排队买饭，还没有排到我，我发现钱没带，于是提前结束本次排队，回宿舍拿钱，回来后重新排队。
break，排队买饭，还没有排到我，突然不想买了，于是结束排队。
异常处理，排队买饭，突然收到一个紧急电话，必须去跑1000米。这个紧急电话，就是一个异常，跑1000米，就是处理。

#### 函数和程序

函数是用来实现一个功能的程序块。

举个例子，打电话告诉老爸缺钱了。这个过程，有两个动作，“打电话”和“告诉”。这里的“打电话”和“告诉”就是两个函数，也就是两个程序块。

程序块是什么？那些用大括号括起来的代码就是程序块。

程序呢？程序就是命令序列的集合。通俗一点说，程序就是做一件事情的步骤的集合。

“打电话”是一个程序，可以分为这样几个步骤：掏出手机，找到号码，拨号，等待接通。

当然你可以分的更细，比如“掏出手机”也可以看成一个程序，分为这样几个步骤：伸手拿到手机，提高一厘米，提高一厘米，提高一厘米……

#### 闭包

闭包是啥玩意？各种专业文档中给出了void根本看不懂的定义。这里借用阮一峰的理解：闭包就是能够读取和保持其他函数内部变量的函数。

1、作用域
要理解闭包，首先必须理解Javascript的变量作用域。变量作用域无非两种：全局变量和局部变量。

Javascript特有“链式作用域”结构。js寻找变量的定义会从最近的区域开始，本地找不到就往上一层区域，直到找到命名空间的顶端，在浏览器的世界里顶端就是window对象了。所以，父对象的所有变量，对子对象都是可见的，反之则不成立。

2、一个例子
```
<html>
	<head>	
	</head>
	<body>
		<script >
		function f1(){
				var n = "hello voidking";
				function f2(){
					alert(n);
				}
				return f2;
				}
				var result = f1();
				result();
		</script>
	</body>
</html>
```
运行这段代码，浏览器弹出了“hello voidking”对话框。

这段代码中的f2，就是一个闭包。

虽然外部函数已经返回，但是内部函数仍然记得外部函数定义的变量。

由于在Javascript中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成“定义在一个函数内部的函数”。

在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。功能类似于Java对象中的set和get函数。


3、传引用
闭包可以更新外部变量的值。

Javascript中函数的传参是传值，而不是传引用。而在闭包中，其存储的是一个对外部变量的引用。


### 正则表达式
说到正则表达式，很多人会想到通配符。正则表达式和通配符的不同在哪里呢？

1、通配符是系统级的，在bash shell界面下可以直接使用；正则表达式是需要工具支持的，比如grep、sed、awk等工具。
2、通配符一般用来查找文件、文件夹；正则表达式的匹配更加精确，主要用来文本过滤和字符串的操作。

Javascript对于正则表达式有很好的支持，也就是说，我们可以很方便的进行文本过滤和字符串操作。比如开篇提到的邮箱格式验证，我们就可以用正则表达式来实现。

### DOM
DOM，文档对象模型（Document Object Model）。

DOM独立于语言和平台，是一套标准接口，用来对XML和HTML文档进行增删查改。

DOM规范的核心就是树模型，对于要解析的HTML文档，解析器会把HTML文档加载到内存中，在内存中为HTML文件建立逻辑形式的节点树。而每一个节点，代表一个可以进行交互的对象。

### XML 
XML，可扩展标记语言（eXtensible Markup Language）。

它的格式很HTML很像，但是，HTML的标记是固定的，不区分大小写；而XML的标记是自定义的，区分大小写的。

XML文档结构分为3个部分：序言、主体和尾声。其中尾声可有可无。
如果把一个HTML文档看做是一个主体（一棵树），那么，XML和HTML文档结构的不同，就在于XML多了一个序言，而且，主体可能不止一棵树。

### 事件
编程语言中的事件，我们可以简单理解为事情。

当一件事情发生，我们可能会产生反应，也许不会。

比如，你们的老师让你们写一篇实验报告，这就是一个事件。这个事件发生后，你就会去写实验报告，这就是反应。也许你会吐槽，告诉远方的某位姑娘，老师多么无聊，让你们写实验报告。这时，虽然姑娘也知道了这件事，但是微微一笑（没有去写报告吧），或者连反应都没有。而更多的不知道这个事件的人，当然就当做没发生。

Javascript中的事件，提供了与窗口以及当前加载文档交互的基础。而Javascript对事件的处理，也就是通常所说的反应。

### Cookie
Cookie（小甜饼）是由浏览器存储在客户端系统的文本，而且与浏览器的每次请求一同发送到服务器。使用Cookie可以方便地帮助Web服务器保存有关访客的信息。

Cookie常用于以下场合：
（1）保存用户登录状态
（2）跟踪用户行为
（3）定制界面
（4）创建购物车

### Ajax
Ajax，异步Javascript和XML（Asynchronous Javascript and XML），被称为远程脚本技术。

Javascript在早期，和服务器通信的方法只有一个——提交表单，远程脚本技术使两者之间的通信变得更加丰富。它可以使Javascript超越客户端的界限，使其能够处理Web服务器上的文件。

特点：局部更新，节省带宽，提高加载速度。

### 结束语
看完上面的概念，是不是在想：什么玩意儿？看不懂没关系，知道有那么一回事就行，下一篇实践篇将会告诉你Javascript到底怎么玩。以上内容也许理解失误的地方，感谢大家留言指正。

### 参考文档
李炎恢的Javascript视频教程
《Javascript完全学习手册（作者张银鹤等）》
《Javascript基础教程（第七版，作者Tom Negrino、Dori Smith）》
《Javascript开发技术详解（作者李峰、晁阳）》
一些技术大牛的博客……
