---
title: 使用accessToken记录登录状态
toc: true
date: 2016-06-23 10:51:10
tags:
- Node
- Cookie
categories: 设计开发
---

# js设置cookie
## 海哥的插件
浏览器端用到了海哥写的Cookie插件：https://github.com/voidking/jquery-cookie ，值得好好学习一下。
```
cookie.prototype.setCookie('accessToken', accessToken,7);
```

## jquery官方插件
如果使用jquery官方给的cookie插件，用法如下：
```
$.cookie('accessToken', accessToken,{ expires: 7});
```

<!--more-->
## 原生js
如果使用原生的JS来操作cookie，用法如下：
```
//获取当前时间
var date=new Date();
var expireDays=7;
//将date设置为7天以后的时间
date.setTime(date.getTime()+expireDays*24*3600*1000);
//或者也可以用
date.setDate(date.getDate+expireDays);
//将accessToken这个cookie设置为7天后过期
document.cookie = 'accessToken='+accessToken+';expires=7';
```

# js删除cookie
## 海哥的插件
```
cookie.prototype.delCookie('accessToken'); 
```

## jquery官方插件
```
$.removeCookie('accessToken');
```

## 原生JS
```
//获取当前时间
var date=new Date();
//将date设置为过去的时间
date.setTime(date.getTime()-10000);
//将accessToken这个cookie删除
document.cookie='accessToken=v; expire='+date.toGMTString();
console.log(document.cookie);
```

# js获取cookie
## 海哥的插件
```
var accessToken = cookie.prototype.getCookie('accessToken');
```

## jquery官方插件
```
var accessToken = $.cookie('accessToken');
```

## 原生js
```
var accessToken = getCookie('accessToken');
function getCookie(objName){
    var arrStr = document.cookie.split("; ");
    for(var i = 0;i < arrStr.length;i ++){
            var temp = arrStr[i].split("=");
            if(temp[0] == objName) 
                return unescape(temp[1]);
    }
}
```


# Node端获取cookie
```
var Cookies = {};
req.headers.cookie && req.headers.cookie.split(';').forEach(function( Cookie ) {
    var parts = Cookie.split('=');
    Cookies[ parts[ 0 ].trim() ] = ( parts[ 1 ] || '' ).trim();
});
var accessToken = Cookies['accessToken'] || '';
```

# 后记
accessToken可以在前端获取后传给Node端，也可以在Node端获取。如果这个accessToken不为空，则表明该用户已经登录，否则表明该用户未登录。

# 参考文档
jQuery Cookie
http://plugins.jquery.com/cookie/

jQuery Cookie项目地址
https://github.com/carhartl/jquery-cookie

关于document.cookie的使用
http://www.cnblogs.com/newsouls/archive/2012/11/12/2766567.html