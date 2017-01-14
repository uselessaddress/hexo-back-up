---
title: 微信获取用户信息
toc: true
date: 2016-07-14 17:40:29
tags:
- 微信
- 前端
categories: 设计开发
---
# 前言
在微信公众平台开发中，经常会需要获取用户信息。获取用户信息的方法很简单，传入ACCESS_TOKEN和OPENID，直接请求微信给出的接口就可以了。

<!--more-->

本文中，参照方倍工作室的分类方法，把access_token分为两种。一种是使用AppID和AppSecret获取的access_token，一种是OAuth2.0授权中产生的access_token，方倍工作室分别称为全局access_token和授权access_token。

# 全局access_token

## 获取全局access_token
传入APPID和APPSECRET，请求微信给出的接口，就可以获取到全局access_token。
```
https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=APPID&secret=APPSECRET
```

返回结果：
```
{
    "access_token": "NU7Kr6v9L9TQaqm5NE3OTPctTZx797Wxw4Snd2WL2HHBqLCiXlDVOw2l-Se0I-WmOLLniAYLAwzhbYhXNjbLc_KAA092cxkmpj5FpuqNO0IL7bB0Exz5s5qC9Umypy-rz2y441W9qgfnmNtIZWSjSQ",
    "expires_in": 7200
}
```

## 获取openid
用户关注以及回复消息的时候，均可以获得用户的OpenID。
```
<xml>
    <ToUserName><![CDATA[gh_b629c48b653e]]></ToUserName>
    <FromUserName><![CDATA[ollB4jv7LA3tydjviJp5V9qTU_kA]]></FromUserName>
    <CreateTime>1372307736</CreateTime>
    <MsgType><![CDATA[event]]></MsgType>
    <Event><![CDATA[subscribe]]></Event>
    <EventKey><![CDATA[]]></EventKey>
</xml>
```
其中的FromUserName就是OpenID。

## 获取用户信息
有了access_token和openid，请求如下接口：
```
https://api.weixin.qq.com/cgi-bin/user/info?access_token=ACCESS_TOKEN&openid=OPENID&lang=zh_CN
```

获取到的用户信息数据格式如下：
```
{
    "subscribe": 1, 
    "openid": "o6_bmjrPTlm6_2sgVt7hMZOPfL2M", 
    "nickname": "Band", 
    "sex": 1, 
    "language": "zh_CN", 
    "city": "广州", 
    "province": "广东", 
    "country": "中国", 
    "headimgurl": "http://wx.qlogo.cn/mmopen/g3MonUZtNHkdmzicIlibx6iaFqAc56vxLSUfpb6n5WKSYVY0ChQKkiaJSgQ1dZuTOgvLLrhJbERQQ4eMsv84eavHiaiceqxibJxCfHe/0", 
    "subscribe_time": 1382694957,
    "unionid": "o6_bmasdasdsad6_2sgVt7hMZOPfL"
    "remark": "",
    "groupid": 0
}
```

# 授权access_token

## 微信配置
假设M站的网址为`http://wx.voidking.com`，那么在微信公众平台上，JS接口安全域名修改为`wx.voidking.com`，网页授权获取用户基本信息修改为`wx.voidking.com`。

## 获取code
在确保微信公众账号拥有授权作用域（scope参数）的权限的前提下（服务号获得高级接口后，默认拥有scope参数中的snsapi_base和snsapi_userinfo），引导关注者打开如下页面：
```
https://open.weixin.qq.com/connect/oauth2/authorize?appid=APPID&redirect_uri=REDIRECT_URI&response_type=code&scope=SCOPE&state=STATE#wechat_redirect
```
若提示“该链接无法访问”，请检查参数是否填写错误，是否拥有scope参数对应的授权作用域权限。


## 获取授权access_token和openid
获取code后，请求接口：
```
https://api.weixin.qq.com/sns/oauth2/access_token?appid=APPID&secret=SECRET&code=CODE&grant_type=authorization_code
```

返回数据如下：
```
{
   "access_token":"ACCESS_TOKEN",
   "expires_in":7200,
   "refresh_token":"REFRESH_TOKEN",
   "openid":"OPENID",
   "scope":"SCOPE",
   "unionid": "o6_bmasdasdsad6_2sgVt7hMZOPfL"
}
```

## 获取用户信息
获取code时，如果网页授权作用域为snsapi_userinfo，则此时开发者可以通过access_token和openid拉取用户信息了。请求接口如下：
```
https://api.weixin.qq.com/sns/userinfo?access_token=ACCESS_TOKEN&openid=OPENID&lang=zh_CN
```

返回数据如下：
```
{
    "openid":"OPENID",
    "nickname":"NICKNAME",
    "sex":"1",
    "province":"PROVINCE"
    "city":"CITY",
    "country":"COUNTRY",
    "headimgurl":"http://wx.qlogo.cn/mmopen/g3MonUZtNHkdmzicIlibx6iaFqAc56vxLSUfpb6n5WKSYVY0ChQKkiaJSgQ1dZuTOgvLLrhJbERQQ4eMsv84eavHiaiceqxibJxCfHe/46", 
    "privilege":[
        "PRIVILEGE1",
        "PRIVILEGE2"
    ],
    "unionid": "o6_bmasdasdsad6_2sgVt7hMZOPfL"
}
```

## Node端源码
https://github.com/voidking/nodebase/blob/master/controllers/weixin.js

# 两种方法的对比

## 接口
全局access_token获取用户信息接口为`https://api.weixin.qq.com/cgi-bin/user/info`。
授权access_token获取用户信息接口为`https://api.weixin.qq.com/sns/userinfo`。

## 返回数据
全局access_token获取用户信息返回数据为：
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/weixin-get-userinfo/global.jpg)

授权access_token获取用户信息返回数据为：
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/weixin-get-userinfo/auth.jpg)

## 小结
通过授权access_token无法得知用户是否关注了公众号，必须通过全局access_token。

# 后记
有问题，看文档。经历了四年发展的微信公众平台，文档已经越来越完善。

# 书签
微信公众平台开发(76) 获取用户基本信息
http://www.cnblogs.com/txw1958/p/weixin76-user-info.html

微信公众平台开发者文档——获取用户基本信息
http://mp.weixin.qq.com/wiki/14/bb5031008f1494a59c6f71fa0f319c66.html

微信公众平台开发者文档——网页授权获取用户基本信息
http://mp.weixin.qq.com/wiki/17/c0f37d5704f0b64713d5d2c37b468d75.html

微信公共平台Node库 API
http://doxmate.cool/node-webot/wechat-api/api.html