title: Javascript学习笔记——实践篇
date: 2014-11-14 19:27:20
tags: JavaScript
categories: 设计开发
toc: true
---


### 注释
本文中的文件名，写在了代码注释中。既然用到了三种文件，就把这三种文件的注释方法先说明一下。

#### HTML注释
语法`<!--注释的内容-->`,示例如下：
```
<!--欢迎来到VoidKing的主页-->
```

#### CSS注释
语法`/*注释的内容*/`，示例如下：
```
/*这是注释*/
/*
这也是注释
可以分段
*/
```
<!--more-->
#### Javascript注释
和C语言相同，语法`//注释的内容`或者`/*注释的内容*/`，示例如下：
```
//这是注释
/*
这也是注释
*/
```
### 使用Javascript
在html页面中使用Javascript，有三种方法：
#### body中
body中的Javascript代码，相当于C语言中位于main函数内代码，格式如下：
```
<!--index.html-->
<html>
	<head>
	</head>
	<body>
		<script type="text/javascript">
		document.write("voidking.com");
		alert("voidking.com");
		</script>
	</body>
</html>
```
#### head中
head中的Javascript代码，相当于C语言中位于main函数外的代码，一般是封装好的函数。格式如下：
```
<!--index.html-->
<html>
	<head>
		<meta http-equiv="Content-Type" content="charset=utf-8" />
		<script type="text/javascript">
		function hello(){
			var str=prompt("VoidKing的网址是什么？","请在这里输入");
			if(str!=null && str!="")
			{
				alert("你输入的是："+str);
			}
			else
			{
				alert("你什么也没有输入！");
			}
		}
		</script>
	</head>
	<body>
		<script type="text/javascript">
			hello();
		</script>
	</body>
</html>
```
#### 外部
head中的Javascript代码不易维护，所以多数情况下我们会使用外部引用来代替。格式如下：
```
<!--index.html-->
<html>
	<head>
		<meta http-equiv="Content-Type" content="charset=utf-8" />
		<script type="text/javascript" src="javascript.js"></script>
	</head>
	<body>
		<script type="text/javascript">
			hello();
		</script>
	</body>
</html>
```
```
//javascript.js
function hello(){
			var str=prompt("VoidKing的网址是什么？","请在这里输入");
			if(str!=null && str!="")
			{
				alert("你输入的是："+str);
			}
			else
			{
				alert("你什么也没有输入！");
			}
		}
```
### 使用CSS
上面我们已经说完了使用Javascript的三种方式，爱思考的小伙伴肯定想到了CSS的使用方法，在这里，我也总结一下。
#### 内联样式
当特殊的样式需要应用到个别元素时，就可以使用内联样式。格式如下：
```
<!--index.html-->
<html>
	<head>
	</head>
	<body>
		<p style="color:red;margin-left:20px">
		This is a paragraph.
		</p>
	</body>
</html>
```

#### 内部样式
当单个文件需要特别样式的时候，就可以使用内部样式表。格式如下:
```
<!--index.html-->
<html>
	<head>
		<style type="text/css">
		body{background-color:red}
		p{margin-left:20px}
		</style>
	</head>
	<body>
		<p>
		This is a paragraph.
		</p>
	</body>
</html>
```
#### 外部样式
当样式需要被应用到很多页面的时候，外部样式表最适合。格式如下：
```
<!--index.html-->
<html>
	<head>
		<link rel="stylesheet" type="text/css" href="style.css"/>
	</head>
	<body>
		<p>
		This is a paragraph.
		</p>
	</body>
</html>
```
```
/*style.css*/
body{background-color:red}
p{margin-left:20px}
```

### 借花献佛
本来想自己整理出一批好的例子，但是，无意中发现了一个网站，已经做得非常好了。所以，void在这里就偷懒一下。
例子精品，还附有教程讲解，分享给大家——[梦之都](http://www.dreamdu.com/javascript/novice/)！

### 结束语
这篇文章比预期的用时要短上很多，主要因为，我把痛苦的工作全部交给梦之都了，好机智地说！还想写个提高篇来着，但是能力有限，不能误人子弟啊！所以，Javascript笔记至此结束，接下来，该是jQuery了！