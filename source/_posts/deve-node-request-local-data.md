---
title: 利用Node的request模块访问本地数据
toc: true
date: 2016-07-28 20:19:21
tags:
- 前端
- node
- request
categories: 设计开发
---
# 前言
最近公司系统架构调整，数据还没有迁移完毕，很多接口出现了问题，联调成了一件困难的事。开发还要继续，牛逼的前端自力更生！

<!--more-->

# 获取地址
以获取用户的地址信息为例，对代码进行改造。
## 原代码
```
// 获取用户地址信息
var queryAddressUrl = config.apihost + '/address/selAddress';
var param = {
  accessToken: accessToken
};
request.post({url: queryAddressUrl, form:JSON.stringify(param)},function(error, response, body){
  console.log(response.body);
  if(!error && response.statusCode == 200){
    ep.emit('address',response.body);
  }
});
```

## 改造后代码
```
// 获取用户地址信息
// var queryAddressUrl = config.apihost + '/address/selAddress';
var queryAddressUrl = config.host + '/data/address.json';
var param = {
  accessToken: accessToken
};
// post改为get
request.get({url: queryAddressUrl, form:JSON.stringify(param)},function(error, response, body){
  console.log(response.body);
  if(!error && response.statusCode == 200){
    ep.emit('address',response.body);
  }
});
```
两段代码对比，可以发现，请求的url由远程接口地址改为了本地文件地址，请求的type由post改为了get。

## address.json
因为public文件夹的虚拟路径为网站根目录，且为静态文件目录，所以在/public/data文件夹下，新建文件address.json，内容如下：
```
{
    "code": 1,
    "ext": {
        "msg": "成功"
    },
    "obj": {
        "pcdAddress": "江苏省 南京市 雨花台区",
        "purchaserName": "郝锦",
        "purchaserTel": "15195892217",
        "street": "华博智慧园",
        "areaId": 0,
        "defaultAddress": false
    }
}
```
至此，改造成功，可以愉快地获取数据了！

# 后记
有些需要计算的数据并不能模拟，比如支付宝的签名字符串。

利用ajax也可以获取本地数据，对于前端开发者来说同样很实用。用法很简单，一看就会，详情自行百度。