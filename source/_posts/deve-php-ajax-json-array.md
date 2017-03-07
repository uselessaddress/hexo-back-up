---
title: PHP接收ajax的json数组
toc: true
date: 2017-03-03 19:00:00
tags:
- php
- ajax
categories: 设计开发
---
# 前言
前端和PHP端（服务器）通过ajax进行数据交互时，数据格式一般为字符串、数组、json、json数组。下面，我们针对这四种数据格式，进行前端和PHP端模拟交互，寻找一些规律和结论。

<!--more-->

# 前端
```
var data = {
    product_name: '毛巾',
    product_arr: ['毛巾','肥皂'],
    product_json: {
        product_name: '毛巾',
        product_num: '5',
        product_unit: '条'
    },
    product_json_arr: [{
        product_name: '毛巾',
        product_num: '5',
        product_unit: '条'
    },{
        product_name: '肥皂',
        product_num: '10',
        product_unit: '块'
    }]
}

console.log(data);

$.ajax({
    url: '/',
    type: 'POST',
    dataType: 'json',
    data: data,
    success: function(data){
        console.log(data);
    },
    error: function(xhr){
        console.log(xhr);
    }
});
```

# PHP端
## 接收字符串
```
$product_name = $_POST['product_name'];
echo json_encode($product_name,JSON_UNESCAPED_UNICODE);
```
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/php-ajax-json-array/string.jpg)
由结果看出，PHP端获取到了product_name。

## 接收数组
```
$product_arr = $_POST['product_arr'];
echo json_encode($product_arr,JSON_UNESCAPED_UNICODE);
```
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/php-ajax-json-array/array.jpg)
由结果看出，PHP端获取到了product_arr。

```
$product_arr = $_POST['product_arr'];
echo json_encode($product_arr[0],JSON_UNESCAPED_UNICODE);
```
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/php-ajax-json-array/array2.jpg)
由结果看出，PHP端拿到的确实是数组。

## 接收json
```
$product_json = $_POST['product_json'];
echo json_encode($product_json,JSON_UNESCAPED_UNICODE);
```
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/php-ajax-json-array/json.jpg)
由结果看出，PHP获取到了product_json。

```
$product_json = $_POST['product_json'];
echo json_encode($product_json->product_name,JSON_UNESCAPED_UNICODE);
```
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/php-ajax-json-array/json2.jpg)
根据返回结果，猜测PHP端拿到的json数据是字符串。

```
$product_json = json_decode($_POST['product_json']);
echo json_encode($product_json->product_name,JSON_UNESCAPED_UNICODE);
```
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/php-ajax-json-array/json3.jpg)
根据返回结果，证明PHP端拿到的json数据不是字符串。因为json字符串通过json_decode函数是可以转换成json格式数据的，而此处的json数据转换失败。

后端已经没有办法再变化，我们调整前端数据格式，把json数据先转化成字符串再传输。
```
var data = {
    product_json: JSON.stringify({
        product_name: '毛巾',
        product_num: '5',
        product_unit: '条'
    })
}
```
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/php-ajax-json-array/json4.jpg)
根据返回结果，证明前端json数据必须先转换成字符串，然后PHP端可以获取json字符串，接着正常处理即可。

## json数组
```
$product_json_arr = $_POST['product_json_arr'];  
echo json_encode($product_json_arr,JSON_UNESCAPED_UNICODE);
```
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/php-ajax-json-array/json-array.jpg)
由结果看出，PHP端拿到了product_json_arr。

```
$product_json_arr = $_POST['product_json_arr'];  
echo json_encode($product_json_arr[0],JSON_UNESCAPED_UNICODE);
```
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/php-ajax-json-array/json-array2.jpg)
由结果看出，PHP端拿到的确实是数组。

```
$product_json_arr = $_POST['product_json_arr']; 
echo json_encode($product_json_arr[0]->product_name,JSON_UNESCAPED_UNICODE);
```
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/php-ajax-json-array/json-array3.jpg)
由结果看出，PHP端拿到的是数组，但是数组里的并不是json数据。

```
$product_json_arr = json_decode($_POST['product_json_arr'],true);
echo json_encode($product_json_arr[0]->product_name,JSON_UNESCAPED_UNICODE);
```
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/php-ajax-json-array/json-array4.jpg)
由结果看出，数组里的json数据不是字符串，猜测这里也需要前端先转json字符串。

调整前端数据格式，把json数组数据先转化成字符串再传输。
```
var data = {
    product_json_arr: JSON.stringify([{
        product_name: '毛巾',
        product_num: '5',
        product_unit: '条'
    },{
        product_name: '肥皂',
        product_num: '10',
        product_unit: '块'
    }])
}

```

![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/php-ajax-json-array/json-array5.jpg)
由结果看出，依然不行。

再次修改后端代码：
```
$product_json_arr = json_decode($_POST['product_json_arr']);
echo json_encode($product_json_arr[0]->product_name,JSON_UNESCAPED_UNICODE);
```
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/php-ajax-json-array/json-array6.jpg)
输出成功，也就是说，问题出在json_decode上面。json_decode($str, true) 可以得到数组，第二参数不加默认为false，得到对象。而json数组出现时，要选择生成对象。

# 小结
通过上面的实验，我们可以得出结论：涉及到json格式的数据，需要在前端先使用JSON.stringify()函数转换为字符串；然后，在PHP端通过json_decode()函数转换为对象。




