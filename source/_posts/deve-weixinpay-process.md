---
title: 微信公众号支付流程
toc: true
date: 2016-08-11 20:32:49
tags:
- 微信
categories: 设计开发
---
# 前言
花费了一天时间，调通了微信公众号支付。作下记录，方便以后再次填坑。先声明，微信公众号支付，不同于微信H5支付，这点在本文结束时再详细说明。

<!--more-->

# 微信配置
## 设置测试目录
在微信公众平台设置，栏目见下图。支付测试状态下，设置测试目录，测试人的微信号添加到白名单，发起支付的页面目录必须与设置的精确匹配。并将支付链接发到对应的公众号会话窗口中才能正常发起支付测试。注意正式目录一定不能与测试目录设置成一样，否则支付会出错。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/weixinpay-process/%E6%B5%8B%E8%AF%95%E7%9B%AE%E5%BD%95.png)

## 设置正式支付目录
根据图中栏目顺序进入修改栏目，勾选JSAPI网页支付开通该权限，并配置好支付授权目录，该目录必须是发起支付的页面的精确目录，子目录下无法正常调用支付。具体界面如图所示：
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/weixinpay-process/%E6%AD%A3%E5%BC%8F%E7%9B%AE%E5%BD%95.png)

# 交互流程
## 业务流程时序图
首先看下微信官方给出的交互流程：
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/weixinpay-process/%E6%97%B6%E5%BA%8F%E5%9B%BE.png)

## 服务器交互
微信公众号支付的流程，简而言之只有两步。第一步，下单，拿到prepay_id等信息；第二步，利用第一步拿到的prepay_id等信息进行支付。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/weixinpay-process/flowchart.jpg)

# 代码

## Node端
```
// 微信支付
exports.weixinpay = function(req, res){
  var orderId = req.params.orderId;
  var selectSum = req.params.selectSum;
  var token = req.params.token;
  // 获取code
  if(!req.query.code){
    var r_url = 'http://' +config.host+'/artist/weixinpay/'+orderId+'/'+selectSum+'/'+token;
    var url = 'https://open.weixin.qq.com/connect/oauth2/authorize?appid='+config.weixin.appid+'&redirect_uri='+urlencode(r_url)+'&response_type=code&scope=snsapi_userinfo&state=111#wechat_redirect';
    res.redirect(url);
  }else{
    var code = req.query.code;
    var state = req.query.state;

    var ep = new eventproxy();
    ep.all('userInfo',function(userInfoData){
      var userInfoData = JSON.parse(userInfoData);
      res.render('artist/weixinpay',{
        // 其他数据
        openid: userInfoData.openid,
        orderId: orderId,
        selectSum: selectSum,
        token: token
      });
    });

    // 获取用户信息userInfo
  }
}

exports.weixinpay2 = function(req, res){
  var wxParamUrl = config.apihost + '/order/getH5SubOrderforWechat';
  var param = {
    orderId: req.query.orderId,
    selectSum: req.query.selectSum,
    accessToken: req.query.token,
    openId: req.query.openid
  };
  request.post({url: wxParamUrl,form: JSON.stringify(param)},function(error, response, body){    
    res.render('artist/weixinpay2',{
      // 其他数据
      wxParam: JSON.parse(response.body)
    });
  });
}
```

上面的代码看起来很奇怪，为什么需要一个weixinpay2函数？代码全部放在weixinpay函数里面，然后在weixinpay.html里调用微信支付接口，不是很好么？这个，就涉及到支付目录的问题了。weixinpay函数中，微信授权获取用户信息后的回调url，目录太深，而且不确定，因为我们是通过目录来传递参数的。这样，就无法在微信公众号上面设置支付授权目录。所以，我们不能在weixinpay.html这个页面发起支付请求，只能把参数转发给下一个weixinpay2.html页面，在weixinpap2.html中发起支付请求。

那么，为什么获取用户信息后的回调url目录太深？直接把orderId等信息当做参数放在回调url后面不就可以了吗？很遗憾，回调url无法带入你的自定义参数。因此，只能把参数当做回调url的一部分，也就是目录的一部分。

综上，weixinpay函数负责获取用户的openid，weixinpay.html负责跳转weixinpay2.html；weixinpay2函数负责获取调用微信JSAPI的参数，weixinpay2.html负责调用微信JSAPI。

## html
```
<!--weixinpay.html-->
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>微信支付</title>
</head>
<body>
    <a id="weixin-link" href="/artist/weixinpay2?orderId=<%= orderId%>&selectSum=<%= selectSum%>&token=<%= token%>&openid=<%= openid%>" style="display: none">微信支付</a>

<script>
    var link = document.getElementById('weixin-link');
    link.click();
</script>
</body>
</html>
```

```
<!--weixinpay2.html-->
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>微信支付</title>
</head>
<body>
    <input id="appId" type="hidden" value="<%= wxParam.obj.appid%>">
    <input id="timeStamp" type="hidden" value="<%= wxParam.obj.timestamp%>">
    <input id="nonceStr" type="hidden" value="<%= wxParam.obj.noncestr%>">
    <input id="package" type="hidden" value="<%= wxParam.obj.packagestr%>">
    <input id="signType" type="hidden" value="MD5">
    <input id="paySign" type="hidden" value="<%= wxParam.obj.sign%>">

<script>
function onBridgeReady(){
    var appId = document.getElementById('appId').value;
    var timeStamp = document.getElementById('timeStamp').value;
    var nonceStr = document.getElementById('nonceStr').value;
    var package = document.getElementById('package').value;
    var signType = document.getElementById('signType').value;
    var paySign = document.getElementById('paySign').value;
    WeixinJSBridge.invoke(
        'getBrandWCPayRequest', {
            "appId": appId,     //公众号名称，由商户传入     
            "timeStamp":timeStamp,         //时间戳，自1970年以来的秒数     
            "nonceStr": nonceStr, //随机串     
            "package": package,     
            "signType": signType,         //微信签名方式：     
            "paySign": paySign //微信签名 
        },
        function(res){
            WeixinJSBridge.log(res.err_msg);
            //alert(res.err_code + res.err_desc + res.err_msg);
            if (res.err_msg == "get_brand_wcpay_request:ok") {  
                window.location.href = '/artist/alipayresult?trade_status=TRADE_SUCCESS';
                // 执行跳转页面....  
            } else if (res.err_msg == "get_brand_wcpay_request:cancel") {  
                alert ("用户取消支付!");  
            } else {  
                alert ("支付失败!");  
            }  
            
        }
    ); 
}

if (typeof WeixinJSBridge == "undefined"){
    if( document.addEventListener ){
        document.addEventListener('WeixinJSBridgeReady', onBridgeReady, false);
    }else if (document.attachEvent){
        document.attachEvent('WeixinJSBridgeReady', onBridgeReady); 
        document.attachEvent('onWeixinJSBridgeReady', onBridgeReady);
    }
}else{
    onBridgeReady();
}
</script>
</body>
</html>
```

为了加快执行速度，这里的两个页面中，都使用原生的js来实现页面逻辑。

# 错误
## 支付验证签名失败
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/weixinpay-process/signerror.jpg?imageView2/0/w/600)
很明显，支付验证签名失败。怎么判断算出的签名是否正确？使用微信支付[接口签名校验工具](https://pay.weixin.qq.com/wiki/doc/api/jsapi.php?chapter=20_1)。
校验方式选择自定义参数，把需要签名的参数名称和参数值输入页面，点击生成签名即可。

## 当前页面的URL未注册
不好意思，这个错误忘记截图了。借来了一张图，凑合着看。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/weixinpay-process/urlerror.jpg?imageView2/0/w/600)
这个问题，是由于调用支付接口的url不对！那么，怎样是对的呢？举个栗子：如果调用支付接口的url为`http://wx.voidking.com/pay/weixinpay`，那么微信公众平台上的支付授权目录应该设置为`http://wx.voidking.com/pay/`。也就是说，支付授权目录应该设置为调用支付接口的url的上一级目录。

# 支付成功
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/weixinpay-process/success.png?imageView2/0/w/600)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/weixinpay-process/success.jpg?imageView2/0/w/600)

# 后记
微信公众号支付，两个缺点，一是必须在微信浏览器使用，二是必须有一个拉取用户信息的步骤。但是我们发现，很多网站，在其他浏览器也可以使用微信支付，也不需要拉取用户信息，这是怎么回事？

因为，这是两种不同的支付方式！今天我们讨论的，叫做微信公众号支付；而在其他浏览器中调用微信支付，叫做微信H5支付！

更多关于微信H5支付的内容，请参考[微信H5支付官方文档](https://pay.weixin.qq.com/wiki/doc/api/wap.php?chapter=15_1)。

# 书签
微信支付开发文档——公众号支付
https://pay.weixin.qq.com/wiki/doc/api/jsapi.php?chapter=7_1

微信支付接口签名校验工具
https://pay.weixin.qq.com/wiki/doc/api/jsapi.php?chapter=20_1

非微信内置浏览器中的网页调起微信支付的方案研究
http://blog.csdn.net/ahence/article/details/51317814

微信支付|商户平台开发者文档
https://pay.weixin.qq.com/wiki/doc/api/wap.php?chapter=15_1

【微信支付V2.0】H5支付实例
http://wxpay.weixin.qq.com/pub_v2/pay/wap.v2.php
