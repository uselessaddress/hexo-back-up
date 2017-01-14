---
title: 通过浏览器打开应用程序
toc: true
date: 2016-06-23 16:06:00
tags:
- 前端
categories: 设计开发
---
# 原理
通过浏览器打开应用程序，利用的是在浏览器地址栏中输入一个协议，如果有本地程序（应用）能够解析这个协议，那么这个应用将被调起。

<!--more-->

# 前端代码
如果安装过app调起app，否则页面跳转到app下载页面，这个逻辑分支实现原理如下：

当在浏览器地址栏输入app定义的协议调起app时，如果已经安装过，系统中断浏览器进程，继而响应app进程，因而在切换到app的时候浏览器中js代码将不再执行，当再次切换回来时系统进程又切回浏览器，js代码从中断处继续执行。所做的逻辑分支就是利用这个关键点。可设置延时来做分支，当app可以被调起时，切换到app，js不再执行，这期中有个app调起时间（与系统有关），如果设置的延时时间过短，则js会继续执行。当切换回来，记录切换回来的时间点，与切换之前比较大于设定时间说明已经切换成功，将不再执行下载，否则下载。

PS：app的协议是与app开发同学约定，由app开发同学通过activity scheme设置的。

```
var appInfo = {
    iosUrl: "",    //打开IOS客户端链接
    androidUrl: "",  //打开ANdroid客户端的链接
    downloadUrl: ""  //下载客户端或者去App Store的链接
};

var t1 = new Date().getTime();

iframe2open();
jump2download();

//利用iframe打开客户端
function iframe2open(){
    var src = $.os.ios ? appInfo.iosUrl : appInfo.androidUrl,
        iframe = document.createElement("iframe");

    iframe.src = src;
    iframe.style.display = "none";
    document.body.appendChild(iframe); 
} 

//跳转下载页面
function jump2download(){
    var _timer = null;

    clearTimeout(_timer);
    _timer = setTimeout(function(){
        var t2 = new Date().getTime();

        if (t2 - t1 <= 800) {
            window.location = appInfo.downloadUrl;
        }
    },400);
}
```

# 书签
通过浏览器直接打开iOS/Android App 应用程序
http://itindex.net/blog/2014/11/07/1415353560000.html

[Make a link in the Android browser start up my app?](http://stackoverflow.com/questions/3469908/make-a-link-in-the-android-browser-start-up-my-app)

[Launch custom android application from android browser](http://stackoverflow.com/questions/2958701/launch-custom-android-application-from-android-browser)

非微信内置浏览器中的网页调起微信支付的方案研究
http://blog.csdn.net/ahence/article/details/51317814

浏览器调起app应用方法
http://blog.csdn.net/u012193330/article/details/52190143

android自定义协议和html加载时自动尝试调用本地APP
http://www.oschina.net/code/snippet_256033_35330/