---
title: thinkphp实现短信验证注册
toc: true
date: 2016-10-14 18:00:01
tags:
- thinkphp
- php
- angular
- 短信
- 云片
- 验证码
categories: 设计开发
---
# 前言
小太阳项目最先做的模块，用户管理模块。业主端该模块分为三个功能：注册登录、个人信息修改、认证。
注册时需要用到短信验证码，本文记录一下思路和具体实现。
短信验证平台使用云片，短信验证码的生成使用thinkphp。

<!--more-->

# 思路
1、用户输入手机号，请求获取短信验证码。
2、thinkphp生成短信验证码，存储，同时和其他参数一起发送请求给云片。
3、云片发送短信验证码到指定手机号。
4、用户输入短信验证码。
5、thinkphp根据验证码是否正确、验证码是否过期两个条件判断是否验证通过。


# 代码实现
## 验证接口
接口地址：`https://sms.yunpian.com/v1/sms/send.json`。
使用postman，输入三个必须的参数`apikey`、`mobile`和`text`。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/sms-vertification-code/%E6%8E%A5%E5%8F%A3%E6%B5%8B%E8%AF%95.jpg?imageView2/0/w/700)

## php发起http/https请求
使用php的curl函数发起https请求，带入参数`apikey`、`mobile`和`text`。
```
// 获取短信验证码
public function getSMSCode(){

    // create curl resource 
    $ch = curl_init(); 

    // set url
    $url = 'https://sms.yunpian.com/v1/sms/send.json'; 
    curl_setopt($ch, CURLOPT_URL, $url); 

    // set param
    $paramArr = array(
        'apikey' => '******',
        'mobile' => '******',
        'text' => '【小太阳】您的验证码是1234'
    );
    $param = '';
    foreach ($paramArr as $key => $value) {
        $param .= urlencode($key).'='.urlencode($value).'&';
    }
    $param = substr($param, 0, strlen($param)-1);

    curl_setopt($ch, CURLOPT_POSTFIELDS, $param);
    curl_setopt($ch, CURLOPT_HEADER, 0);
    curl_setopt($ch, CURLOPT_POST, 1);

    //curl默认不支持https协议，设置不验证协议
    curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false); 
    curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, false); 

    //return the transfer as a string 
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1); 

    // $output contains the output string 
    $output = curl_exec($ch); 

    // close curl resource to free up system resources 
    curl_close($ch); 

    echo $output;
}
```


## 生成随机短信验证码
默认生成四位的随机短信验证码。
```
// 生成短信验证码
public function createSMSCode($length = 4){
    $min = pow(10 , ($length - 1));
    $max = pow(10, $length) - 1;
    return rand($min, $max);
}
```

## 整合
在数据库新建表sun_smscode：
```
DROP TABLE IF EXISTS `sun_smscode`;
CREATE TABLE `sun_smscode` (
  `id` int(8) NOT NULL AUTO_INCREMENT,
  `mobile` varchar(11) NOT NULL,
  `code` int(4) NOT NULL,
  `create_at` datetime NOT NULL,
  `update_at` datetime NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=MyISAM AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;
```

thinkphp代码：
```
// 获取短信验证码
public function getSMSCode(){

    // create curl resource 
    $ch = curl_init(); 

    // set url
    $url = 'https://sms.yunpian.com/v1/sms/send.json'; 
    curl_setopt($ch, CURLOPT_URL, $url); 

    // set param
    $mobile = $_POST['mobile'];
    $code = $this->createSMSCode();
    $paramArr = array(
        'apikey' => '******',
        'mobile' => $mobile,
        'text' => '【小太阳】您的验证码是'.$code
    );
    $param = '';
    foreach ($paramArr as $key => $value) {
        $param .= urlencode($key).'='.urlencode($value).'&';
    }
    $param = substr($param, 0, strlen($param)-1);

    curl_setopt($ch, CURLOPT_POSTFIELDS, $param);
    curl_setopt($ch, CURLOPT_HEADER, 0);
    curl_setopt($ch, CURLOPT_POST, 1);
    curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false); //不验证证书下同
    curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, false); 

    //return the transfer as a string 
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1); 

    // $output contains the output string 
    $output = curl_exec($ch); 

    // close curl resource to free up system resources 
    curl_close($ch); 
    //$outputJson = json_decode($output);
    $outputArr = json_decode($output, true);
    //echo $outputJson->code;
    //echo $outputArr['code'];

    if($outputArr['code'] == '0'){
        $data['mobile'] = $mobile;
        $data['code'] = $code;

        $smscode = D('smscode');
        $smscodeObj = $smscode->where("mobile='$mobile'")->find();
        if($smscodeObj){
            $data['update_at'] = date('Y-m-d H:i:s');
            $success = $smscode->where("mobile='$mobile'")->save($data);
            if($success !== false){
                $result = array(
                    'code' => '0',
                    'ext' => '修改成功',
                    'obj' => $smscodeObj
                );
            }
            echo json_encode($result,JSON_UNESCAPED_UNICODE);
        }else{
            $data['create_at'] = date('Y-m-d H:i:s');
            $data['update_at'] = $data['create_at'];
            if($smscode->create($data)){
                $id = $smscode->add();
                if($id){
                    $smscode_temp = $smscode->where("id='$id'")->find();
                    $result = array(
                        'code'=> '0',
                        'ext'=> '创建成功',
                        'obj'=>$smscode_temp
                    );
                    echo json_encode($result,JSON_UNESCAPED_UNICODE);
                }
            }
        }
        
    }
}
```

## 验证短信验证码
验证短信验证码时间是否过期，验证短信验证码是否正确。
```
// 验证短信验证码是否有效
public function checkSMSCode(){
    $mobile = $_POST['mobile'];
    $code = $_POST['code'];
    $nowTimeStr = date('Y-m-d H:i:s');

    $smscode = D('smscode');
    $smscodeObj = $smscode->where("mobile='$mobile'")->find();
    if($smscodeObj){
        $smsCodeTimeStr = $smscodeObj['update_at'];
        $recordCode = $smscodeObj['code'];
        $flag = $this->checkTime($nowTimeStr, $smsCodeTimeStr);
        if(!$flag){
            $result = array(
                'code' => '1',
                'ext' => '验证码过期，请刷新后重新获取'
            );
            echo json_encode($result,JSON_UNESCAPED_UNICODE);
            return;
        }

        if($code != $recordCode){
            $result = array(
                'code' => '2',
                'ext' => '验证码错误，请重新输入'
            );
            echo json_encode($result,JSON_UNESCAPED_UNICODE);
            return;
        }

        $result = array(
            'code' => '0',
            'ext' => '验证通过'
        );
        echo json_encode($result,JSON_UNESCAPED_UNICODE);
    }
}

// 验证验证码时间是否过期
public function checkTime($nowTimeStr,$smsCodeTimeStr){
    //$nowTimeStr = '2016-10-15 14:39:59';
    //$smsCodeTimeStr = '2016-10-15 14:30:00';
    $nowTime = strtotime($nowTimeStr);
    $smsCodeTime = strtotime($smsCodeTimeStr);
    $period = floor(($nowTime-$smsCodeTime)/60); //60s
    if($period>=0 && $period<=20){
        return true;
    }else{
        return false;
    }
}
```


# 改进
为了防止短信轰炸，在请求获取短信验证码时，需要加入图片验证码。

thinkphp提供了生成图片验证码的函数，下面我们来实现验证码的生成、刷新和验证。
## 生成和刷新图片验证码
```
// 获取图片验证码，刷新图片验证码
public function getPicCode(){
    $config = array(
        'fontSize'=>30,    // 验证码字体大小
        'length'=>4,     // 验证码位数
        'useNoise'=>false, // 关闭验证码杂点
        'expire'=>600
    );
    $Verify = new \Think\Verify($config);
    $Verify->entry(2333);//2333是验证码标志
}
```

假设，该函数的对应url为`http://localhost/owner-bd/index.php/Home/CheckCode/getPicCode`，那么，图片验证码的地址就是这个url，放入页面图片标签的src属性即可。

## 验证图片验证码
```
// 验证验证码是否正确
public function checkPicCode($code){
    $verify = new \Think\Verify();
    if($verify->check($code, 2333)){
        $result = array(
            'code' => '0',
            'ext' => '验证通过'
        );
        echo json_encode($result,JSON_UNESCAPED_UNICODE);
    }else{
        $result = array(
            'code' => '1',
            'ext' => '验证码错误，请重新输入'
        );
        echo json_encode($result,JSON_UNESCAPED_UNICODE);
    };
}
```
以上方法，我们利用了thinkphp提供的check方法，实现起来很简单。但是，如果想要得到验证细节，就没有办法了。比如，验证码错误，可能验证码超时，可能因为输入验证码错误，可能因为验证码已经使用过等等。必要的时候，可以重写thinkphp的验证码类，或者重写thinkphp的check方法。

# 跑通前后端
## 后端修改
验证图片验证码函数，改为被调用函数：
```
public function checkPicCode($picCode){
    $verify = new \Think\Verify();
    if($verify->check($picCode, 2333)){
        return true;
    }else{
        return false;
    };
}
```

在获取短信验证码函数的最顶部，添加调用图片验证码函数，只有通过验证，才发送请求给云片。
```
// 获取短信验证码
public function getSMSCode(){
    $picCode = $_POST['picCode'];
    if(!$this->checkPicCode($picCode)){
        $result = array(
            'code' => '1',
            'ext' => '验证码错误，请重新输入'
        );
        echo json_encode($result,JSON_UNESCAPED_UNICODE);
        return;
    }
    /*省略*/
}
```

## 前端核心代码
```
<!--register.html-->
<!DOCTYPE html>
<html lang="zh" ng-app="sunApp">
<head>
    <meta charset="UTF-8">
    <title>注册</title>
</head>
<body ng-controller="registerController">
    <form action="" class="register-form" ng-show="isShow1">
        <div class="input-group">
            <input type="text" class="mobile" ng-model="mobile" placeholder="手机号">
        </div>
        <div class="input-group">
            <input type="text" class="pic-code" ng-model="picCode" placeholder="图片验证码">
            <img class="img" src="{{picCodeUrl}}" alt="" ng-click="refresh()">
        </div>
        <div class="input-group">
            <input type="text" class="sms-code" ng-model="SMSCode" placeholder="短信验证码">
            <button class="btn-sms" ng-click="getSMSCode()" ng-disabled="btnSMSDisabled">{{btnSMSText}}</button>
        </div>
        <button class="confirm-btn" ng-click="next()">下一步</button>
    </form>

    <form action="" class="register-form" ng-show="isShow2">
        <div class="input-group">
            <input type="text" class="mobile" ng-model="mobile" placeholder="手机号" disabled="true">
        </div>
        <div class="input-group">
            <input type="password" class="password" ng-model="password" placeholder="请输入密码">
            <input type="password" class="password" ng-model="password2" placeholder="请再次输入密码">
        </div>
        <button class="confirm-btn" ng-click="getSMSCode()">注册</button>
    </form>
</body>
</html>
```


```
// register.js
angular.module('sunApp').controller('registerController', function ($scope,$http,$httpParamSerializer,$state,$interval) { 
    $scope.picCodeUrl = '/owner-bd/index.php/Home/CheckCode/getPicCode';
    $scope.isShow1 = true;
    $scope.isShow2 = false;
    $scope.btnSMSText = '获取验证码';
    $scope.btnSMSDisabled = false;
    $scope.checkOver = false;

    // 获取短信验证码
    $scope.getSMSCode = function(){
        var param = {
            mobile: $scope.mobile,
            picCode: $scope.picCode
        };
        $http({
            method:'POST',
            url:'/owner-bd/index.php/Home/SMS/getSMSCode',
            //url: '/owner-fd/mock/common.json',
            headers:{
                'Content-Type':'application/x-www-form-urlencoded'
            },
            dataType: 'json',
            data: $httpParamSerializer(param)
        }).then(function successCallback(response) {
            console.log(response.data);
            if(response.data.code == '0'){
                $scope.checkOver = true;
                $scope.btnSMSDisabled = true;
                var time = 60;
                var timer = null;
                timer = $interval(function(){
                    time = time - 1;
                    $scope.btnSMSText = time+'秒';
                    if(time == 0) {
                        $interval.cancel(timer);
                        $scope.btnSMSDisabled = false;
                        $scope.btnSMSText = '重新获取';
                    }
                }, 1000);
            }
        }, function errorCallback(response) {
            console.log(response.data);
        });
    }

    // 验证短信验证码
    $scope.next = function(){
        if(!$scope.checkOver){
            console.log('未通过验证');
            return;
        }
        var param = {
            mobile: $scope.mobile,
            code: $scope.SMSCode
        };
        $http({
            method:'POST',
            url:'/owner-bd/index.php/Home/SMS/checkSMSCode',
            //url: '/owner-fd/mock/common.json',
            headers:{
                'Content-Type':'application/x-www-form-urlencoded'
            },
            dataType: 'json',
            data: $httpParamSerializer(param)
        }).then(function successCallback(response) {
            console.log(response.data);
            if(response.data.code == '0'){
                $scope.isShow1 = false;
                $scope.isShow2 = true;
            }
        }, function errorCallback(response) {
            console.log(response.data);
        });
    }

    // 刷新图片验证码
    $scope.refresh = function(){
        $scope.picCodeUrl = '/owner-bd/index.php/Home/CheckCode/getPicCode?'+Math.random();
    }

});
```

## 优化
以上代码，安全性不是很好，我们可以利用工具绕过前端验证。为了避免这个问题，可以在checkPicCode和checkSMSCode函数中添加session值来标记。
```
$_SESSION['checkPicCode'] = true;
$_SESSION['checkSMSCode'] = true;
```
在最后一步，向数据库中添加用户时，先验证一下两个session值是否都为true，都为true时再添加。

## 成果
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/sms-vertification-code/result.gif)


# 后记
以后也许有用的代码：
```
echo json_encode($_SESSION);// 打印出session中的数据
echo session_id();// 打印当前session的id
```

# 书签
云片网
https://www.yunpian.com/

cURL函数
http://php.net/manual/zh/ref.curl.php

curl 基础例子
http://php.net/manual/zh/curl.examples.php

在PHP语言中使用JSON
http://www.ruanyifeng.com/blog/2011/01/json_in_php.html

thinkphp验证码
http://document.thinkphp.cn/manual_3_2.html#verify

修改ThinkPHP的验证码类
http://www.cnblogs.com/BTMaster/p/3547878.html

ThinkPHP 3.2版本 , 无法读取$_SESSION['verify_code']
http://www.cnblogs.com/lovezbs/p/4496117.html

LICEcap - Download
http://licecap.en.softonic.com/

gif动态图局部加马赛克模糊广告文字
http://www.leawo.cn/space-138176-do-thread-id-64000.html
