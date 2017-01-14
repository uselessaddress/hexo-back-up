title: XMPP学习笔记——调试控制台
date: 2014-11-28 09:06:27
tags: [jQuery,XMPP]
categories: 设计开发
toc: true
---

### 前言
开发人员总是喜欢不断地加工并完善自己的工具，在开发XMPP应用程序的过程中，我们将需要一款工具来辅助研究和查看协议流量。要是不使用查看源代码命令，或者不能轻易加工URL来测试远程站点的功能，那么很少有Web开发人员能够轻松工作。

对于XMPP节，这样的工具可用来查看协议流量并轻易地创建要发送的节。现在，我们就来做一个这样的工具。

Peek可用来帮助我们研究XMPP扩展如何运转以及为何有些扩展并不能按照预期执行操作。我们可以从应用程序中剪切并粘贴XMPP节构建代码，看看服务器响应究竟是什么。如果不熟悉特定的协议扩展，那么可以输入实例，看看服务器如何响应不同的输入。
<!--more-->
### 操作步骤

其实这个程序和之前的Hello很像，步骤类似。

#### peek.html
新建peek.html，内容如下：
```
<!DOCTYPE HTML>
<html>
  <head>
    <meta http-equiv="Content-type" content="text/html;charset=UTF-8">
    <title>Peek</title>

    <link rel="stylesheet" type="text/css" href="./style/jquery-ui.css" >
	<link rel="stylesheet" type="text/css" href="./style/jquery-ui.theme.css" >
	
	<script type="text/javascript" src="./js/jquery.js"></script>
	<script type="text/javascript" src="./js/jquery-ui.js"></script>	
    <script type="text/javascript" src="./js/strophe.js"></script>   
    <script type="text/javascript" src='./js/strophe.flxhr.js'></script>
	<script type="text/javascript" src='./js/flXHR.js'></script>

    <link rel='stylesheet' type='text/css' href='./style/peek.css'>
    <script src='./js/peek.js'></script>
  </head>
  <body>
    <h1>Peek</h1>

    <div id='console'></div>
    <textarea id='input' class='disabled'
              disabled='disabled'>
	</textarea>

    <div id='buttonbar'>
      <input id='send_button' type='button' value='Send Data'
             disabled='disabled' class='button'>
      <input id='disconnect_button' type='button' value='Disconnect'
             disabled='disabled' class='button'>
    </div>

    <!-- login dialog -->
    <div id='login_dialog' class='hidden'>
      <label>JID:</label><input type='text' id='jid'>
      <label>Password:</label><input type='password' id='password'>
    </div>
  </body>
</html>

```
<!--more-->
#### peek.css
新建peek.css，内容如下：
```
body {
    font-family: Helvetica;
}

h1 {
    text-align: center;
}

#console {
    padding: 10px;
    height: 300px;
    border: solid 1px #aaa;

    background-color: #000;
    color: #eee;
    font-family: monospace;

    overflow: auto;
}

#input {
    width: 100%;
    height: 100px;
    font-family: monospace;
}

.incoming {
    background-color: #111;
}

textarea.disabled {
    background-color: #bbb;
}

#buttonbar {
    margin: 10px;
}

#disconnect_button {
    float: left;
    width: 100px;
}

#send_button {
    float: right;
    width: 100px;
}

/* xml styles */
.xml_punc { color: #888; }
.xml_tag { color: #e77; }
.xml_aname { color: #55d; }
.xml_avalue { color: #77f; }
.xml_text { color: #aaa }
.xml_level0 { padding-left: 0; }
.xml_level1 { padding-left: 1em; }
.xml_level2 { padding-left: 2em; }
.xml_level3 { padding-left: 3em; }
.xml_level4 { padding-left: 4em; }
.xml_level5 { padding-left: 5em; }
.xml_level6 { padding-left: 6em; }
.xml_level7 { padding-left: 7em; }
.xml_level8 { padding-left: 8em; }
.xml_level9 { padding-left: 9em; }

```

#### peek.js
新建peek.js，内容如下：
```
var Peek = {
    connection: null,

    show_traffic: function (body, type) {
        if (body.childNodes.length > 0) {
            var console = $('#console').get(0);
            var at_bottom = console.scrollTop >= console.scrollHeight - 
                console.clientHeight;;

            $.each(body.childNodes, function () {
                $('#console').append("<div class='" + type + "'>" + 
                                     Peek.pretty_xml(this) +
                                     "</div>");
            });

            if (at_bottom) {
                console.scrollTop = console.scrollHeight;
            }
        }
    },

    pretty_xml: function (xml, level) {
        var i, j;
        var result = [];
        if (!level) { 
            level = 0;
        }

        result.push("<div class='xml_level" + level + "'>");
        result.push("<span class='xml_punc'>&lt;</span>");
        result.push("<span class='xml_tag'>");
        result.push(xml.tagName);
        result.push("</span>");

        // attributes
        var attrs = xml.attributes;
        var attr_lead = []
        for (i = 0; i < xml.tagName.length + 1; i++) {
            attr_lead.push("&nbsp;");
        }
        attr_lead = attr_lead.join("");

        for (i = 0; i < attrs.length; i++) {
            result.push(" <span class='xml_aname'>");
            result.push(attrs[i].nodeName);
            result.push("</span><span class='xml_punc'>='</span>");
            result.push("<span class='xml_avalue'>");
            result.push(attrs[i].nodeValue);
            result.push("</span><span class='xml_punc'>'</span>");

            if (i !== attrs.length - 1) {
                result.push("</div><div class='xml_level" + level + "'>");
                result.push(attr_lead);
            }
        }

        if (xml.childNodes.length === 0) {
            result.push("<span class='xml_punc'>/&gt;</span></div>");
        } else {
            result.push("<span class='xml_punc'>&gt;</span></div>");

            // children
            $.each(xml.childNodes, function () {
                if (this.nodeType === 1) {
                    result.push(Peek.pretty_xml(this, level + 1));
                } else if (this.nodeType === 3) {
                    result.push("<div class='xml_text xml_level" + 
                                (level + 1) + "'>");
                    result.push(this.nodeValue);
                    result.push("</div>");
                }
            });
            
            result.push("<div class='xml xml_level" + level + "'>");
            result.push("<span class='xml_punc'>&lt;/</span>");
            result.push("<span class='xml_tag'>");
            result.push(xml.tagName);
            result.push("</span>");
            result.push("<span class='xml_punc'>&gt;</span></div>");
        }
        
        return result.join("");
    },

    text_to_xml: function (text) {
        var doc = null;
        if (window['DOMParser']) {
            var parser = new DOMParser();
            doc = parser.parseFromString(text, 'text/xml');
        } else if (window['ActiveXObject']) {
            var doc = new ActiveXObject("MSXML2.DOMDocument");
            doc.async = false;
            doc.loadXML(text);
        } else {
            throw {
                type: 'PeekError',
                message: 'No DOMParser object found.'
            };
        }

        var elem = doc.documentElement;
        if ($(elem).filter('parsererror').length > 0) {
            return null;
        }
        return elem;
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

    $('#disconnect_button').click(function () {
        Peek.connection.disconnect();
    });

    $('#send_button').click(function () {
        var input = $('#input').val();
        var error = false;
        if (input.length > 0) {
            if (input[0] === '<') {
                var xml = Peek.text_to_xml(input);
                if (xml) {
                    Peek.connection.send(xml);
                    $('#input').val('');
                } else {
                    error = true;
                }
            } else if (input[0] === '$') {
                try {
                    var builder = eval(input);
                    Peek.connection.send(builder);
                    $('#input').val('');
                } catch (e) {
                    console.log(e);
                    error = true;
                }
            } else {
                error = true;
            }
        }

        if (error) {
            $('#input').animate({backgroundColor: "#faa"});
        }
    });

    $('#input').keypress(function () {
        $(this).css({backgroundColor: '#fff'});
    });
});

$(document).bind('connect', function (ev, data) {
    var conn = new Strophe.Connection(
        "http://bosh.metajack.im:5280/xmpp-httpbind");

    conn.xmlInput = function (body) {
        Peek.show_traffic(body, 'incoming');
    };

    conn.xmlOutput = function (body) {
        Peek.show_traffic(body, 'outgoing');
    };

    conn.connect(data.jid, data.password, function (status) {
        if (status === Strophe.Status.CONNECTED) {
            $(document).trigger('connected');
        } else if (status === Strophe.Status.DISCONNECTED) {
            $(document).trigger('disconnected');
        }
    });

    Peek.connection = conn;
});

$(document).bind('connected', function () {
    $('.button').removeAttr('disabled');
    $('#input').removeClass('disabled').removeAttr('disabled');
});

$(document).bind('disconnected', function () {
    $('.button').attr('disabled', 'disabled');
    $('#input').addClass('disabled').attr('disabled', 'disabled');
});

```

### 输入测试及运行结果

![](http://voidking.qiniudn.com/@/imgs/XMPP/peek登陆.jpg)
![](http://voidking.qiniudn.com/@/imgs/XMPP/连接成功1.jpg)
![](http://voidking.qiniudn.com/@/imgs/XMPP/连接成功2.jpg)

#### 控制出席

打开Peek应用程序，登陆到XMPP服务器，输入`<presence/>`，然后点击Send按钮。
![](http://voidking.qiniudn.com/@/imgs/XMPP/presence结果.jpg)

#### 设置状态为已离开
```
$pres().c('show').t("away").up().c('status').t("reading");
```
![](http://voidking.qiniudn.com/@/imgs/XMPP/设置状态已离开.jpg)

#### 探测版本

```
$iq({type:"get",id:"version",to:"jabber.org"}).c("query",{xmlns:"jabber:iq:version"});
```
或者
```
<iq type='get' id='version1' to='jabber.org'><query xmlns='jabber:iq:version'/></iq>
```
![](http://voidking.qiniudn.com/@/imgs/XMPP/探测版本.jpg)

没有反应，不知道为什么。

#### iq节错误

```
$iq({type:"get",id:"version2",to:"gmail.com"}).c("query",{xmlns:"jabber:iq:version"});
```
![](http://voidking.qiniudn.com/@/imgs/XMPP/iq节错误.jpg)

```
<iq type='get' id='info1' to='bad-room-123@conference.jabber.org'>
	<query xmlns='http://jabber.org/protocol/disco#info' />
</iq>
```
![](http://voidking.qiniudn.com/@/imgs/XMPP/iq节错误2.jpg)
#### message节错误
```
$msg({to:'voidking@voidking.lit',type:'chat'}).c('body').t('What think you of books ?');
```
![](http://voidking.qiniudn.com/@/imgs/XMPP/message节错误.jpg)

#### presence节错误
不知道怎么发送presence节错误信息，省略先。

### 结束语

这篇文章也是摘自《XMPP高级编程》，void也不详细解释代码了，看不懂的地方，请小伙伴参考这本书。

### 参考文档
《XMPP高级编程（作者Jack Moffitt）》
《XMPP The Definitive Guide（作者Peter Saint-Andre, Kevin Smith, and Remko Tron?on）》
前辈们的技术博客……
