---
title: Chrome插件开发基础
toc: true
date: 2016-10-19 16:49:53
tags:
- chrome
- 插件
categories: 设计开发
---
# 前言
前些天，小伙伴问我会不会开发chrome插件。突然意识到，这是个有趣的事情，那就搞一搞。

最开始想做一个批量下载图片插件，很快找到一个fatkun。做一个视频广告屏蔽插件？太难，放弃。要不做一个翻墙插件？更难，放弃。做一个划词翻译插件怎么样？已经有了Google翻译和划词翻译。做一个页面截图插件？已经有了捕捉网页截图 - FireShot的。做一个markdown编辑器？已经有了Markdown Here。

果然是前人种树，后人没地种哇！后来想做一个柯林斯词典翻译，然而没有平台提供这个词典的接口，唯一的办法是抓取百度翻译的页面，然后截取结果。太麻烦，还要搭个服务器。

最终，决定做一个获取公网IP的插件。
开发工具：sublime；
开发语言：html+css+js。

<!--more-->

# 官方入门demo
## 目录结构
chrome-extention-start
|-icon.png
|-manifest.json
|-popup.html
|-popup.js

其中，icon.png是插件的图标；manifest.json是插件的配置文件；popup.html是单击插件图标后弹出的页面；popup.js是业务处理的js。


## 启用插件
访问`chrome://extensions`，勾选开发者模式，加载已解压的扩展程序，选择chrome-extention-start文件夹。

## 试用
官方给出的效果是这样的：
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/chrome-extention/gettingstarted-preview.png)

小编满怀期待打开YouTube，结果是这样的：
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/chrome-extention/real.jpg)
不要在意这些细节，至少第一个插件已经跑起来了！

# 查询公网IP
## manifest.json
```
{
  "manifest_version": 2,

  "name": "Public IP",
  "description": "This extension shows your public IP",
  "icons": {
    "16": "icon_16.png",
    "48": "icon_48.png",
    "64": "icon_64.png",
    "128": "icon_128.png"
  },
  "version": "1.0",

  "browser_action": {
    "default_icon": "icon_19.png",
    "default_title": "查看公网IP",
    "default_popup": "popup.html"
  },
  "permissions": [
    "http://ip.chinaz.com/getip.aspx"
  ]
}
```
需要注意的是，permissions中的url是允许跨域的。

## popup.js
```
$(function(){
  String.prototype.replaceAll  = function(s1,s2){     
    return this.replace(new RegExp(s1,"gm"),s2);     
  }

  $.ajax({
    url: 'http://ip.chinaz.com/getip.aspx',
    type: 'GET',
    data: {},
    success: function(data){
      console.log(data);
      data = data.replace("ip","'ip'");
      data = data.replace("address","'address'");
      data = data.replaceAll("\'","\"");
      console.log(data);
      var dataJson = JSON.parse(data);
      $('#ip').val(dataJson.ip);
      $('#address').val(dataJson.address);
    },
    error: function(xhr){
      console.log(xhr);
    }
  });
  
});
```



## 效果
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/chrome-extention/result.jpg)

# 发布
插件的发布非常简单，上传项目的zip压缩包之后，填写一些信息即可。但是，想要获得发布插件的权限，非常麻烦，具体我会在后记部分吐槽。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/chrome-extention/info.jpg)

# 本地插件安装
假设电脑被墙，又需要安装Chrome插件，那么下载crx文件，然后在Chrome地址栏输入`chrome://extensions/`，并将下载的crx拖入该页面即可。

# 后记
谷歌，你妹！这特么歧视也太明显了吧！大陆开发者，无法发布应用。要想发布，只能使用除了中国大陆外的信用卡支付！我*&……%￥#@！

历经波折，终于拿到一张全球付的卡。然而由于激动，进错了页面，到了Google Play的支付页面，填写信息时居然有中国的选项，理所当然选了中国，填了真实信息。

后来，发现进错页面，回到[开发者信息中心](https://chrome.google.com/webstore/developer/dashboard/)，却再也无法点出付款的页面！！！
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/chrome-extention/error.jpg)
当我转到google payment center，却提示遇到了一个问题！What the fuck！这是摆明了不给修改的机会哇！
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/chrome-extention/error2.jpg)

万般无奈，注册了小号，才支付成功。
最后，附上插件链接：
https://chrome.google.com/webstore/detail/public-ip/cdpgbfmmhnbmkdnhlgjpbjdgblggnhck

# 书签
Getting Started: Building a Chrome Extension
https://developer.chrome.com/extensions/getstarted

360极速浏览器应用开放平台
http://open.chrome.360.cn/extension_dev/overview.html

Chrome扩展技术手册
http://crxdoczh.readthedocs.io/en/latest/

Chrome扩展及应用开发（首发版）
http://www.ituring.com.cn/book/1421

如何从零开始写一个 Chrome 扩展？
https://www.zhihu.com/question/20179805

2016年最实用的七款chrome浏览器“神器”级别插件
http://mt.sohu.com/20160621/n455512782.shtml

如何发布一款Chrome App
https://segmentfault.com/a/1190000000354014

发布了几个自己制作的Chrome插件
http://www.jianshu.com/p/NpjteJ

Chrome插件（Extensions）开发攻略
http://www.cnblogs.com/guogangj/p/3235703.html

Google Play Developer Console
https://play.google.com/apps/publish/signup/

开发者信息中心
https://chrome.google.com/webstore/developer/dashboard/

Google payment center
https://payments.google.com/payments/home#settings

全球付 -国际购物-在线消费新体验
https://www.globalcash.hk/index-welcome.do