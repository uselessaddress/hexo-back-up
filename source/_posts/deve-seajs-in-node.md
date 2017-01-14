title: 在node项目中使用Sea.js
toc: true
date: 2016-07-15 20:48:08
tags:
- 前端
- seajs
- node
categories: 设计开发
---
# 前言
Sea.js 追求简单、自然的代码书写和组织方式，具有以下核心特性：

- 简单友好的模块定义规范：Sea.js遵循CMD规范，可以像Node.js一般书写模块代码。
- 自然直观的代码组织方式：依赖的自动加载、配置的简洁清晰，可以让我们更多地享受编码的乐趣。

Sea.js还提供常用插件，非常有助于开发调试和性能优化，并具有丰富的可扩展接口。

<!--more-->

# 配置
## 下载seajs
seajs官方下载：http://seajs.org/docs/#downloads

## 项目结构
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/seajs/nodebase.jpg)

## 概念说明
下面明确几个概念，方便接下来的描述：
1、`/`（网站根目录），是项目目录`nodebase`。
2、虚拟路径，是node把某个文件（夹）转化之后，该文件（夹）的相对于`/`的路径。
3、访问路径，是通过浏览器url访问的路径。

> 虚拟路径不一定是访问路径，访问路径不一定是虚拟路径。因为，只有我们指定的资源、指定的路径是开放给用户访问的。

## 静态资源配置
```
// 把文件夹中的内容添加到网站根目录下，静态文件务必添加，否则访问不到
app.use(express.static(path.join(__dirname, 'bower_components')));
app.use(express.static(path.join(__dirname, 'public')));
```
在node项目的入口文件app.js中，加入如上配置，那么，bower_components、public两个文件夹的虚拟路径都变成了`/`，访问路径都变成了`/`。

```
// 静态文件目录
var staticDir = path.join(__dirname, 'public');
// 虚拟目录
app.use('/dist', express.static(staticDir));
app.use('/public', express.static(staticDir));
```
在node项目的入口文件app.js中，加入如上配置，那么，public文件夹的虚拟路径就变成了`/dist`和`/public`，访问路径就变成了`/dist`和`/public`。

通过上面两个配置，可以看出，静态资源的虚拟路径和访问路径是相同的。

## sea-config.js
```
seajs.config({
    //base: '../',
    base: window.host + '/',
    alias: {
        'jquery': 'seajs/jquery/jquery.min.js',
        'layer': 'seajs/layer/layer.js',
        'swiper': 'Swiper/dist/js/swiper.min.js'       
    }
});
```
如果base的值使用相对路径`../`，那么页面引入依赖的js文件的时候，就是相对于页面文件的虚拟路径。

```
app.set('views','./views');// 页面目录配置
```
举个例子，在app.js中使用了页面目录配置，views文件夹虚拟路径变成了`/`。注意，views文件夹依然没有访问路径。

views文件夹的目录结构如下：
```
views
|-weixin
| |-home.html
|-index.html
```

在路由转发中，index.html和home.html的虚拟路径分别如下：
```
res.render('index',{
    title: 'index'
});
res.render('weixin/home',{
    title: 'home'
});
```
在index.html中引入sea-config.js，在加载依赖的js文件的时候，会在index.html的基础上向上一层寻找。因为index.html所在的虚拟路径为`/`，没有上一层，所以肯定找不到js文件。

而home.html所在的虚拟路径为`/weixin/`，向上一层寻找，是虚拟路径`/`。而静态文件的虚拟路径都是`/`，刚好可以找到需要的js文件。

> 鉴于相对路径的差异问题，base最好使用绝对路径，配置在全局config.js中。

## home.js
```
seajs.use(['jquery','layer'],function($,layer){
    var index = {
        init:function(){
            this.saveData();
            this.bindEvent();
        },
        saveData: function(){
            if(window.localStorage){
                var x = window.localStorage.userInfo? JSON.parse(window.localStorage.userInfo):{};
                if((x.unionid == undefined) || (x.unionid == "undefined")){
                    var temp = {
                        unionid: $('#unionid').val(),
                        openid: $('#openid').val(),
                        nickname: $('#nickname').val(),
                        headimgurl:$ ('#headimgurl').val()
                    };
                    window.localStorage.userInfo = JSON.stringify(temp);
                }
            }
        },
        bindEvent:function(){
            layer.alert('layer');
        }
    }
    index.init();
});
```

## 引入js文件
```
<script>
    window.host = '<%= host%>';
</script>
<script src="/seajs/seajs-1.3.1/dist/sea.js"></script>
<script src="/seajs/sea-config.js"></script>
<script src="/js/weixin/home.js"></script>
```

# CMD模块化
## 不封装
因为seajs遵循CMD规范，所以遵循CMD规范的插件，比如swiper，引入后就可以使用。

## jquery封装
jquery遵循AMD规范，需要封装一下，使其符合CMD规范才可以使用，否则会报错。
```
define(function(require, exports, module) {
    // 模块化jquery源码
});
```

## 依赖jquery的插件封装
有些插件，是在jquery的基础上的开发的，需要先引入jquery。
```
define(function(require, exports, module) {
    var $ = require('jquery');
    // 依赖jquery的模块化代码
});
```

另外一种方法：
```
define(function(require, exports, module) {
    return function($){
        // 依赖jquery的模块化代码
    } 
});
```
这种方法在使用模块前需要先传入jquery进行初始化。


## 普通插件封装
如果是未模块化的插件（普通js代码），需要暴露对应的接口。
```
define(function(require, exports, module) {
    var $ = require('jquery');
    var voidking = {
        init: function(){
            // 未模块化代码
            var height = $(document).height();
            console.log(height);
        }
    };
    module.exports = voidking;
});
```
在使用模块前需要调用init方法初始化。

# layer的坑
## 不封装
不对layer.js进行封装，直接引入时，会报错`jQuery is not defined`。
需要说明的是，在使用layer-mobile.js的时候，不需要封装就可以正常使用。

## 封装
由上面的报错，可以看出，layer.js需要依赖jquery，那就封装一下，引入jquery吧！
```
define(function(require, exports, module) {
    var jQuery = require('jquery');
    // layer.js代码
});
```
然后，错误就变成了`layer.alert is not a function`。
在控制台输入`layer`，可以看到layer是一个对象。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/seajs/layer-bug.jpg)
啊嘞，layer对象明明包含了一个alert方法！！！为啥不能调用捏？

不甘心的小编，又尝试了很多其他的封装方法，依然无法使用。

## 不完美解决办法
layer的作者贤心说，layer是CMD规范的。那么，理论上直接引入就可以使用。但是，错误当道，`jQuery is not defined`。那么问题的关键，就是给layer.js引入jquery。

使用封装的办法，引入jquery，经过测试，行不通。

如果直接把jquery源码粘贴到layer.js文件中，是否可以呢？经过测试，确实可以！！！

好吧，这个坑总算是填上了。虽然不够完美，但是确实是个有效的方法！如果有更好的解决的办法，希望能留言告诉我，谢谢。

# seajs模块加载顺序
1、从seajs.use方法入口，开始加载use到的模块。
2、use到的模块这时mod缓存当中一定是不存在的，seajs创建一个新的mod，赋予一些初始的状态。
3、执行mod.load方法。
4、一堆逻辑之后走到seajs.request方法，请求模块文件。模块加载完成之后，执行define方法。
5、define方法分析提取模块的依赖模块，保存起来，缓存factory但不执行。
6、模块的依赖模块再被加载，如果继续有依赖模块，则继续加载，直至所有被依赖的模块都加载完毕。
7、所有的模块加载完毕之后，执行use方法的callback。
8、模块内部逻辑从callback开始执行，require方法在这个过程当中才被执行。
PS: define方法纯粹只是分析模块、存储模块，并没有执行模块。require方法就是根据id在define定义存储的模块缓存中找到相应的模块，并执行它，获得模块定义返回的方法。

# 后记
也许，参加Sea.js的社区维护也是一件很好玩的事情。又多了很多许多需要学习的东西，路漫漫其修远兮！

# 书签
Seajs简易文档
http://yslove.net/seajs/

快速上手seajs——简单易用Seajs
http://www.tuicool.com/articles/3uIZzy

seajs base配置
http://www.tuicool.com/articles/3Eb6Fj

jQuery 插件的模块化
https://lifesinger.wordpress.com/2011/05/18/jquery-plugins-modulization/

seajs 加载Jquery的遇到问题?
https://www.zhihu.com/question/21703739

如何改造现有文件为 CMD 模块
https://github.com/seajs/seajs/issues/971

CMD 模块定义规范
https://github.com/seajs/seajs/issues/242

社区
https://github.com/seajs/seajs/issues/271

前端模块化开发那点历史
https://github.com/seajs/seajs/issues/588

如何参与开发
https://github.com/seajs/seajs/issues/276

Develop A Package
http://spmjs.io/documentation/develop-a-package

高富帅seajs使用示例及spm合并压缩工具露脸
http://www.zhangxinxu.com/wordpress/2012/07/seajs-node-nodejs-spm-npm/

Hello Sea.js
http://island205.github.io/HelloSea.js/index.html

从零开始编写自己的JavaScript框架（一）
http://www.ituring.com.cn/article/48461

Javascript模块化编程（一）：模块的写法
http://www.ruanyifeng.com/blog/2012/10/javascript_module.html

JavaSript模块规范 - AMD规范与CMD规范介绍
http://blog.chinaunix.net/uid-26672038-id-4112229.html

该如何理解AMD ，CMD，CommonJS规范
http://www.cnblogs.com/qianshui/p/5216580.html?utm_source=tuicool&utm_medium=referral
http://www.tuicool.com/articles/MVrMBrI

(function($){...})(jQuery)是什么意思
http://blog.csdn.net/rambo_china/article/details/7742321

深入探寻seajs的模块化与加载方式
http://www.jb51.net/article/64024.htm

seajs模块加载机制
http://www.jianshu.com/p/1245af09383e

SeaJS中jQuery插件模块化及其调用方式
http://my.oschina.net/briviowang/blog/208587

seajs模块化jQuery与jQuery插件
http://julabs.com/blog/seajs-jquery-and-plugins/

Sea.js 手册与文档
http://www.zhangxinxu.com/sp/seajs/docs/zh-cn/module-definition.html