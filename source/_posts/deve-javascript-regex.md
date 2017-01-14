---
title: JavaScript正则表达式
toc: true
date: 2015-02-12 12:37:03
tags:
- JavaScript
- 前端
- 正则表达式
categories: 设计开发
---
# 前言
正则表达式（Regular Expression）主要是用来描述一个句法规则的模式。其实说的通俗一点，就是利用字符和元字符的组合，对一些符合既定句法的模式进行模糊匹配。它的主要功能是文本查询和字符串操作。本文讨论一下JavaScript中的正则表达式用法。

<!--more-->

# 定义正则表达式

1、定义正则表达式有两种形式，一种是普通方式，一种是构造函数方式。
2、普通方式：var reg=/表达式/附加参数
表达式：一个字符串，代表了某种规则，其中可以使用某些特殊字符，来代表特殊的规则，后面会详细说明。
附加参数：用来扩展表达式的含义，目前主要有三个参数：
g：代表可以进行全局匹配。
i：代表不区分大小写匹配。
m：代表可以进行多行匹配。
上面三个参数，可以任意组合，代表复合含义，当然也可以不加参数。
例子：
```
var reg=/a*b/;
var reg=/abc+f/g;
```
3、构造函数方式：var reg=new RegExp("表达式","附加参数");
其中“表达式”与“附加参数”的含义与上面那种定义方式中的含义相同。
例子：
```
var reg=new RegExp("a*b");
var reg=new RegExp("abc+f","g");
```
4、普通方式与构造函数方式的区别
普通方式中的表达式必须是一个常量字符串，而构造函数中的表达式可以是常量字符串，也可以是一个js变量，例如根据用户的输入来作为表达式参数等等：
```
var reg=new RegExp(document.forms[0].exprfiled.value,"g");
```

# 表达式模式

1、表达式模式，是指表达式的表达方式与样式， 即 var reg=/表达式/附加参数 中的“表达式”怎样去描述？
2、从规范上讲，表达式模式分为简单模式和复合模式。
3、简单模式：是指通过普通字符的组合来表达的模式，例如
```
var reg=/abc0d/;
```
可见简单模式只能表示具体的匹配。
4、复合模式：是指含有通配符来表达的模式，例如：
```
var reg=/a+b?\w/;
```
其中的+、?和\w都属于通配符，代表着特殊的含义。因此复合模式可以表达更为抽象化的逻辑。
复合模式中各个通配符的含义及其使用请阅读下文给出的参考文档。

# 表达式操作

1、表达式操作，在这里是指和表达式相关的方法，我们将介绍六个方法。
2、表达式对象（RegExp）方法：

（1）exec(str)，返回str中与表达式相匹配的第一个字符串，而且以数组的形式表现，当然如果表达式中含有捕捉用的小括号，则返回的数组中也可能含有()中的匹配字符串，例如：
```
var regx=/\d+/;
var rs=regx.exec("3432ddf53");
```
返回的rs值为：{3432}
```
var regx2=new RegExp("ab(\d+)c");
var rs2=regx2.exec("ab234c44");
```
返回的rs2值为：{ab234c,234}
另外，如果有多个合适的匹配，则第一次执行exec返回一个第一个匹配，此时继续执行exec，则依次返回第二个第三个匹配。例如：
```
var regx=/user\d/g;
var rs=regx.exec("ddduser1dsfuser2dd");
var rs1=regx.exec("ddduser1dsfuser2dd");
```
则rs的值为{user1}，rs1的值为{user2}，当然注意regx中的g参数是必须的，否则无论exec执行多少次，都返回第一个匹配。后面还有相关内容涉及到对此想象的解释。

（2）test(str)，判断字符串str是否匹配表达式，返回一个布尔值。例如：
```
var regx=/user\d+/g;
var flag=regx.test("user12dd");
```
flag的值为true。

3、String对象方法

（1）match(expr)，返回与expr相匹配的一个字符串数组，如果没有加参数g，则返回第一个匹配，加入参数g则返回所有的匹配
例子：
```
var regx=/user\d/g;
var str="user13userddduser345";
var rs=str.match(regx);
```
rs的值为：{user1,user3}

（2）search(expr)，返回字符串中与expr相匹配的第一个匹配的index值。
例子：
```
var regx=/user\d/g;
var str="user13userddduser345";
var rs=str.search(regx);
```
rs的值为：0

（3）replace(expr,str)，将字符串中匹配expr的部分替换为str。另外在replace方法中，str中可以含有一种变量符号$，格式为$n，代表匹配中被记住的第n的匹配字符串（注意小括号可以记忆匹配）。
例子：
```
var regx=/user\d/g;
var str="user13userddduser345";
var rs=str.replace(regx,"00");
```
rs的值为：003userddd0045
例子2：
```
var regx=/u(se)r\d/g;
var str="user13userddduser345";
var rs=str.replace(regx,"$1");
```
rs的值为：se3userdddse45
对于replace(expr,str)方法还要特别注意一点，如果expr是一个表达式对象则会进行全局替换（此时表达式必须附加参数g，否则也只是替换第一个匹配），如果expr是一个字符串对象，则只会替换第一个匹配的部分，例如：
```
var regx="user"
var str="user13userddduser345";
var rs=str.replace(regx,"00");
```
rs的值为： 0013userddduser345

（4）split(expr)，将字符串以匹配expr的部分做分割，返回一个数组，而且表达式是否附加参数g都没有关系，结果是一样的。
例子：
```
var regx=/user\d/g;
var str="user13userddduser345";
var rs=str.split(regx);
```
rs的值为：{3userddd,45}

# 表达式相关属性

1、表达式相关属性，是指和表达式相关的属性，如下面的形式：
```
var regx=/myexpr/;
var rs=regx.exec(str);
```
其中，和表达式自身regx相关的属性有两个，和表达式匹配结果rs相关的属性有三个，下面将逐一介绍。
2、和表达式自身相关的两个属性：

（1）lastIndex，返回开始下一个匹配的位置，注意必须是全局匹配（表达式中带有g参数）时，lastIndex才会有不断返回下一个匹配值，否则该值为总是返回第一个下一个匹配位置，例如：
```
var regx=/user\d/;
var rs=regx.exec("sdsfuser1dfsfuser2");
var lastIndex1=regx.lastIndex;
rs=regx.exec("sdsfuser1dfsfuser2");
var lastIndex2=regx.lastIndex;
rs=regx.exec("sdsfuser1dfsfuser2");
var lastIndex3=regx.lastIndex; 
```
上面lastIndex1为9，第二个lastIndex2也为9，第三个也是9；如果regx=/user/d/g，则第一个为9，第二个为18，第三个为0。

（2）source，返回表达式字符串自身。例如：
```
var regx=/user\d/;
var rs=regx.exec("sdsfuser1dfsfuser2");
var source=regx.source;
```
source的值为user\d
3、和匹配结果相关的三个属性：

（1）index，返回当前匹配的位置。例如：
```
var regx=/user\d/;
var rs=regx.exec("sdsfuser1dfsfuser2");
var index1=rs.index;
rs=regx.exec("sdsfuser1dfsfuser2");
var index2=rs.index; 
rs=regx.exec("sdsfuser1dfsfuser2");
var index3=rs.index; 
```
index1为4，index2为4，index3为4，如果表达式加入参数g，则index1为4，index2为13，index3会报错（index为空或不是对象）。

（2）input，用于匹配的字符串。例如：
```
var regx=/user\d/;
var rs=regx.exec("sdsfuser1dfsfuser2");
var input=rs.input;
```
input的值为sdsfuser1dfsfuser2。

（3）[0]，返回匹配结果中的第一个匹配值，对于match而言可能返回一个多值的数字，则除了[0]外，还可以取[1]、[2]等等。例如：
```
var regx=/user\d/;
var rs=regx.exec("sdsfuser1dfsfuser2");
var value1=rs[0];
rs=regx.exec("sdsfuser1dfsfuser2");
var value2=rs[0]; 
```
value1的值为user1,value2的值为user2

# 实际应用

1、实际应用一
描述：有一表单，其中有一个“用户名”input域
要求：汉字，而且不能少于2个汉字，不能多于4个汉字。
实现：
```
<script>
function checkForm(obj){
     var username=obj.username.value;
     var regx=/^[\u4e00-\u9fa5]{2,4}$/g;
     if(!regx.test(username)){
               alert("Invalid username!");
               return false; 
     }
     return true;
}
</script>

<form name="myForm" onSubmit="return checkForm(this)">
    <input type="text" name="username"/>
    <input type="submit" vlaue="submit"/>
</form>
```
2、实际应用二
描述：给定一个含有html标记的字符串，要求将其中的html标记去掉。
实现：
```
<script>
function toPlainText(htmlStr){
   var regx=/<[^>]*>|<\/[^>]*>/gm;
   var str=htmlStr.replace(regx,"");
   return str;
}
</script>
<form name="myForm">
    <textarea id="htmlInput"></textarea>
    <input type="button" value="submit" onclick="toPlainText(document.getElementById('htmlInput').value"/>
</form>
```

# 参考文档
js正则表达式语法
http://blog.csdn.net/zaifendou/article/details/5746988

正则表达式30分钟入门教程
http://demo.voidking.com/reprint/正则表达式/正则表达式30分钟入门教程.htm

常用的正则表达式
http://demo.voidking.com/reprint/正则表达式/常用的正则表达式.htm

正则表达式速查表
http://demo.voidking.com/reprint/正则表达式/正则表达式速查表.htm

正则表达式测试器
http://demo.voidking.com/reprint/正则表达式/正则表达式测试器.htm

正则表达式在线测试器
详情请见：http://demo.voidking.com/reprint/regexpal/

deerchao大侠正则表达式原文地址
http://www.jb51.net/tools/zhengze.html