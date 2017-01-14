---
title: Cookie、localStorage和sessionStorage
toc: true
date: 2016-07-30 11:18:09
tags:
- 前端
- cookie
- localstorage
- sessionstorage
categories: 设计开发
---
# 前言
在《使用accessToken记录登录状态》一文中，已经讨论了Cookie的增删查改。
本文详细探讨一下Cookie、localStorage和sessionStorage的概念差别，以及localStorage的用法。

<!--more-->

# Cookie
Cookie的特点是小，只有4k，常用来存储辨别用户信息的数据，比如accessToken。而且，Cookie有数量限制，每个特定的域名下，最多生成50个Cookie（IE7+）。最大的优势是几乎所有的浏览器都支持。

# localStorage
localStorage 是 HTML5 标准中新加入的技术，拥有5M的存储空间，主流浏览器都支持。

## 存储
```
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
```

## 读取
```
var temp = (window.localStorage && window.localStorage.userInfo)?JSON.parse(window.localStorage.userInfo):{}
var unionid = temp.unionid;
console.log(unionid);
```

# sessionStorage
sessionStorage和localStorage非常相似，最主要的差别，是生命周期。localStorage除非被清除，否则永久保存；sessionStorage关闭页面或浏览器后被清除。

# 异同 

| 特性 | Cookie | localStorage  |  sessionStorage | 
| ----- | ----- | ----- | ----- | 
| 数据的生命期 | 可设置失效时间，默认是关闭浏览器后失效 | 除非被清除，否则永久保存 | 仅在当前会话下有效，关闭页面或浏览器后被清除 |  
| 存放数据大小 | 4K左右  |  一般为5MB | 一般为5MB |
| 与服务器端通信 | 每次都会携带在HTTP头中，如果使用cookie保存过多数据会带来性能问题 |    仅在客户端（即浏览器）中保存，不参与和服务器的通信 | 仅在客户端（即浏览器）中保存，不参与和服务器的通信 | 
| 易用性 | 需要程序员自己封装，源生的Cookie接口不友好 | 源生接口可以接受，亦可再次封装来对Object和Array有更好的支持 | 源生接口可以接受，亦可再次封装来对Object和Array有更好的支持 |

# 书签
详说 Cookie, LocalStorage 与 SessionStorage
https://segmentfault.com/a/1190000002723469

[HTML5 localStorage本地存储实际应用举例](http://www.zhangxinxu.com/wordpress/2011/09/html5-localstorage%E6%9C%AC%E5%9C%B0%E5%AD%98%E5%82%A8%E5%AE%9E%E9%99%85%E5%BA%94%E7%94%A8%E4%B8%BE%E4%BE%8B/)

谈谈本地存储利弊Cookie、localStorage、sessionStorage
http://www.tuicool.com/articles/fM32ier
