title: 团队博客项目（二）
date: 2014-10-22 22:02:02
tags: [node.js,项目]
categories: 设计开发
toc: true
---
## hello voidking
### 查看效果
1、单击webstorm右上角的绿色三角形（或者shift+f10），运行项目。

2、打开浏览器，输入localhost:3000，有没有看到welcome to express ？

### 修改文字
下面我们把welcome to express修改为hello voidking！
1、打开views文件夹下的index.ejs，诶？这不是html代码吗？发现有个<%= title%>，这是个啥玩意？
这时，我们就要解释一下模板引擎了。专业一点说，模板引擎是一个可以根据模板生成html代码的工具。通俗一点讲，模板引擎就像是一个函数，不同的x值对应不同的y值。
比如y=x+1，当x=1时y=2。这里的x就相当于<%= title%>，y就相当于html页面。懂了？不懂拉倒，自己慢慢想，这不是重点。
2、知道了原理，修改就简单了，不就是给x赋值嘛！打开routes文件夹下的index.js文件，看到这段代码：
```
router.get('/', function(req, res) {
  res.render('index', { title: 'Express' });
});
```
修改如下：
```
router.get('/', function(req, res) {
  res.render('index', { title: 'voidking' });
});
```
看懂了吧，把“voidking”赋值给了title，仅此而已。
<!--more-->
3、运行，看到效果没？“welcome to voidking”，怎么改成“hello voidking”？当然是修改index.ejs文件了！“welcome to” 就相当于y=x+1中的1，是固定的。

### 一点解释
也许你已经运行成功了，但是你还是不理解这些代码。下面我简单解释下，不懂没关系，我会在接下来的教程中详细解释。
1、bin文件夹，不用管。

2、node_modules文件夹，存放包。包又是啥玩意？类比吧，`node.js中的包（文件夹） = java中的包（jar文件）`。

3、public文件夹，存放图片等资源文件。

4、routes文件夹，存放路由文件，也就是`业务层`处理的文件。

5、views文件夹，存放`交互层`文件，这次项目中，也就是ejs模板文件。

6、app.js文件，程序入口。不要费心去找“main”函数了，它就是！

7、package.json文件，包的配置文件，相当于java的maven工程中的pom.xml文件。
也许咱们这个项目用了很多第三方的包，那么，别人要运行这个项目，必须也安装了这些包。所以，package.json文件的主要目的，就是记录依赖的第三方包。这样，代码是不是很容易迁移？没错！
具体写法这里不展开了，下次专门说一下。

8、细心的朋友可能发现了，业务层和交互层都有了，那么数据层在哪？别着急，马上我们会新建一个models文件夹，存放数据层文件。

表达的不好，请见谅。推荐一本好书——《node.js开发指南》，byvoid写的，这次项目我主要参考的就是这本书。不懂的地方，看书吧！






