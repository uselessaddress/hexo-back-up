---
title: 微信自定义分享中node处理url
toc: true
date: 2016-07-18 21:35:24
tags:
- 微信
- node
- url
categories: 设计开发
---
# 前言
程序员的工作就是填坑，有时是填别人挖的坑，有时是填自己挖的坑。

<!--more-->

# 微信自定义分享
在使用node开发的时候，经常会用到微信自定义分享。自定义分享的步骤，大致如下：

## 微信配置
登录微信公众平台，进入“公众号设置”的“功能设置”里填写“JS接口安全域名”。

## node端
1、设置分享数据（TKD）。
2、获取权限验证配置数据。
3、获取用户信息（非必要），因为需要在分享页面显示分享用户的昵称。

```
var config = require('../config');
var urlencode = require('urlencode');
var eventproxy = require('eventproxy');
var request = require('request');
var WechatAPI = require('wechat-api');

var WXapi = new WechatAPI(config.weixin.appid, config.weixin.appsecret);

// 微信
exports.home = function(req, res){
    // 获取code
    var subscribe = 1;
    if(!req.query.code){
        var r_url = config.host+'/weixin/home';
        var url = 'https://open.weixin.qq.com/connect/oauth2/authorize?appid='+config.weixin.appid+'&redirect_uri='+urlencode(r_url)+'&response_type=code&scope=snsapi_userinfo&state=111#wechat_redirect';
        res.redirect(url);
    }else{
        var code = req.query.code;
        var state = req.query.state;
        
        var ep = new eventproxy();
        ep.all('shareConfig','userInfo',function(shareConfigData,userInfoData){
            console.log(shareConfigData);
            console.log(userInfoData);
            // 设置分享数据（TKD）
            var shareData = {
                enable: true,
                title: '分享',
                icon: 'http://7oxjrx.com1.z0.glb.clouddn.com//imgs/head.jpg',
                desc: '没见过这么拉风的分享描述吧！',
                mUrl: config.host+'/weixin/userinfo?openid=000&nickname=voidking&num=10'
            };
            res.render('./weixin/home',{
                title: '微信',
                host: config.host,
                shareConfigData: shareConfigData,
                shareData: shareData,
                userInfoData: JSON.parse(userInfoData)
            });
        });

        var param = {
            debug: false,
            jsApiList: ['onMenuShareTimeline', 'onMenuShareAppMessage'],
            url: req.protocol+"://"+req.hostname+req.originalUrl
        };     
        WXapi.getJsConfig(param, function(err,result){
            // result就是权限配置验证配置数据
            ep.emit('shareConfig',result);    
        });

        // 通过code换取网页授权access_token
        var params = {
            appid:config.weixin.appid,
            secret:config.weixin.appsecret,
            code:code,
            grant_type:'authorization_code'
        };
        var getAccessTokenUrl = 'https://api.weixin.qq.com/sns/oauth2/access_token?appid='+params.appid+'&secret='+params.secret+'&code='+params.code+'&grant_type='+params.grant_type;
        request.get(getAccessTokenUrl,function(error, response, body){
            if (!error && response.statusCode == 200) {
                var re = JSON.parse(response.body);
                console.log(re);
                var getuserinfo = 'https://api.weixin.qq.com/sns/userinfo?access_token='+re.access_token+'&openid='+re.openid+'&lang=zh_CN';
                request.get(getuserinfo,function(error2, response2, body2){
                    if (!error2 && response2.statusCode == 200) {
                        ep.emit('userInfo',response2.body);
                    }
                });
            }
        });
    }
}

```

## 前端
1、前端页面中引入微信的js文件`http://res.wx.qq.com/open/js/jweixin-1.0.0.js`。
2、微信权限验证数据配置。
3、分享数据配置。
```
<script type="text/javascript">
    wx.config({
       debug:  false,  //调式模式，设置为ture后会直接在网页上弹出调试信息，用于排查问题
       appId: '<%= shareConfigData.appId%>',
       timestamp: '<%= shareConfigData.timestamp%>',
       nonceStr: '<%= shareConfigData.nonceStr%>',
       signature: '<%= shareConfigData.signature%>',
       jsApiList: [  //需要使用的网页服务接口
           'checkJsApi',  //判断当前客户端版本是否支持指定JS接口
           'onMenuShareTimeline', //分享给好友
           'onMenuShareAppMessage', //分享到朋友圈
           'onMenuShareQQ',  //分享到QQ
           'onMenuShareWeibo' //分享到微博
       ]
     });
     wx.ready(function () {   //ready函数用于调用API，如果你的网页在加载后就需要自定义分享和回调功能，需要在此调用分享函数。//如果是微信游戏结束后，需要点击按钮触发得到分值后分享，这里就不需要调用API了，可以在按钮上绑定事件直接调用。因此，微信游戏由于大多需要用户先触发获取分值，此处请不要填写如下所示的分享API
        wx.onMenuShareTimeline({  //例如分享到朋友圈的API  
           title: '<%= shareData.title%>', // 分享标题
           link: '<%= shareData.mUrl%>', // 分享链接
           imgUrl: '<%= shareData.icon%>', // 分享图标
           desc:'<%= shareData.desc%>',
           success: function () {
               // 用户确认分享后执行的回调函数
           },
           cancel: function () {
               // 用户取消分享后执行的回调函数
           }
        });
        wx.onMenuShareAppMessage({  //例如分享到朋友圈的API  
           title: '<%= shareData.title%>', // 分享标题
           link: '<%= shareData.mUrl%>', // 分享链接
           imgUrl: '<%= shareData.icon%>', // 分享图标
           desc:'<%= shareData.desc%>',
           success: function () {
               // 用户确认分享后执行的回调函数
           },
           cancel: function () {
               // 用户取消分享后执行的回调函数
           }
        });
        wx.onMenuShareQQ({  //例如分享到朋友圈的API  
           title: '<%= shareData.title%>', // 分享标题
           link: '<%= shareData.mUrl%>', // 分享链接
           imgUrl: '<%= shareData.icon%>', // 分享图标
           desc:'<%= shareData.desc%>',
           success: function () {
               // 用户确认分享后执行的回调函数
           },
           cancel: function () {
               // 用户取消分享后执行的回调函数
           }
        });
        wx.onMenuShareWeibo({  //例如分享到朋友圈的API  
           title: '<%= shareData.title%>', // 分享标题
           link: '<%= shareData.mUrl%>', // 分享链接
           imgUrl: '<%= shareData.icon%>', // 分享图标
           desc:'<%= shareData.desc%>',
           success: function () {
               // 用户确认分享后执行的回调函数
           },
           cancel: function () {
               // 用户取消分享后执行的回调函数
           }
        });
    }); 
    wx.error(function (res) {
        // alert(res.errMsg);  //打印错误消息。及把 debug:false,设置为debug:ture就可以直接在网页上看到弹出的错误提示
    });
</script>
```

## 源码
https://github.com/voidking/nodebase/blob/master/controllers/weixin.js
https://github.com/voidking/nodebase/blob/master/views/weixin/home.html


# `&`转`&amp;`
以上，已经完成了微信自定义分享的工作。然而，还存在一些坑！！！

如上，我们的分享页面url为`http://wx.voidking.com/weixin/userinfo?openid=000&nickname=voidking&num=10`，参数是写死的，实际生产环境中需要使用真实数据拼接出来。

该分享页面的url，如果输出到页面上，就显示正常，比如使用隐藏的input接收。
```
<input type="hidden" id="url" value="<%= shareData.url%>">
```
显示结果为
```
<input type="hidden" id="url" value="http://wx.voidking.com/weixin/userinfo?openid=000&nickname=voidking&num=10">
```

但是，如果输出到微信分享的js中，url中的`&`就会变成`&amp;`。
```
wx.onMenuShareTimeline({  //例如分享到朋友圈的API  
    link: '<%= shareData.mUrl%>', // 分享链接
    // 其他分享信息省略
});
```
显示结果为
```
wx.onMenuShareTimeline({  //例如分享到朋友圈的API  
    link: 'http://wx.voidking.com/weixin/userinfo?openid=000&amp;nickname=voidking&amp;num=10', // 分享链接
    // 其他分享信息省略
});
```

啊嘞，`&`转义成了`&amp;`！！！没错，这就是第一个坑。因为分享出去的url参数不是以常规`&`间隔，而是`&amp;`，所以增加了我们截取参数的难度。

有两种解决方案：
1、前端解决。输入内容到隐藏的input，然后获取值，添加到微信分享的js中。经测试，行不通。
2、node端解决。在跳转分享页面时，改变获取参数的方式，代码如下。

```
exports.userinfo = function(req, res){
    
    function getArg(str,arg) {
        var reg = new RegExp('(^|&)' + arg + '=([^&]*)(&|$)', 'i');
        var r = str.match(reg);
        if (r != null) {
            return unescape(r[2]);
        }
        return null;
    }
    String.prototype.replaceAll  = function(s1,s2){     
        return this.replace(new RegExp(s1,"gm"),s2);     
    } 

    var str = decodeURI(req.url.split('?')[1]);
    console.log(str);
    str = str.replaceAll('&amp%3B','&'); 
    str = str.replaceAll('&amp;','&');
    console.log(str);
    var name = getArg(str,'name');
    console.log(name);
    res.render('weixin/userinfo',{
        title: '用户信息',
        host: config.host
    });
}
```

# `;`转`%3B`
以上，我们解决了微信分享中`&`转义的问题，在电脑端或者安卓端查看分享都是正常的。然而，在IOS端查看会出现错误！！！

因为链接`http://wx.voidking.com/weixin/userinfo?openid=000&amp;nickname=voidking&amp;num=10`在使用IOS访问的时候，变成了`http://wx.voidking.com/weixin/userinfo?openid=000&amp%3Bnickname=voidking&amp%3Bnum=10`，`;`转义成了`%3B`！！！

解决办法，参考`&`转`&amp;`一节node端代码，替换字符串即可。

# 从url中截取中文参数
以上，url属性中name的值是voidking，没有什么问题。然而，当我把name的值换为“帅哥”时，控制台打印出了乱码。啊哈，这就对了，中文肯定要处理一下嘛！

解决办法，使用decodeURI函数解码编码过的uri，参考`&`转`&amp;`一节node端代码。

# 书签
微信公众平台开发者文档——微信JS-SDK说明文档
http://mp.weixin.qq.com/wiki/7/aaa137b55fb2e0456bf8dd9148dd613f.html

微信公共平台Node库 API
http://doxmate.cool/node-webot/wechat-api/api.html

wechat-api Documentation
http://doxmate.cool/node-webot/wechat-api/api.html#api_js_exports_getJsConfig