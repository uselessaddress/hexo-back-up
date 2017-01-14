title: Node.js学习笔记（一）
date: 2014-09-25 20:30:31
tags: node.js
categories: 设计开发
toc: true
---

整理自：[慕课网](http://www.imooc.com/learn/75)
# helloworld工程的搭建

## 技术及工具
> **node.js + mongodb(工具mongoose) + express + jade + moment.js + npm + jquery + bootstrap + grunt**

我靠，那么多！淡定，暂时，咱们这个工程只要 node.js + express + jade 。
## 准备工作
下载安装[node.js](http://nodejs.org/)。
## 工程结构
```
helloworld/
  -node_modules/
  -bower_components/
  -views/
    -index.jade
  -app.js
```
上两张图，直观感受下：
![工程结构][1]
![工程结构][2]
<!--more-->
## 具体操作
### 新建helloworld文件夹
在F盘下新建个文件夹叫做“ helloworld ”，然后打开命令提示符，cd命令进入F盘下的helloworld文件夹。
### 执行以下命令：
```
npm install express
npm install jade
npm install mongodb
npm install bower -g
bower install bootstrap
```
### 查看文件夹
看看helloworld文件夹，诶？结构和你给的不一样啊！那就对了，咱接着搞！

### 新建文件
在helloworld文件夹下，新建文件**app.js**，输入内容如下：
```
var express = require('express');
var port = process.env.PORT || 3000 ;
var app = express();

app.set('views','./views');
app.set('view engine','jade');
app.listen(port);

console.log('helloworld started on port ' + port);

app.get('/', function(req, res){
  res.render('index',{title:'helloworld'});
});
```
至于这段代码的意思，请自行观看视频哈！
在views文件夹下，新建文件**index.jade**（这个文件就相当于html文件），输入内容如下：
```
doctype
html
	head
		meta(charset="utf-8")
		title #{title}
	body
		h1 #{title}
```
至此，大功告成！
### 查看效果
在helloworld文件夹下执行命令：
```
node app.js
```
在浏览器中打开：http://localhost:3000
有没有看到“helloworld”这个出现在了浏览器上？没看到？请留言！
PS：
你也可以新建admin.jade，也输入和index.jade相同的代码。然后在app.js添加如下代码：
```
app.get('/admin/', function(req, res){
  res.render('admin',{title:'admin'});
});
```
访问地址：http://localhost:3000/admin/
这时你会看到“admin”出现在了浏览器上！

## 结束
好了，今天的笔记就写到这里！有任何问题欢迎留言。
  [1]: http://ihelloworld.qiniudn.com/@/imgs/nodejs%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84.jpg
  [2]: http://ihelloworld.qiniudn.com/@/imgs/nodejs%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84%E5%88%86%E6%94%AF.jpg
