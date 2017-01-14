title: jQuery学习笔记——事件篇
date: 2014-11-19 15:09:44
tags: [JavaScript,jQuery]
categories: 设计开发
toc: true
---

### 前言
任何基于GUI的现代应用程序都是基于事件驱动的，Web应用程序也不例外。

所有事件驱动的应用程序都采用相同的工作模式：建立事件机制、等待相关事件发生（比如鼠标单击）、对该事件做出相应。

### 浏览器事件模型

忆苦思甜，先了解一下传统的事件模型的缺点，我们才能明白改革开放是多么好的政策！

#### DOM第0级事件模型
也许你听说过网景事件模型、基本事件模型、浏览器事件模型，但是大多数人称其为DOM第0级事件模型。

为什么称其为第0级事件模型呢？因为，虽然该模型并不是一个正式的标准，但是所有主流的浏览器都与之兼容。而且，所有的现代浏览器依然支持这种模型。
<!--more-->

#### 插播广告

几乎所有的标准、协议、规范，都是在一门技术出现甚至成熟之后，才会形成。所以，我们也不能责怪W3C组织，没有及时给网景推出的事件模型一个名分，毕竟技术要领先于标准。


#### DOM第2级事件模型
等一下？void，你是不是把DOM第1级事件模型漏掉了？
没有漏掉，因为没有DOM第1级事件模型。DOM级别1于1998年10月成为W3C推荐标准，但是，该标准中并没有定义事件相关的内容。直到2000年11月，DOM级别2被引入时，W3C才真正为事件处理建立了标准模型。

这个模型得到了所有标准兼容的现代浏览器的支持，比如Firefox、Safari、Opera等。IE浏览器特立独行，它支持DOM第2级事件模型的一个功能子集。

#### IE事件模型
刚才说到，IE不支持DOM第2级事件模型。IE为每个DOM元素定义了一个名为attachEvent()的方法，而不是addEventListener()。

#### 小结
以上你可以认为是废话……总而言之，想要使用事件模型，不得不考虑到兼容问题，非常麻烦！这时，我们的jQuery事件模型华丽登场！

### jQuery事件模型，

jQuery把不一致的代码从页面代码中提取出来，将其隐藏在API中，因此，我们终于不用再去考虑兼容性问题！

#### 使用jQuery绑定事件处理器
```
<html>
	<head>	
		<meta charset="UTF-8">
		<script type="text/javascript" src="jquery-1.10.2.js"></script>
		<script type="text/javascript">
			$(function(){
				$("img").bind("click",function(){alert("hello voidking");});
			});
		</script>
	</head>
	<body>
		<img src="./head.jpg" alt="头像"/>		
	</body>
</html>
```
```
<html>
	<head>	
		<meta charset="UTF-8">
		<script type="text/javascript" src="jquery-1.10.2.js"></script>
		<script type="text/javascript">
			$(function(){
				$("#voidking")
				.bind("click",function(){
					alert("hello voidking once!");
				})
				.bind("click",function(){
					alert("hello voidking twice!");
				})
				.bind("click",function(){
					alert("hello voidking three times!");
				});
			});
		</script>
	</head>
	<body>
		<img id="voidking" src="./head.jpg" alt="头像"/>		
	</body>
</html>
```
由上面两段代码我们发现，绑定事件很简单，只需要一个bind()函数。而且，一个事件可以对应多个处理器。
除了bind()函数，jQuery还为创建特定的事件处理器提供了一些便捷方法。比如one、focusin、focusout等。

#### 删除事件处理器

创建了一个事件处理器，那么它在页面剩余的生命周期里就是有效的。但是一些特殊的交互，要求根据一定标准删除处理器。这时我们就要用到unbind()函数，jQuery考虑的很周到啊！
```
<html>
	<head>	
		<meta charset="UTF-8">
		<script type="text/javascript" src="jquery-1.10.2.js"></script>
		<script type="text/javascript">
			$(function(){
				$("#voidking1").bind("click.voidking",function(){
					alert("hello voidking1");
					$("*").unbind('click.voidking');
				});
				
				$("#voidking2").bind("click.voidking",function(){
					alert("hello voidking2");
					$("*").unbind('click.voidking');
				});
				
			});
		</script>
	</head>
	<body>
		<img id="voidking1" src="./head.jpg" alt="头像"/>
		<img id="voidking2" src="./head.jpg" alt="头像"/>		
	</body>
</html>
```
上面这个例子中，我们实现了一个效果：有任何一张图片被点击了一次，那么两张图都不可以再被点击。

#### Event实例
使用bind()方法，无论是什么浏览器，Event实例都会作为第一个参数传入函数。

那么，怎么处理不同浏览器中Event实例中属性的差异呢？原来，jQuery定义了一个jQuery.Event对象，真正传入的函数的，是这个对象。这个对象复制了大部分原始Event的属性，忽略了Event实例的差异性。就像是一个接口，我们不需要管它怎么实现，只要调用就好了！

#### 预先管理事件处理器

当混合使用Ajax时，我们可能在页面的生命周期内频繁引入DOM元素或删除它们。在管理这些动态元素的事件处理器时，就绪处理器就起不到多大作用了，因为这些动态元素在就绪处理器执行时还不存在。

jQuery提供了live()方法，该方法允许预先为那些不存在的元素创建事件处理器。它的语法和bind()非常相似，看上去似乎比bind()更加强大，那么，用live()代替bind()不就好了吗？不是的，首先，“live”事件不是原生的“普通”事件。其次，live()方法只能应用于选择器，不能应用于衍生而来的包装集。

这里不再举例，下面的Ajax会详细探讨。

live()创建的选择器可以使用die()方法来解除绑定，语法和unbind()相似。

#### 触发事件处理器
触发事件，听起来就很难懂的样子，我们换个说法：模拟用户动作。比如点击，我们可以用代码去模拟用户点击，达到的效果与真实的鼠标点击是一样的。

```
<html>
	<head>	
		<meta charset="UTF-8">
		<script type="text/javascript" src="jquery-1.10.2.js"></script>
		<script type="text/javascript">
			$(function(){
				$("#voidking").bind("click",function(){
					alert("hello voidking");
				});					
				$("#voidking").click();//模拟用户单击事件
				$("#voidking").trigger("click");//模拟用户单击事件

				$("p").bind("myEvent", function (event, message1, message2) {
					alert(message1 + ' ' + message2);
				});
				$("p").trigger("myEvent", ["hello","trigger"]);	//弹出两次		
				$('p').triggerHandler("myEvent",["hello","triggerHandler"]); //弹出一次，只触发第一个p元素
				
				$("input").select(function(){
				
				});
				$("#trigger").click(function(){
					$("input").trigger("select");//触发select事件，执行select默认操作和select处理器内操作
				});
				$("#triggerHandler").click(function(){
					$("input").triggerHandler("select");//触发select事件，但是不会执行select默认操作
				});
			});	
		</script>
	</head>
	<body>
		<img id="voidking" src="./head.jpg" alt="头像"/>	
		<p></p>
		<p></p>
		<input type="text"  value="hello voidking" />
		<br />
		<button id="trigger">激活select事件，同时选中文本</button></br>
		<button id="triggerHandler">激活select事件，不选中文本</button>
	</body>
</html>
```
上面这个例子，基本上涵盖了触发事件管理器的所有知识点。这里不详细解释了，自己看注释，不懂的请百度或留言。

#### 更多
toggle: 这个方法在jQuery 1.8中宣告过时，在jQuery 1.9中已经移除。
为了写出一个图片大中小切换效果，各种百度谷歌，最终没搞出来。开始怀疑，toggle过时了，经过查看官方文档，果然！我靠，各位大神，你们13、14年写的教程，居然使用过时的代码……还有你，w3school，该更新了大哥！

### 结束语
以上总结不可能面面俱到，差不多了，就写到这儿吧！

### 参考文档
李炎恢的jQuery视频教程
《jQuery攻略（作者B.M.Harwani）》
《jQuery实战（作者Bear Bibeault、Yehuda Katz）》
《jQuery高级编程（作者 Cesar Otero、Rob Larsen）》
《jQuery Javascript 与CSS开发入门经典（作者Richard York）》


