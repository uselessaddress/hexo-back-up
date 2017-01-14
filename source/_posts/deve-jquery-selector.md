title: jQuery学习笔记——选择器篇
date: 2014-11-17 00:05:25
tags: [JavaScript,jQuery]
categories: 设计开发
toc: true
---

### 选择器语法
jQuery选择元素的语法为：`$(selector,[content]);`

如果第一个参数是选择器，那么第二个参数就是指示该操作的上下文，默认为整个DOM文档。上下文参数可以是DOM元素的引用，也可以包含jQuery选择器的字符串，或者是DOM元素包装集。

上文hello voidking中的html文件内容修改为：
```
<html>
<head>	
	<script type="text/javascript" src="jquery-1.10.2.js"></script>
	<script type="text/javascript">
		$(function(){
			$("p","div#voidking").css("color","red");
		});
	</script>
</head>
<body>
	<div id="voidking">
		<p>
			welcome to jQuery world !
		</p>
	</div>
	<p>
		welcome to jQuery world !
	</p>
</body>
</html>
```
<!--more-->
在浏览器中打开，我们发现，只有第一行是红色的。这是因为，$("p","div#voidking")指定的范围是：id值为voidking的<div>元素内。

### CSS基本选择器
#### id选择器
获取一个id为voidking的元素。

在CSS中：
```
#voidking{
	color:red;
}
```
在jQuery中：
```
$("#voidking").css("color","red");
```
#### class选择器

获取所有class为voidking的元素。

在CSS中：
```
.voidking{
	color:red;
}
```
在jQuery中：
```
$(".voidking").css("color","red");
```
#### 标签选择器

获取所有标签名为div的元素。

在CSS中：
```
div{
	color:red;
}
```
在jQuery中：
```
$("div").css("color","red");
```
#### 群组选择器
获取标签名为span、em和class为voidking的所有元素。

在CSS中：
```
span,em,.voidking{
	color:red;
}
```
在jQuery中：
```
$("span,em,.voidking").css("color","red");
```

#### 后代选择器
获取标签名为ul下的标签名为li下的标签名为a的所有元素。

在CSS中：
```
ul li a{
	color:red;
}
```
在jQuery中：
```
$("ul li a").css("color","red");
```
或者
```
$("ul").find("li").find("a").css("color", "red");
```

#### 通配选择器
获取所有元素，一般不使用。

在CSS中：
```
*{
	color:red;
}
```
在jQuery中：
```
$("*").css("color","red");
```

#### 组合
上面的六种选择器，已经可以满足大部分的选择需要。而它们还可以结合起来使用。
```
$("div.voidking,ul li a").css("color","red");
```

### CSS高级选择器

#### 子选择器
选择id为voidking的元素下，子标签为p的元素。

在CSS中：
```
#voidking > p{
	color:red;
}
```
在jQuery中：
```
$("#voidking > p").css("color","red");
```
或者
```
$("#voidking").children("p").css("color", "red");
```

#### 同级下一个选择器
选择和id为voidking的元素同级的，下一个标签为p的元素。

在CSS中：
```
#voidking + p{
	color:red;
}
```
在jQuery中：
```
$("#voidking + p").css("color","red");
```
或者
```
$("#voidking").next("p").css("color", "red");
```

#### 同级所有下面选择器
选择和id为voidking的元素同级的，下面所有标签为p的元素。

在CSS中：
```
#voidking ~ p{
	color:red;
}
```
在jQuery中：
```
$("#voidking ~ p").css("color","red");
```
或者
```
$("#voidking").nextAll("p").css("color", "red");
```
#### PS
在find()、children()、next()、nextAll()四个函数中，如果不传入参数，默认为"*"。

建议使用方法而不是符号，理论上讲，使用方法的效率高于使用符号，而且，使用方法更加易读易懂。

接下来的选择器就没有类似于" "、">"、"+"、"~"这样的符号了，全部由函数来完成。

#### 同级上一个选择器
选择和id为voidking的元素同级的，上一个标签为p的元素。

```
$("#voidking").prev("p").css("color", "red");
```
#### 同级所有上面选择器
选择和id为voidking的元素同级的，上面所有标签为p的元素。
```
$("#voidking").prevAll("p").css("color", "red");
```
#### 同级上下所有选择器
```
$("#voidking").siblings("p").css("color", "red");
```
等价于
```
$("#voidking").prevAll("p").css("color", "red");
$("#voidking").nextAll("p").css("color", "red");
```
#### 非指定选择器
同级上、下非指定元素选定，遇到则停止。
```
$("#voidking").prevUntil("p").css("color", "red");
$("#voidking").nextUntil("p").css("color", "red");
```

### 属性选择器
#### 精确匹配
```
$('[id='voidking']').css("color",'red');
```
```
$('div[id='voidking']').css("color",'red');
```

#### 精确不匹配
```
$("p[class != 'voidking']").css("color",'red');
```

#### 匹配开头
```
$("[id ^= 'void']").css("color","red");
```

#### 匹配结尾
```
$("[id $= 'king']").css("color","red");
```

#### 其他

| CSS选择器 | jQuery选择器 | 描述 |
| -------  | ----: | :------: |
| elem[id] | $("elem[id]") | 选择具有id属性的元素 |
| elem[id \|= 'void'] | $("elem[id\ |= 'void']") | 选择具有id属性，且属性值以viod开头或者属性值等于void的元素 |
| elem[class ~= 'voidking'] | $("elem[id ~= 'voidking']") | 选择具有class属性，且属性值是一个以空格分格的列表，其中包含voidking的元素 |
| elem[id \*= 'oidki'] | $("elem[id \*= 'oidki']") | 选择具有id属性,且属性值中包含"oidki"字串的元素 |

### 过滤选择器

#### 位置选择器
| jQuery选择器 |  描述 |
| -------  | ------- |
| $("li:first") |  返回匹配集合的第一个元素 |
| $("li:last") |  返回匹配集合的最后一个元素 |
| $("li:odd") |  返回匹配集合的奇数成员 |
| $("li:even") |  返回匹配集合的偶数成员 |
| $("li:eq(3)") |  返回匹配集合的索引值等于3的元素（第4个元素） |
| $("li:not(3)") |  返回匹配集合的索引值不等于3的所有元素 |
| $("li:gt(2)") |  返回匹配集合的索引值大于2的所有元素 |
| $("li:lt(3)") |  返回匹配集合的索引值小于3的所有元素 |

#### 基本过滤选择器
基本过滤选择器包括位置选择器，比位置选择器多了一些东东：
| 过滤器 |  描述 |
| -------  | ------- |
| :animated |  选择当前正在执行动画的所有元素 |
| :header |  选择所有的标题元素，比如h1、h2、h3等 |

#### 过滤表单元素

| 过滤器 |  描述 |
| -------  | ------- |
| :text |  选择所有类型为text的元素 |
| :password |  选择所有类型为password的元素 |
| :radio |  选择所有类型为radio的元素 |
| :checkbox |  选择所有类型为checkbox的元素 |
| :checked |  匹配所有已被选中的元素 |
| :image |  选择所有类型为image的元素 |
| :file |  选择所有类型为file的元素 |
| :submit |  选择所有类型为submit的元素 |
| :reset |  选择所有类型为reset的元素 |
| :button |  选择所有button元素和类型为botton的元素 |
| :input |  选择所有input、textarea、select和button元素 |
| :selected |  选择所有类型已选中的元素 |
| :enabled |  选择所有可用元素 |
| :disabled |  选择所有不可用元素 |

#### 可见性过滤器

| 过滤器 |  描述 |
| -------  | ------- |
| :visible |  选择所有可见元素 |
| :hidden |  选择所有隐藏元素 |

#### 内容过滤器

| 过滤器 |  描述 |
| -------  | ------- |
| :contains() |  选择所有包含特定文本内容的元素 |
| :has() |  选择至少含有一个元素与制定选择器匹配的元素 |
| :empty |  选择所有不包含子元素或文本的空元素 |
| :parent |  选择所有含有子元素或文本节点的元素 |

#### 关系过滤器

| 过滤器 |  描述 |
| -------  | ------- |
| :first-child |  选择每个父元素的第一个子元素 |
| :last-child |  选择每个父元素的最后一个子元素 |
| :nth-child |  选择每个父元素的第nth-child()个子元素 |
| :only-child |  选择具有唯一一个子元素的元素 |

### 自定义选择器

有些时候，jQuery提供的选择器不够用，我们就需要自己创建选择器。比如，我们需要选择具有绿色背景的元素：

```html
<html>
<head>	
	<script type="text/javascript" src="jquery-1.10.2.js"></script>
	<script type="text/javascript">
		$(function(){
			//通过扩展$.expr[":"]实现自定义选择器
			$.expr[":"].greenbg = function(element){
				return $(element).css("background-color") === "green";
			};	
			//此处兼容性问题。输出在Firefox和IE中有所不同，Firefox值为0，IE值为1。
			alert($(":greenbg").length);
			$(":greenbg").text("hello voidking");
		});
	</script>
</head>
<body>
	<div style="width:200 ; height:200 ; background-color:green;" ></div>
	<div style="width:200 ; height:200 ; background-color:red;" ></div>
</body>
</html>
```
### 结束语
我靠，选择器这块实在是太难搞了！一天一夜才完成这份总结。内容纯手打，代码测试通过！

### 参考文档
李炎恢的jQuery视频教程
《jQuery攻略（作者B.M.Harwani）》
《jQuery实战（作者Bear Bibeault、Yehuda Katz）》
《jQuery高级编程（作者 Cesar Otero、Rob Larsen）》
《jQuery Javascript 与CSS开发入门经典（作者Richard York）》