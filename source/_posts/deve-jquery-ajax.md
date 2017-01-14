title: jQuery学习笔记——Ajax篇
date: 2014-11-22 15:10:14
tags: [JavaScript,jQuery]
categories: 设计开发
toc: true
---

### 显示欢迎信息
Ajax的优点我们已经说过了，上个例子先！
```
<!--显示欢迎信息.html-->
<html>
	<head>
	<title>显示欢迎信息</title>
	<meta charset="UTF-8">
	<script type="text/javascript" src="jquery-1.11.1.js"></script>
	<script type="text/javascript">
		$(function(){
			$("#submit").click(function(){
				var name = $(".uname").val();
				var data = "uname=" + name;
				$.ajax({
					type:"POST",
					url:"http://demo.voidking.com/welcome.php",
					data:data,
					success: function(html){
						$("#message").html(html);
					}
				});
				return false;
			});
		});
	</script>
	</head>
	<body>
		<form>
			<label>Enter your name</label>
			<input type="text" name="uname" class="uname" /><br/>
			<input type="submit" id="submit"/>
		</form>
		<div id="message">
		</div>
	</body>
</html>
```

```
<!--welcome.php-->
<?php
	$name = $_POST['uname'];
	echo "welcome ".$name;
?>
```
把ajax实例01.html、welcome.php、jquery-1.11.1.js上传到自己的服务器。打开浏览器访问“显示欢迎信息.html”，输入一个名字，就能看到效果了。

为什么要上传服务器，搞的这么麻烦？大哥，ajax主要就是用来和服务器交互的好不好？如果你觉得麻烦，可以直接访问我的服务器查看效果:[VoidKing编程实例](http://demo.voidking.com)，接下来的例子也可以在这个网页找到。

当然，你也可以直接本地打开“显示欢迎信息.html”，但是提交数据没有反应，因为同源策略限制交互。怎么破？简单点，本地搭建一个php服务器！高大上点，自行百度，修改代码！
<!--more-->

### 执行认证
```
<!DOCTYPE html>
<html>
	<head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
        <title>ajax实例02</title>
        <script src="jquery-1.11.1.js" type="text/javascript"></script>  
        <script type="text/javascript">
			$(function() {   
				$('#submit').click(function () {           
					var name = $('.uname').val();		
					var pwd = $('.passwd').val();	
					var data='uname='+name+'&password='+pwd;
					$.ajax({
						type:"GET",
						url:"logincheck.php", 
						data:data,             
						success:function(html) {			
							$("#message").html(html);		
						}
					});		
					return false;	
				});
			});
		</script>
	</head>
	<body>
		<p>用户名输入guest，密码输入jquery，会出现欢迎信息，否则显示未注册</p><br/>
		<form>  
		<label>Enter your Name</label>  
		<input type="text"  name="uname" class="uname"/>  <br/>
		<label>Enter your Password</label>  
		<input type="password"  name="password" class="passwd"/>  <br/>
		<input type="submit" id="submit"/>  
		</form>  
		<div id="message"></div>
	</body>
</html>
```
```
<!--logincheck.php-->
<?php   
$name = trim($_GET['uname']); 
$pswd = trim($_GET['password']);    
if(($name=="guest") && ($pswd=="jquery"))
  echo "Welcome ".  $name;
else
  echo "Sorry you are not authorized";
?>  
```

### 验证用户名
```
<html>
	<head>
		<meta charset="UTF-8">
		<title>验证用户名</title>
		<script type="text/javascript" src="jquery-1.11.1.js"></script>
		<script type="text/javascript">
			$(function() {   
				$('.error').hide();
				$('#submit').click(function () {           
					var name = $('.uname').val();		
					var data='uname='+name;
					$.ajax({
						type:"POST",
						url:"validateuser.php", 
						data:data,             
						success:function(html) {
							$('.error').show();		
							$('.error').text(html);				
						}
					});		
					return false;	
				});
			});
		</script>
		</head>
	<body>
		<p>验证用户名，只能是数字、字母和下划线</p><br/>
		<form>  
			<span class="label">Enter your Name</span>  
			<input type="text"  name="uname" class="uname"/>  <span class="error"> </span><br>
			<input type="submit" id="submit"/>  
		</form>  
	</body>
</html>
```
```
<!--validateuser.php-->
<?php   
$name = $_POST['uname']; 
if (!eregi("^[a-zA-Z0-9_]+$", $name)) 
{
	echo "Invalid User name!";
}
else
{
	echo "Great user name!";
}
?>  
```

### 使用自动完成
```
<html>
	<head>
		<meta charset="UTF-8">
		<title>使用自动完成</title>
		
		<style type="text/css">
			.listbox {
				position: relative;
				left: 10px;
				margin: 10px;
				width: 200px;
				background-color: #000;
				color: #fff;
				border: 2px solid #000;
			}

			.nameslist {
				margin: 0px;
				padding: 0px;
			list-style:none;
			}

			.hover {
				background-color: cyan;
				color: blue;
			}
		</style>
		
		<script type="text/javascript" src="jquery-1.11.1.js"></script>
		<script type="text/javascript">
			$(function() {  
				$('.listbox').hide(); 
				$('.userid').keyup(function () {       
					var uid = $('.userid').val();	
					var data='userid='+uid;
					$.ajax({
						type:"POST",
						url:"autocomplete.php", 
						data:data,             
						success:function(html) {
							$('.listbox').show();
							$('.nameslist').html(html);
							$('li').hover(function(){
								$(this).addClass('hover');
							},
							function(){
								$(this).removeClass('hover');
							});
							$('li').click(function(){
								$('.userid').val($(this).text());
								$('.listbox').hide(); 
							});
						}
					});		
				return false;	
				});
			});
		</script>
		</head>
	<body>
		<p>输入名字的第一个字符时，弹出建议框。</p><br/>
		<form>  
			<span class="label">Enter user id</span>  
			<input type="text"  name="userid" class="userid"/> 
			<div class="listbox">
				<div class="nameslist">
				</div>
			</div>
		</form>   
	</body>
</html>
```

```
<?php   
	$name = $_POST['userid']; 
	$connect = mysql_connect("localhost", "demo", "voidking") or die("Please, check your server connection.");
	mysql_select_db("demo");
	$query = "SELECT name from user where name like '$name%'";
	$results = mysql_query($query) or die(mysql_error());
	if($results)
	{
		while ($row = mysql_fetch_array($results)) {
			extract($row);
			echo '<li>' . $name. '</li>';
		}
	}
?>  
```

### 导入HTML
```
<!--导入HTML-->
<html>
	<head>
		<meta charset="UTF-8">
		<title>导入HTML</title>
		<script type="text/javascript" src="jquery-1.11.1.js"></script>
		<script type="text/javascript">
			$(function() {   
				$('.list').click(function () {           
					$('#message').load('namesinfo.html');
					return false;   
				});
			});
		</script>
		</head>
	<body>
		<p>单击超链接，从另一个文件中导入一些HTML内容到当前网页中。</p><br/>
		<p>We are going to organize the Conference on IT on 2nd Feb 2010</p>
		<a href="http://demo.voidking.com" class="list">Participants</a>
		<div id="message"></div>
	</body>
</html>
```

```
<!--namesinfo.html-->
<p>The list of the persons taking part in conference </p>
<ul>
<li>Jackub</li>
<li>Jenny</li>
<li>Jill</li>
<li>John</li>
</ul>
<p>We wish them All the Best</p>
```

### 取得JSON数据
```
<!--取得JSON数据-->
<html>
	<head>
		<meta charset="UTF-8">
		<title>取得JSON数据</title>
		<script type="text/javascript" src="jquery-1.11.1.js"></script>
		<script type="text/javascript">
			$(function() {   
				$('#submit').click(function () {      
					$.ajax({
						type:"GET",
						url:"drinkinfo.json",    
						dataType:"json",
						success: function (data) {   
							var drinks="<ul>";
							$.each(data, function(i,n){
								drinks+="<li>"+n["optiontext"]+"</li>";
							});
							drinks+="</ul>";              
							$('#message').append(drinks);	
						}
					});   
					return false;	
				});		
			});
		</script>
		</head>
	<body>
		<p>从JSON文件中异步地导入信息到当前的网页中。</p><br/>
		<p>For information from JSON file click the button given below :<br>
		<input type="submit" id="submit"/>  
		<div id="message"></div>  
	</body>
</html>
```
drinkinfo.json文件内容如下：
```
[
	{"optiontext" : "Tea", "optionvalue" : "Tea"},
	{"optiontext" : "Coffee", "optionvalue" : "Coffee"},
	{"optiontext" : "Juice", "optionvalue" : "Juice"}
]
```
矮油我靠，在服务器上打开浏览器，读取正常；在本地打开浏览器，居然无法读取，404错误！

经过测试，只要把文件名更改一下就好了。即把drinkinfo.json改成drinkinfo.txt。真是个奇葩问题，个人猜测是浏览器传输文件有格式限制。

### 取得XML数据
```
<html>
	<head>
		<meta charset="UTF-8">
		<title>取得XML数据</title>
		<script type="text/javascript" src="jquery-1.11.1.js"></script>
		<script type="text/javascript">
			$(document).ready(function() {   
				$('#submit').click(function () {      
					$.ajax({
						type:"GET",
						url:"student.xml",    
						dataType:"xml",
						success: function (sturec) {    
							var stud="<ul>";
							$(sturec).find('student').each(function(){
								var name = $(this).find('first-name').text()
								stud+="<li>"+name+"</li>";
							});
							stud+="</ul>"; 
							$('#message').append(stud);
						}
					});   
					return false;	
				});		
			});
		</script>
		</head>
	<body>
		<p>从student.xml文件中异步导入信息到当前的网页中。</p><br/>
		<p>To see the Names of the students extracted from XML file click the button given below :</p>
		<input type="submit" id="submit"/>  
		<div id="message"></div>
	</body>
</html>
```
student.xml文件内容如下：
```
<?xml version="1.0" encoding="utf-8" ?>
<school>
  <student>
    <roll>101</roll>
    <name>
      <first-name>Anil</first-name>
      <last-name>Sharma</last-name>
    </name>
    <address>
      <street>
        22/10 Sri Nagar Road
      </street>
      <city>
        Ajmer
      </city>
      <state>
        Rajasthan
      </state>
    </address>
    <marks>
      85
    </marks>
   </student>
    <student>
    <roll>102</roll>
    <name>
      <first-name>Manoj</first-name>
      <last-name>Arora</last-name>
    </name>
    <address>
      <street>
        H.No 11-B Alwar Gate
      </street>
      <city>
        Ajmer
      </city>
      <state>
        Rajasthan
      </state>
    </address>
    <marks>
      92
    </marks>
  </student>
</school>
```

### 结束语
以上例子，全部摘自《jQuery攻略》，不详解，看代码！

### 参考文档
李炎恢的jQuery视频教程
《jQuery攻略（作者B.M.Harwani）》
《jQuery实战（作者Bear Bibeault、Yehuda Katz）》
《jQuery高级编程（作者 Cesar Otero、Rob Larsen）》
《jQuery Javascript 与CSS开发入门经典（作者Richard York）》