title: XMPP学习笔记——Hello
date: 2014-11-27 13:19:49
tags: [jQuery,XMPP]
categories: 设计开发
toc: ture
---


### 前言
首先声明，我们将要写的这个小程序，是属于XMPP客户端的。以后要写的程序，也都是XMPP客户端的。

马上开始第一个XMPP程序了，真有点小激动呢！接下来，各位同学和void一起试试手吧！

### 准备
编程之前，我们需要准备好环境和依赖的工具。

这个程序，我们需要Tomcat服务器、jquery.js、jquery-ui.js、jquery-ui.css、jquery-ui.theme.css、strophe.js、strophe.flxhr.js、flXHR.js以及依赖的文件。哦，还有一个XMPP账号。

下面简单介绍一下环境的准备和依赖工具的下载，最后提供打包好的工具。
<!--more-->
#### 本地服务器

因为Hello程序是一个仅由HTML、CSS和JavaScript代码组成的Web应用程序，所以运行和测试它是非常简单的。

尽管flXHR库可用来执行跨域请求而无需任何服务器设置，但它并不允许我们在file://这样的URL下面运行，却请求http://URL 的应用程序。这意味着我们要使用Web服务器通过HTTP来访问Hello程序。

这里，void使用的是tomcat，任何其他Web服务器也可以胜任。或者，你也可以直接将代码上传到你的公网服务器。

#### jQuery
jQuery库使得HTML和CSS的处理变得异常简单，它在XML（XMPP节）的操作方面也非常方便。

最新版本请到[jQuery官网](http://jquery.com/)下载。

#### jQuery UI
jQuery UI库提供了我们所需要的一些常见的用户界面构造块，包括对话框和标签页。

最新版本请到[jQuery UI官网](http://jqueryui.com/)下载。

PS：最新的jquery.js和最新的jquery-ui.js可能会有兼容性问题，保险起见，最好使用jQuery UI官网提供的jquery.js。

#### Strophe 
Strophe库使得编写XMPP客户端应用程序变得极其简单，而且它已有多种编程语言版本。当然，我们将使用Javascript版本。

最新版本请到[Strophe官网](http://strophe.im/)下载。

#### flXHR
Strophe能够使用flXHR（标准 XMLHttpRequest API的Flash替代技术）来简化Javascript同源策略的处理。通常，Javascript应用程序不能与外部服务器通信，但借助Flash和flXHR，Strophe能够克服这个限制。

最新版本请到[flXHR官网](http://flxhr.flensed.com/)下载。但是，void登不上去这个网站，也ping不到它的服务器，不知道是被屏蔽了还是下线了。

这里提供[flXHR开源项目](https://github.com/flensed/flXHR)下载地址，很古老，三年前的，莫非这个项目有了更好的替代品？不再更新了？不管了，我们先用着。经过测试，这个项目还是可以使用的。

国内github的下载速度真的让人绝望，尤其下载稍大的项目，看着几K的下载速度，哭的心都有了：大哥，你是要我等到天荒地老吗？

void通过网盘离线下载成功，需要的同学自己取吧，下载地址http://yunpan.cn/cAtFDZVWCJ6QX （提取码：1429）。

flXHR.js依赖的文件较多，这个程序中具体用到的有flensed.js、checkplayer.js、flXHR.swf、flXHR.vbs、swfobject.js、updateplayer.swf。

#### XMPP账号
假设，qq开源，你要写一个qq客户端，想要测试能否连接到qq的服务器，首先要有个qq号吧？一个道理，做测试之前，我们要先有一个XMPP账号。

虽然http://register.jabber.org 已经下线，但是我们可以找到很多其他免费的公共XMPP服务器，这里给出一个列表https://xmpp.net/directory.php 。

这里绝大部分服务器是国外的，速度比较慢，于是void在自己的阿里云上搭建了一个openfire服务器。当然，你也可以本地搭建服务器，然后通过局域网访问。

找到或者搭建好XMPP服务器，然后，下载安装[spark](http://www.igniterealtime.org/downloads/index.jsp)，看看能否注册和登陆，测试是否可用。

至此，假设我们已经拥有了一个XMPP账号。没有搞定的小伙伴请留言。

#### 工具打包下载
上面提到的工具，以及Hello程序的代码，打包到360网盘了，需要的小伙伴自取http://yunpan.cn/cAnXNmvewvGme  （提取码 baaf）

### 具体步骤
这个程序的功能是：向XMPP服务器发送一条信息并将响应显示出来。下面我们来一步一步完成它！

#### 目录结构
接下来的文件放置位置，请下载“工具打包下载”一节的压缩文件，用作参考。文件位置不对的话，需要修改代码，相信你们都懂得。

#### hello.html

新建文件hello.html，内容如下：
```
<!DOCTYPE HTML>
<html>
  <head>
    <title>Hello</title>
    
    <link rel="stylesheet" type="text/css" href="./style/jquery-ui.css" >
	<link rel="stylesheet" type="text/css" href="./style/jquery-ui.theme.css" >
	
	<script type="text/javascript" src="./js/jquery.js"></script>
	<script type="text/javascript" src="./js/jquery-ui.js"></script>	
    <script type="text/javascript" src="./js/strophe.js"></script>   
    <script type="text/javascript" src='./js/strophe.flxhr.js'></script>
	<script type="text/javascript" src='./js/flXHR.js'></script>

    <link rel='stylesheet' type="text/css" href='./style/hello.css'>
    <script type="text/javascript" src='./js/hello.js'></script>
  </head>
  <body>
    <h1>Hello</h1>

    <div id='log'>
    </div>
    <!-- login dialog -->
    <div id='login_dialog' class='hidden'>
      <label>JID:</label><input type='text' id='jid'>
      <label>Password:</label><input type='password' id='password'>
    </div>
  </body>
</html>

```

#### hello.css

新建hello.css文件，内容如下：
```
body {
    font-family: Helvetica;
}

h1 {
    text-align: center;
}

.hidden {
    display: none;
}

#log {
    padding: 10px;
}

```

#### hello.js

新建hello.js文件，内容如下：
```
var Hello = {
    connection: null,
    start_time: null,

    log: function (msg) {
        $('#log').append("<p>" + msg + "</p>");
    },

    send_ping: function (to) {
        var ping = $iq({
            to: to,
            type: "get",
            id: "ping1"}).c("ping", {xmlns: "urn:xmpp:ping"});

        Hello.log("Sending ping to " + to + ".");

        Hello.start_time = (new Date()).getTime();
        Hello.connection.send(ping);
    },

    handle_pong: function (iq) {
        var elapsed = (new Date()).getTime() - Hello.start_time;
        Hello.log("Received pong from server in " + elapsed + "ms.");

        Hello.connection.disconnect();
        
        return false;
    }
};

$(document).ready(function () {
    $('#login_dialog').dialog({
        autoOpen: true,
        draggable: false,
        modal: true,
        title: 'Connect to XMPP',
        buttons: {
            "Connect": function () {
                $(document).trigger('connect', {
                    jid: $('#jid').val(),
                    password: $('#password').val()
                });
                
                $('#password').val('');
                $(this).dialog('close');
            }
        }
    });
});

$(document).bind('connect', function (ev, data) {
    var conn = new Strophe.Connection(
        "http://bosh.metajack.im:5280/xmpp-httpbind");

    conn.connect(data.jid, data.password, function (status) {
        if (status === Strophe.Status.CONNECTED) {
            $(document).trigger('connected');
        } else if (status === Strophe.Status.DISCONNECTED) {
            $(document).trigger('disconnected');
        }
    });

    Hello.connection = conn;
});

$(document).bind('connected', function () {
    // inform the user
    Hello.log("Connection established.");

    Hello.connection.addHandler(Hello.handle_pong, null, "iq", null, "ping1");

    var domain = Strophe.getDomainFromJid(Hello.connection.jid);
    
    Hello.send_ping(domain);

});

$(document).bind('disconnected', function () {
    Hello.log("Connection terminated.");

    // remove dead connection object
    Hello.connection = null;
});
```
上面Javascript的代码由三个基本部分组成。首先是应用程序的命名空间对象，应用程序的状态和函数都在此定义。位于命名空间对象之后的是文档准备就绪事件处理程序，一旦浏览器准备就绪它就会初始化应用程序。最后是自定义事件处理程序，它负责处理那些不是由元素或用户交互触发的程序。

##### 命名空间对象

我们使用命名空间对象，尽可能避免使用全局变量，这样可以将问题降到最少。上面代码中的Hello对象就是一个例子。

##### 文档就绪事件处理程序

一旦DOM可供JavaScript代码使用，就会立即引发文档准备就绪事件。一般而言，最好将初始化代码放在这里。

##### 自定义事件处理程序

jQuery库是的创建和使用自定义事件变得非常容易，这些自定义事件通常用来提高代码的可读性并减少组件之间的耦合度。

#### 发布
上面三个文件，详解请读书[《XMPP高级编程》](http://yunpan.cn/cAn9eRhJnG4Ew)（提取码 20fb），内容太多，不码字了。

完成之后，把整个工程文件夹拷贝到tomcat的webapp文件夹下。然后，启动tomcat，就可以访问了。

### 最终效果
![](http://voidking.qiniudn.com/@/imgs/XMPP/初始界面.jpg)
![](http://voidking.qiniudn.com/@/imgs/XMPP/登陆.jpg)
![](http://voidking.qiniudn.com/@/imgs/XMPP/连接成功.jpg)

### 结束语
如果最终效果是你期望的，恭喜你，至少你已经知其然了！

### 参考文档
《XMPP高级编程（作者Jack Moffitt）》
《XMPP The Definitive Guide（作者Peter Saint-Andre, Kevin Smith, and Remko Tron?on）》
前辈们的技术博客……
