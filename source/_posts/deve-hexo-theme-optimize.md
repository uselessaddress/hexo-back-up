title: hexo主题优化
date: 2015-05-31 16:22:12
tags:
- hexo
- 优化
categories: 设计开发
toc: true
---

### 前言
大概一年前开始使用hexo，主题一直使用jacman。后来Hexo更新到3.0，主题也跟着更新，但是，懒惰的小编并没有参与这一更新活动。
如今，安装了hexo3.0，问题来了——主题不匹配。当年千辛万苦优化过的主题，又要重新搞起！

### 选择主题
啊嘞，几个月不见，hexo已经如此牛逼！不仅官网更加高端大气上档次，而且主题数量翻了几翻！
https://github.com/hexojs/hexo/wiki/Themes ，可以看到，群众的力量是巨大的！
换个主题，换个心情，这次我选择了yilia，简洁大气！

### 添加“关于”
`hexo new page "about"`，
编辑hexo/source/about/index.md，
编辑hexo/themes/yilia/_config.yml，添加如下：
```
menu:
  关于: /about
```
<!--more-->
### 添加RSS
`npm install hexo-generator-feed --sava`，

注意，后面的参数`--save`绝对不能省，否则该插件信息不会写入package.json。
`hexo clean`，`hexo g`，查看public文件夹，可以看到atom.xml文件。

### 添加sitemap
`npm install hexo-generator-sitemap --save`。
`hexo clean`，`hexo g`，查看public文件夹，可以看到sitemap.xml文件。
sitemap的初衷是给搜索引擎看的，为了提高搜索引擎对自己站点的收录效果，我们最好手动到google和百度等搜索引擎提交sitemap.xml。

### 添加多说
http://duoshuo.com/ ，注册一个账号。登录，添加新站点voidking。
在工具，获取代码界面，可以看到通用代码：
```
<!-- 多说评论框 start -->
	<div class="ds-thread" data-thread-key="请将此处替换成文章在你的站点中的ID" data-title="请替换成文章的标题" data-url="请替换成文章的网址"></div>
<!-- 多说评论框 end -->
<!-- 多说公共JS代码 start (一个网页只需插入一次) -->
<script type="text/javascript">
var duoshuoQuery = {short_name:"voidking"};
	(function() {
		var ds = document.createElement('script');
		ds.type = 'text/javascript';ds.async = true;
		ds.src = (document.location.protocol == 'https:' ? 'https:' : 'http:') + '//static.duoshuo.com/embed.js';
		ds.charset = 'UTF-8';
		(document.getElementsByTagName('head')[0] 
		 || document.getElementsByTagName('body')[0]).appendChild(ds);
	})();
	</script>
<!-- 多说公共JS代码 end -->

```

`var duoshuoQuery = {short_name:"voidking"};`中的voidking，就是我们要用到的duoshuo-key，也有人称之为duoshuo_shortname。
打开hexo/themes/yilia/layout/_partial/post/duoshuo.ejs，可以看到：
```
<div class="duoshuo">
	<!-- 多说评论框 start -->
	<div class="ds-thread" data-thread-key="<%=key%>" data-title="<%=title%>" data-url="<%=url%>"></div>
	<!-- 多说评论框 end -->
	<!-- 多说公共JS代码 start (一个网页只需插入一次) -->
	<script type="text/javascript">
	var duoshuoQuery = {short_name:"<%=theme.duoshuo%>"};
	(function() {
		var ds = document.createElement('script');
		ds.type = 'text/javascript';ds.async = true;
		ds.src = (document.location.protocol == 'https:' ? 'https:' : 'http:') + '//static.duoshuo.com/embed.js';
		ds.charset = 'UTF-8';
		(document.getElementsByTagName('head')[0] 
		 || document.getElementsByTagName('body')[0]).appendChild(ds);
	})();
	</script>
	<!-- 多说公共JS代码 end -->
</div>

```
我们需要填入的，是`<%=theme.duoshuo%>`的值，那么，在哪里填入呢？在hexo/themes/yilia/_config.yml文件中。
是这样的，我们可以认为，_config.yml就是`theme`，而在_config.yml中写入`duoshuo: voidking`，就是给`duoshuo`赋值为`voidking`。

当然，你也可以直接在duoshuo.ejs中把`<%=theme.duoshuo%>`替换为`voidking`。但是不建议这么做，因为这样破坏了作者可复用性设计。

### 添加头像
在hexo/themes/yilia/source/img中添加一个图片headportrait.jpg，然后编辑hexo/themes/yilia/_config.yml：
```
#你的头像url
avatar: "img/headportrait.jpg"
```

### 添加favicon
使用Axialis IconWorkshop，制作一个16X16的favicon.png。放置到hexo/themes/yilia/source中，然后编辑hexo/themes/yilia/_config.yml：

```
favicon: /favicon.png
```

添加好之后，本地测试，只有主页会显示favicon，其他页面不显示，不知道是什么原因。

找了很久没找到解决办法，狠了狠心，直接上传。惊奇地发现，上传到gitcafe之后，所有页面都可以显示favicon。

### archieves数量
要修改hexo主题，多少要懂得一点hexo的结构和原理。
全局配置文件hexo/_config.yml中有这么一段：

```
per_page: 5
pagination_dir: page

```

这里的per_page，同时设置index、archive、tag、categoriy。如果想要单独设置，可以这么写：

```
index_generator:
  per_page: 5

archive_generator:
  per_page: 200
  yearly: true  
  monthly: true 

tag_generator:
  per_page: 10 

category_generator: 
  per_page: 10 
```

### Categories
Litten大哥，你这不要Categories是什么节奏？修改起来，非常不容易哇！
需要修改的文件有`hexo/themes/yilia/source/js/main.js`，`hexo/themes/yilia/source/css/_partial/main.styl`，`hexo/themes/yilia/layout/_partial/left-col.ejs`。也许还有别的文件，修改了一下午，以失败告终。。。
总之，还是能力不够，看来得找时间系统学习一下hexo主题的制作了。。。先放着吧！

### 添加文章目录
TOC (Table of contents) ，中文翻译做目录。之前用jacman，感觉目录很方便，可以很快定位文章内容，尤其对于长文章来说。但是，yilia貌似没有集成，那就自己添加吧！查找资料，居然找到了好友twiceYuan的写的教程，世界真小哇！
详情请访问参考文档，摘录twiceYuan的教程如下：

#### 修改article.ejs
打开`hexo/themes/yilia/layout/_partial/article.ejs`，在`<%- post.content %>`前面插入：
```
<% if(post.toc !== false){ %>
	<div id="toc" class="toc-article">
	  <strong class="toc-title">文章目录</strong>
	  <%- toc(post.content) %>
	</div>
<%}%>
```

这里加了一个`post.toc`的判断，也就是说，只有在启用toc的post会出现目录，而不是每篇文章都出现。

#### 修改article.yml
打开`hexo/themes/yilia/layout/source/css/_partial/article.yml`，在最后插入：
```
/*toc*/
.toc-article
  background #eeeeee
  margin 2em 0 0 0.2em
  padding 1em
  border-radius 5px
  .toc-title
    font-size 120%
  strong
    padding 0.3em 1
ol.toc
  width 100%
  margin 1em 2em 0 0
#toc
  line-height 1em
  font-size 0.8em
  float right
  .toc
    padding 0 
    li
      list-style-type none
  .toc-child 
    padding-left 0em
#toc.toc-aside
  display none
  width 13%
  position fixed
  right 2%
  top 320px
  overflow hidden
  line-height 1.5em
  font-size 1em
  color color-heading
  opacity .6
  transition opacity 1s ease-out
  strong
    padding 0.3em 0
    color color-font
  &:hover
    transition opacity .3s ease-out
    opacity 1
  a
    transition color 1s ease-out
    &:hover
      color color-theme
      transition color .3s ease-out

```

#### 使用
编辑.md文档，如果要使用文章目录，就要文章开头添加toc属性并且设置为true。
```
title: hexo主题优化
date: 2015-05-31 16:22:12
tags:
categories: 设计开发
toc: true
```

#### 新的问题
小伙伴很靠谱，这个方法可用！但是，新的问题来了，文章目录特别紧凑，不够人性化，应该是因为主题差别。于是，参考格式良好的css文件，改写了article.styl的`/*toc*/`部分：
```
/*toc*/
.toc-article{
  background #eee
  margin 0 0 0 0.2em
  padding 1em
  border-radius 5px
  -webkit-border-radius 5px
  strong{
	padding 0.3em 0
  }
}  
#toc{
  line-height 1.5em
  font-size 1em
  float right
  ol{
	margin-left: 0
  }	
  .toc{
	padding 0
	li{
	  list-style-type none
	}  
  }    
  .toc-child{
	padding-left 1.5em
  }
} 
```
问题完美解决，看来，不同的theme，添加TOC的时候，确实需要个性定制。

#### 奇怪现象
添加目录后，出现了一个奇怪的现象：如果一篇文章在主页展示全部内容，也就是没有添加`<!--more-->`，那么，该文章就会自动生成目录，无论文章中有没有添加toc属性。
且不去管它，等到技术更加牛逼了再来处理。……等一下，貌似有了灵感，修改article.ejs如下：

```
<% if(post.toc && post.toc !== false){ %>
	<div id="toc" class="toc-article">
	  <strong class="toc-title">文章目录</strong>
	  <%- toc(post.content) %>
	</div>
<%}%>

```


修改完成，执行命令：

`hexo clean`，`hexo g`，`hexo s`。

查看效果，主页目录没有了，Perfect！

### 书写代码块
一直以来，写博客的时候，如果用到代码块，都是用三个反引号包围。现在发现，似乎出现了问题。如果连续两个代码块，那么，经常出现一种情况：判断代码块不准确，两个代码块被当成了一个代码块。
而如果代码块使用缩进一个TAB的方法，那么，代码块前面就会没有行号。
。。。麻烦的问题，且不去管它。。。

### 文章模板
打开hexo/scaffolds/post.md，可以看到：
```
title: {{ title }}
date: {{ date }}
tags:
---
```
如果我们想要在每次生成.md文件的时候，自动生成categories和toc，可以修改如下：
```
title: {{ title }}
date: {{ date }}
tags:
categories: 
toc: true
---
```
### 参考文档
hexo博客的优化技巧续
http://zipperary.com/2013/06/02/hexo-guide-5/

-------

如何在谷歌站长工具提交网站地图sitemap.xml
http://jingyan.baidu.com/article/066074d669a958c3c31cb076.html

------
Hexo 3.0 静态博客使用指南
http://segmentfault.com/a/1190000002592993

------
hexo你的博客
http://ibruce.info/2013/11/22/hexo-your-blog/

------
twiceYuan：[为Hexo添加文章目录](http://twiceyuan.com/2015/01/12/%E4%B8%BAHexo%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0%E6%96%87%E7%AB%A0%E7%9B%AE%E5%BD%95/)

------