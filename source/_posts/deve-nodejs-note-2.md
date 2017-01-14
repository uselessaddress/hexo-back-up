title: Node.js学习笔记（二）
date: 2014-09-27 13:30:27
tags: node.js
categories: 设计开发
toc: true
---
整理自：[慕课网](http://www.imooc.com/learn/75)
# 伪造模板数据跑通前后端交互流程
本篇博客用到的代码，小伙伴们直接复制粘贴就可以了，你们可知道小编一行行敲得多么辛苦T_T，复制粘贴前给个赞啊！
## 工程结构
```
helloworld/
  -bower_components/
  -node_modules/
  -views/
    -includes/
      -head.jade
      -header.jade
    -pages/
      -index.jade
      -detail.jade
      -admin.jade
      -list.jade
    -layout.jade
  -app.js
```
<!--more-->
## 操作步骤
我们在上次helloworld工程的基础上接着做！
1、在views文件夹下新建文件夹includes和pages
2、在views文件夹下，删除index.jade，新建layout.jade，输入内容：
```
doctype
html
	head
		meta(charset="utf-8")
		title #{title}
		include ./includes/head
	body
		include ./includes/header
		block content
```
3、在includes文件夹下，新建文件head.jade和header.jade，分别输入内容：
```
link(href="/bootstrap/dist/css/bootstrap.min.css",rel="stylesheet")
script(src="/jquery/dist/jquery.min.js")
script(src="/bootstrap/dist/js/bootstrap.min.js")
```
```
.container
	.row
		.page-header
			h1 #{title}
			small 天印电影
```
4、在pages文件夹下，新建文件index.jade、detail.jade、admin.jade和list.jade，分别输入内容：
```
extends ../layout

block content
	.container
		.row
			each item in movies
				.col-md-2
					.thumbnail
						a(href="/movie/#{item._id}")
							img(src="#{item.poster}",alt="#{item.title}")
						.caption
							h3 #{item.title}
							p: a.btn.btn-primary(href="/movie/#{item._id}",role="button")
								观看预告片
```
```
extends ../layout

block content
	.container
		.row
			.col-md-7
				embed(src="#{movie.flash}",allowFullScreen="true",quality="high",width="720",height="500",align="middle",type="application/x-shock")
			.col-md-5
				dl.dl-horizontal
					dt 电影名字
					dd= movie.title
					dt 导演
					dd= movie.doctor
					dt 国家
					dd= movie.country
					dt 语言
					dd= movie.language
					dt 上映年份
					dd= movie.year
					dt 简介
					dd= movie.summary
```
```
extends ../layout

block content
	.container
		.row
			form.form-horizontal(method="post",action="/admin/movie/new")
				.form-group
					label.col-sm-3.control-label(for="inputTitle") 电影名字
					.col-sm-10
						input#inputTitle.form-control(type="text",name="movie[title]",value="#{movie.title}")
				.form-group
					label.col-sm-3.control-label(for="inputDoctor") 导演
					.col-sm-10
						input#inputTitle.form-control(type="text",name="movie[doctor]",value="#{movie.doctor}")
				.form-group
					label.col-sm-3.control-label(for="inputCountry") 国家
					.col-sm-10
						input#inputTitle.form-control(type="text",name="movie[country]",value="#{movie.country}")
				.form-group
					label.col-sm-3.control-label(for="inputLanguage") 语言
					.col-sm-10
						input#inputTitle.form-control(type="text",name="movie[language]",value="#{movie.language}")
				.form-group
					label.col-sm-3.control-label(for="inputYear") 上映年份
					.col-sm-10
						input#inputTitle.form-control(type="text",name="movie[year]",value="#{movie.year}")
				.form-group
					label.col-sm-3.control-label(for="inputSummary") 简介
					.col-sm-10
						input#inputTitle.form-control(type="text",name="movie[summary]",value="#{movie.summary}")
						
				.form-group
					label.col-sm-3.control-label(for="inputPoster") 海报地址
					.col-sm-10
						input#inputTitle.form-control(type="text",name="movie[poster]",value="#{movie.poster}")
				.form-group
					label.col-sm-3.control-label(for="inputFlash") 片源地址
					.col-sm-10
						input#inputTitle.form-control(type="text",name="movie[flash]",value="#{movie.flash}")
						
						
				.form-group
					.col-sm-offset-2.col-sum-10
					button.btn.btn-default(type="submit") 录入
```
```
extends ../layout

block content
	.container
		.row
			table.table.table-hover.table-bordered
				thead
					tr
						th 电影名称
						th 导演
						th 国家
						th 上映年份
						//- th 录入时间
						th 查看
						th 修改
						th 删除
				tbody
					each item in movies
						tr(class="item-id-#{item._id}")
							td #{item.title}
							td #{item.doctor}
							td #{item.country}
							td #{item.year}
							//- td #{moment(item.meta.createdAt).format('MM/DD/YYYY')}
							td: a(target="_blank",href="../movie/#{item._id}") 查看
							td: a(target="_blank",href="../admin/update/#{item._id}") 修改
							td
								button.btn.btn-danger.del(type="button",data-id="#{item._id}") 删除
							
						
```
5、修改helloworld文件夹下app.js，主要是加入模拟数据，内容如下：
```
var express = require('express');
var path = require('path');
var port = process.env.PORT || 3000 ;
var app = express();

app.set('views','./views/pages');
app.set('view engine','jade');
app.use(express.bodyParser());
app.use(express.static(path.join(__dirname, 'bower_components')));
app.listen(port);

console.log('helloworld started on port ' + port);

app.get('/', function(req, res){
  res.render('index',{
  title:'首页',
  movies: [
	  {
		title: '机械战警',
		_id: 1,
		poster: 'http://ihelloworld.qiniudn.com/%40%2Fimgs%2FiNJIT.jpg',
	  },
	  {
		title: '机械战警',
		_id: 2,
		poster: 'http://ihelloworld.qiniudn.com/%40%2Fimgs%2FiNJIT.jpg',
	  }  
  ]
  })
})

app.get('/movie/:id', function(req, res){
  res.render('detail',{
  title:'详情页',
  movie: {
	doctor: '未知',
	country: '美国',
	title: '机械战警',
	year: '2014',
	poster: 'http://ihelloworld.qiniudn.com/%40%2Fimgs%2FiNJIT.jpg',
	language: '英语',
	flash: 'http://player.youku.com/player.php/sid/XNjM3Njc3MTY4/v.swf',
	summary: '无',
  }
  })
})

app.get('/admin/movie', function(req, res){
  res.render('admin',{
  title:'后台录入页',
  movie: {
	doctor: '',
	country: '',
	title: '',
	year: '',
	poster: '',
	language: '',
	flash: '',
	summary: '',
  }
  })
})

app.get('/admin/list', function(req, res){
  res.render('list',{
  title:'列表页',
  movies: [
	  {
		_id: 1,
		doctor: '未知',
		country: '美国',
		title: '机械战警',
		year: '2014',
		poster: 'http://ihelloworld.qiniudn.com/%40%2Fimgs%2FiNJIT.jpg',
		language: '英语',
		flash: 'http://player.youku.com/player.php/sid/XNjM3Njc3MTY4/v.swf',
		summary: '无',
	  },
	  {
		_id: 2,
		doctor: '未知',
		country: '美国',
		title: '机械战警',
		year: '2014',
		poster: 'http://ihelloworld.qiniudn.com/%40%2Fimgs%2FiNJIT.jpg',
		language: '英语',
		flash: 'http://player.youku.com/player.php/sid/XNjM3Njc3MTY4/v.swf',
		summary: '无',
	  }
  ]
  })
})

```
## 报错处理
至此，已经敲完了Scott大神给的代码，马上就能看到效果了，好激动！
哎呦我靠！报错，我改！又报错，我再改！还报错……诶？坏了，改不出来了！记录错误如下：
![bodyParser使用报错][1]
百度“nodejs使用app.use(express.bodyParser());出错”，得到如下结论：
1、node.js和windows的兼容性不如POSIX操作系统，因此npm提供给windows的第三方模块较少。
2、bodyParser以前是集成在express中的，现在需要单独安装。
无论哪个结论，解决办法都是安装body-parser：
```
npm install body-parser
```
然后在代码中如下使用：
> var bodyParser = require('body-parser');
> app.use(bodyParser.urlencoded({ extended: false }))
> app.use(bodyParser.json());

即把app.js的内容修改为如下内容：
```
var express = require('express');
var path = require('path');
var port = process.env.PORT || 3000 ;
var app = express();
var bodyParser = require('body-parser');

app.set('views','./views/pages');
app.set('view engine','jade');
app.use(bodyParser.urlencoded({ extended: false }))
app.use(bodyParser.json());
app.use(express.static(path.join(__dirname, 'bower_components')));
app.listen(port);

console.log('helloworld started on port ' + port);

app.get('/', function(req, res){
  res.render('index',{
  title:'首页',
  movies: [
	  {
		title: '机械战警',
		_id: 1,
		poster: 'http://ihelloworld.qiniudn.com/%40%2Fimgs%2FiNJIT.jpg',
	  },
	  {
		title: '机械战警',
		_id: 2,
		poster: 'http://ihelloworld.qiniudn.com/%40%2Fimgs%2FiNJIT.jpg',
	  }  
  ]
  })
})

app.get('/movie/:id', function(req, res){
  res.render('detail',{
  title:'详情页',
  movie: {
	doctor: '未知',
	country: '美国',
	title: '机械战警',
	year: '2014',
	poster: 'http://ihelloworld.qiniudn.com/%40%2Fimgs%2FiNJIT.jpg',
	language: '英语',
	flash: 'http://player.youku.com/player.php/sid/XNjM3Njc3MTY4/v.swf',
	summary: '无',
  }
  })
})

app.get('/admin/movie', function(req, res){
  res.render('admin',{
  title:'后台录入页',
  movie: {
	doctor: '',
	country: '',
	title: '',
	year: '',
	poster: '',
	language: '',
	flash: '',
	summary: '',
  }
  })
})

app.get('/admin/list', function(req, res){
  res.render('list',{
  title:'列表页',
  movies: [
	  {
		_id: 1,
		doctor: '未知',
		country: '美国',
		title: '机械战警',
		year: '2014',
		poster: 'http://ihelloworld.qiniudn.com/%40%2Fimgs%2FiNJIT.jpg',
		language: '英语',
		flash: 'http://player.youku.com/player.php/sid/XNjM3Njc3MTY4/v.swf',
		summary: '无',
	  },
	  {
		_id: 2,
		doctor: '未知',
		country: '美国',
		title: '机械战警',
		year: '2014',
		poster: 'http://ihelloworld.qiniudn.com/%40%2Fimgs%2FiNJIT.jpg',
		language: '英语',
		flash: 'http://player.youku.com/player.php/sid/XNjM3Njc3MTY4/v.swf',
		summary: '无',
	  }
  ]
  })
})
```
## 效果图
吼吼，终于搞定了！有图有真相！
![效果图1][2]
![效果图2][3]
![效果图3][4]
![效果图4][5]

## 结束
操作过程中的其他小错误，在此不作记录，有任何问题欢迎留言！



  [1]: http://ihelloworld.qiniudn.com/@/imgs/%E6%8A%A5%E9%94%99.jpg
  [2]: http://ihelloworld.qiniudn.com/@/imgs/%E6%95%88%E6%9E%9C%E5%9B%BE.jpg
  [3]: http://ihelloworld.qiniudn.com/@/imgs/%E6%95%88%E6%9E%9C%E5%9B%BE2.jpg
  [4]: http://ihelloworld.qiniudn.com/@/imgs/%E6%95%88%E6%9E%9C%E5%9B%BE3.jpg
  [5]: http://ihelloworld.qiniudn.com/%40%2Fimgs%2F%E6%95%88%E6%9E%9C%E5%9B%BE4.jpg