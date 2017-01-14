---
title: 支付宝手机网站支付流程
toc: true
date: 2016-07-29 10:15:34
tags:
- node
- 前端
- 支付宝
categories: 设计开发
---
# 前言
公司M站要接入支付宝，借机研究了一下支付宝的支付流程。毕竟，只有公司才能拿到支付接口权限。

主要参考文档：
https://doc.open.alipay.com/doc2/detail?treeId=60&articleId=103564&docType=1
https://b.alipay.com/order/productDetail.htm?productId=2015110218008816

<!--more-->

# 手机网站支付接口

## 产品简介
手机网站支付主要应用于手机、掌上电脑等无线设备的网页上，通过网页跳转或浏览器自带的支付宝快捷支付实现买家付款的功能，资金即时到账。

## 申请条件
1、您申请前必须拥有企业支付宝账号（不含个体工商户账号），且通过支付宝实名认证审核。
2、如果您有已经建设完成的无线网站（非淘宝、天猫、诚信通网店），网站经营的商品或服务内容明确、完整（古玩、珠宝等奢侈品、投资类行业无法申请本产品）。
3、网站必须已通过ICP备案，备案信息与签约商户信息一致。

# 配置
假设我们已经成功申请到手机网站支付接口，在进行开发之前，需要使用公司账号登录支付宝开放平台。

## 查看PID
1、开发者登录开放平台，点击右上角的“账户及密钥管理”。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/alipay-process/pid.png)

2、选择“合作伙伴密钥”，即可查询到合作伙伴身份（PID），以2088开头的16位纯数字。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/alipay-process/pid2.png)

## 配置密钥
支付宝提供了DSA、RSA、MD5三种签名方式，本次开发中，我们使用RSA签名和加密，那就只配置RSA密钥就好了。

关于RSA加密的详解，参见《支付宝签名与验签》。

## 应用环境
本节可以忽略，本节可以忽略，本节可以忽略！因为官方文档并没有提及应用环境配置的问题。

进入管理中心，对应用进行设置。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/alipay-process/nodeforum.jpg?imageView2/0/w/700)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/alipay-process/config.jpg)
上图是我的应用配置选项，公司账号也许会有所不同。具体哪些参数需要配置？请参照[接口参数说明](https://doc.open.alipay.com/doc2/detail.htm?spm=a219a.7629140.0.0.J6vq89&treeId=60&articleId=104790&docType=1)，需要什么就配置什么。

# 交互
## Node端发起支付请求
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/alipay-process/node%E7%AB%AF%E5%8F%91%E8%B5%B7%E8%AF%B7%E6%B1%82.jpg)
我们公司采用的，就是这种方式，步骤3中Node端获取到的支付宝参数，包括sign。其实，sign的计算工作也可以放在Node端，只不过支付宝没有给出Node的demo，实现起来需要耗费多一点时间。

## 后端发起支付请求
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/alipay-process/%E5%90%8E%E7%AB%AF%E5%8F%91%E8%B5%B7%E8%AF%B7%E6%B1%82.jpg)
这种方式也很好，而且，步骤4中后端获取到支付页面，也可以不传给Node端，自己显示出来。这样，整个流程就更加简单。

## return_url和notify_url
return_url，支付完成后的回调url；notify_url，支付完成后通知的url。支付宝发送给两个url的参数是一样的，只不过一个是get，一个是post。

以上两种发起请求的方式中，return_url在Node端，notify_url在后端。我们也可以根据需要，把两个url都放在后端，或者都放在Node端，改变相应业务逻辑即可。

# Node端详解
Node端发起支付请求有两种选择，一种是获取到后端给的参数后，通过request模块发起get请求，获取到支付宝返回的支付页面，然后显示到页面上；另一种是获取到后端给的参数后，把参数全部输出到页面中的form表单，然后通过js自动提交表单，获取到支付宝返回的支付页面（同时显示出来）。

## request发起请求
```
// 通过orderId向后端请求获取支付宝支付参数alidata
var alipayUrl = 'https://mapi.alipay.com/gateway.do?'+ alidata;
request.get({url: alipayUrl},function(error, response, body){
    res.send(response.body);
});
```

理论上完全正确的请求，然而，获取到的支付页面，输出到页面上，却是乱码。没错，还是一个错误提示页面。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/alipay-process/%E4%B8%8D%E6%AD%A3%E5%B8%B8.jpg)

神奇的地方在于，在刷新页面多次后，正常了！！！啊嘞，这是什么鬼？

![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/alipay-process/%E6%AD%A3%E5%B8%B8.jpg)

先解决乱码问题，看看报什么错！
```
request.get({url: alipayUrl},function(error, response, body){
    var str = response2.body;
    str = str.replace(/gb2312/, "utf-8");
    res.setHeader('content-type', 'text/html;charset=utf-8');
    res.send(str);
});
```

很遗憾，无效！乱码依然是乱码。。。和沈晨帅哥讨论很久，最终决定换一种方案——利用表单提交。

## 表单提交请求

### Node端
```
// node端

// 通过orderId向后端请求获取支付宝支付参数alidata
function getArg(str,arg) {
  var reg = new RegExp('(^|&)' + arg + '=([^&]*)(&|$)', 'i');
  var r = str.match(reg);
  if (r != null) {
      return unescape(r[2]);
  }
  return null;
}

var alipayParam = {
  _input_charset: getArg(alidata,'_input_charset'),
  body: getArg(alidata,'body'),
  it_b_pay: getArg(alidata, 'it_b_pay'),
  notify_url: getArg(alidata, 'notify_url'),
  out_trade_no: getArg(alidata, 'out_trade_no'),
  partner: getArg(alidata, 'partner'),
  payment_type: getArg(alidata, 'payment_type'),
  return_url: getArg(alidata, 'return_url'),
  seller_id: getArg(alidata, 'seller_id'),
  service: getArg(alidata, 'service'),
  show_url: getArg(alidata, 'show_url'),
  subject: getArg(alidata, 'subject'),
  total_fee: getArg(alidata, 'total_fee'),
  sign_type: getArg(alidata, 'sign_type'),
  sign: getArg(alidata, 'sign'),
  app_pay: getArg(alidata, 'app_pay')
};

res.render('artist/alipay',{
  // 其他参数
  alipayParam: alipayParam
});

```

### html

```
<!--alipay.html-->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>支付宝支付</title>
</head>
<body>
    <form id="ali-form" action="https://mapi.alipay.com/gateway.do" method="get">   
        <input type="hidden" name="_input_charset" value="<%= alipayParam._input_charset%>">
        <input type="hidden" name="body" value="<%= alipayParam.body%>">
        <input type="hidden" name="it_b_pay" value="<%= alipayParam.it_b_pay%>">
        <input type="hidden" name="notify_url" value="<%= alipayParam.notify_url%>">
        <input type="hidden" name="out_trade_no" value="<%= alipayParam.out_trade_no%>">
        <input type="hidden" name="partner" value="<%= alipayParam.partner%>">
        <input type="hidden" name="payment_type" value="<%= alipayParam.payment_type%>">
        <input type="hidden" name="return_url" value="<%= alipayParam.return_url%>">
        <input type="hidden" name="seller_id" value="<%= alipayParam.seller_id%>">
        <input type="hidden" name="service" value="<%= alipayParam.service%>">
        <input type="hidden" name="show_url" value="<%= alipayParam.show_url%>">
        <input type="hidden" name="subject" value="<%= alipayParam.subject%>">
        <input type="hidden" name="total_fee" value="<%= alipayParam.total_fee%>">
        <input type="hidden" name="sign_type" value="<%= alipayParam.sign_type%>">
        <input type="hidden" name="sign" value="<%= alipayParam.sign%>">
        <input type="hidden" name="app_pay" value="<%= alipayParam.app_pay%>">
    </form>
<% include ../bootstrap.html %>
<script type="text/javascript" src="<%= dist %>/js/business-modules/artist/alipay.js?v=2016052401"></script>
</body>
</html>
```

### js

```
// alipay.js
seajs.use(['zepto'],function($){
    var index = {
        init:function(){
            var self = this;
            this.bindEvent();
        },
        bindEvent:function(){
            var self = this;
            $('#ali-form').submit();
        }
    }
    index.init();
});
```

### 小结
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/alipay-process/success.jpg)
选择支付宝支付后，成功跳转到了支付宝支付页面，nice！看来这种方案很靠谱。

开始时，打算把alidata直接输出到form表单的action中接口的后面，因为这样似乎最简便。但是，提交表单时，后面的参数全部被丢弃了。所以，不得不得把所有参数放在form表单中。Node端拆分了一下参数，组装成了一个alipayParam对象，这个工作也可以交给后端来做。

显然，request模拟表单提交和真实表单提交结果的不同，得出的结论是，request并不能完全模拟表单提交。或者，request可以模拟，而我不会-_-|||。

## 错误排查
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/alipay-process/error.jpg)
值得点赞的是，支付宝给的错误代码很明确，一看就懂。上面这个错误，签名不对。因为我给alipayParam私自加了个app_pay属性，没有经过签名。

# 微信屏蔽支付宝
## 问题描述
以上，大功告成？不！还有一个坑要填，因为微信屏蔽了支付宝！
在电脑上，跳转支付宝支付页面正常，很完美！然而，在微信浏览器中测试时，却没有跳转，而是出现如下信息。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/alipay-process/%E5%B1%8F%E8%94%BD.jpg?imageView2/0/w/600)

## 完美解决办法
微信端支付宝支付，iframe改造
http://www.cnblogs.com/jiqing9006/p/5584268.html

该办法的核心在于：把微信屏蔽的链接，赋值给iframe的src属性。

### Node端
```
res.render('artist/alipay',{
  alipayParam: alipayParam,
  param: urlencode(alidata) 
});
```

### html
```
<input type="hidden" id="param" value="<%= param%>">
<iframe id="payFrame" name="mainIframe" src="" frameborder="0" scrolling="auto" ></iframe>
```

### js
```
var iframe = document.getElementById('payFrame');
var param = $('#param').val();
iframe.src='https://mapi.alipay.com/gateway.do?'+param;
```

### 报错
然而，在改造时，先是报错ILLEGAL_SIGN，于是利用urlencode处理了字符串。接着，又报错ILLEGAL_EXTERFACE，没有找到解决办法。

暂时放弃，以后如果有了解决办法再补上。

## 官方解决办法
关于微信公众平台无法使用支付宝收付款的解决方案说明
https://cshall.alipay.com/enterprise/help_detail.htm?help_id=524702

该方法的核心在于：确认支付时，提示用户打开外部系统浏览器，在系统浏览器中支付。

### html
```
<!--alipay.html-->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>支付宝支付</title>
</head>
<body>
  <form id="ali-form" action="https://mapi.alipay.com/gateway.do" method="get"> 
    <input type="hidden" name="_input_charset" value="<%= alipayParam._input_charset%>">
    <input type="hidden" name="body" value="<%= alipayParam.body%>">
    <input type="hidden" name="it_b_pay" value="<%= alipayParam.it_b_pay%>">
    <input type="hidden" name="notify_url" value="<%= alipayParam.notify_url%>">
    <input type="hidden" name="out_trade_no" value="<%= alipayParam.out_trade_no%>">
    <input type="hidden" name="partner" value="<%= alipayParam.partner%>">
    <input type="hidden" name="payment_type" value="<%= alipayParam.payment_type%>">
    <input type="hidden" name="return_url" value="<%= alipayParam.return_url%>">
    <input type="hidden" name="seller_id" value="<%= alipayParam.seller_id%>">
    <input type="hidden" name="service" value="<%= alipayParam.service%>">
    <input type="hidden" name="show_url" value="<%= alipayParam.show_url%>">
    <input type="hidden" name="subject" value="<%= alipayParam.subject%>">
    <input type="hidden" name="total_fee" value="<%= alipayParam.total_fee%>">
    <input type="hidden" name="sign_type" value="<%= alipayParam.sign_type%>">
    <input type="hidden" name="sign" value="<%= alipayParam.sign%>">
    <div class="wrapper buy-wrapper" style="display: none;">
        <a href="javascript:void(0);"
           class="J-btn-submit btn mj-submit btn-strong btn-larger btn-block">确认支付</a>
    </div>
  </form>
<input type="hidden" id="param" value="<%= param%>">
<% include ../bootstrap.html %>
<script type="text/javascript" src="<%= dist %>/js/business-modules/artist/ap.js"></script>
<script>
    var btn = document.querySelector(".J-btn-submit");
    btn.addEventListener("click", function (e) {
        e.preventDefault();
        e.stopPropagation();
        e.stopImmediatePropagation();

        var queryParam = '';

        Array.prototype.slice.call(document.querySelectorAll("input[type=hidden]")).forEach(function (ele) {
            queryParam += ele.name + "=" + encodeURIComponent(ele.value) + '&';
        });
        var gotoUrl = document.querySelector("#ali-form").getAttribute('action') + '?' + queryParam;
        _AP.pay(gotoUrl);

        return false;
    }, false);
    btn.click();
</script>
</body>
</html>
```

该页面会自动跳转到同一文件夹下的pay.htm，该文件官方已经提供，把其中的引入ap.js的路径修改一下即可。最终效果如下：
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/alipay-process/%E6%8F%90%E7%A4%BA.jpg?imageView2/0/w/600)


# 后记
支付宝的支付流程和微信的支付流程，有很多相似之处。沈晨指出一点不同：支付宝支付完成后有return_url和notify_url；微信支付完成后只有notify_url。

研读了一下微信支付的开发文档，确实如此。微信支付完成后的通知也分成两路，一路通知到notify_url，另一路返回给调用支付接口的JS，同时发微信消息提示。也就是说，跳转return_url的工作我们需要自己做。

最后，感谢沈晨帅哥提供的思路和帮助，感谢体超帅哥辛苦改后端。

# 书签
支付宝开放平台
https://openhome.alipay.com/platform/home.htm

支付宝开放平台-手机网站支付-文档中心
https://doc.open.alipay.com/doc2/detail?treeId=60&articleId=103564&docType=1

支付宝WAP支付接口开发
http://blog.csdn.net/tspangle/article/details/39932963

[wap h5手机网站支付接口唤起支付宝钱包付款](http://jingyan.baidu.com/article/8ebacdf016222249f65cd532.html?qq-pf-to=pcqq.c2c)

商家服务 - 支付宝 知托付！
https://b.alipay.com/order/techService.htm

微信支付-开发文档
https://pay.weixin.qq.com/wiki/doc/api/jsapi.php?chapter=7_1

微信端支付宝支付，iframe改造
http://www.cnblogs.com/jiqing9006/p/5584268.html

微信如何突破支付宝的封锁
http://blog.csdn.net/lee_sire/article/details/49530875

JavaScript专题（二）：深入理解iframe
http://www.cnblogs.com/fangjins/archive/2012/08/19/2645631.html

关于微信公众平台无法使用支付宝收付款的解决方案说明
https://cshall.alipay.com/enterprise/help_detail.htm?help_id=524702