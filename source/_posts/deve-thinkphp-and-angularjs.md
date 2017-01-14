---
title: thinkphp和angularjs整合
toc: true
date: 2016-09-29 09:12:35
tags:
- angularjs
- thinkphp
- php
- 前端
categories: 设计开发
---
# 前言
为了前后端分离的更彻底，便于前后端独立开发，小太阳项目，计划使用thinkphp+angular。后端专注写接口，前端负责页面渲染。

<!--more-->

# 项目分割
项目分成三个子系统：业主端、物业端、CMS端。每个子系统分别有前端和后端，前端使用angular，后端使用thinkphp。
自此，产生了六个子项目，分别命名为owner-fd、owner-bd、manager-fd、manager-bd、cms-fd和cms-bd。
接下来，我们以cms-fd和cms-bd为例，来说明angular和thinkphp之间的交互。

# 添加业主信息
业务逻辑：在添加业主信息页面，填写业主信息的表单，填写完成后单击“确认添加”按钮，发送http请求给后端。后端把获取到的数据存到数据库中，并且返回值给前端，前端提示成功或失败。

## angular部分
1、入口html
```
<!--cms-fd/index.html-->
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="public/libs/bootstrap/dist/css/bootstrap.min.css">
    <link rel="stylesheet" href="public/libs/layer/skin/layer.css">
    <link rel="stylesheet" href="public/css/index.css">
    <title>CMS系统</title>
</head>
<body ng-app="myApp">
    <ul class="navigator nav nav-pills" ng-controller="MainCtroller">
        <li role="presentation" ng-class="{active:'home' == currentTab}">
            <a ui-sref="home" ng-click="changeTab('home')">首页</a>
        </li>
        <li role="presentation" ng-class="{active:'ownerList' == currentTab}">
            <a ui-sref="ownerList" ng-click="changeTab('ownerList')">业主信息列表</a>
        </li>
        <li role="presentation" ng-class="{active:'ownerAdd' == currentTab}">
            <a ui-sref="ownerAdd" ng-click="changeTab('ownerAdd')">添加业主信息</a>
        </li>
    </ul>
    <div ui-view style="width: 500px;margin: 50px auto 0"></div>

<script src="public/libs/angular/angular.min.js"></script>
<script src="public/libs/angular-ui-router/release/angular-ui-router.min.js"></script>
<script src="public/libs/oclazyload/dist/ocLazyLoad.min.js"></script>
<script src="public/libs/jquery/dist/jquery.min.js"></script>
<script src="public/libs/layer/layer.js"></script>
<script src="public/js/index.js"></script>
</body>
</html>
```

2、入口js
```
/*
 *cms-fd/public/js/index.js
*/
var myApp = angular.module('myApp',['ui.router','oc.lazyLoad']);

myApp.config(function ($stateProvider,$urlRouterProvider) {
    $urlRouterProvider.when('','/home');

    $stateProvider.state('home',{
        url:'/home',
        templateUrl: 'views/home.html',
        resolve:{
            loadMyCtrl:['$ocLazyLoad',function ($ocLazyLoad) {
                return $ocLazyLoad.load({
                    name:'homeController',
                    files:['public/js/home.js']
                })
            }]
         }
    });

    $stateProvider.state('ownerList',{
        url:'/ownerList',
        templateUrl:'views/owner/list.html',
        resolve:{
            loadMyCtrl:function ($ocLazyLoad) {
                return $ocLazyLoad.load({
                    name:'ownerListController',
                    files:['public/js/owner/list.js']
                })
            }
        }
    });

    $stateProvider.state('ownerAdd',{
        url: '/ownerAdd',
        templateUrl: 'views/owner/add.html',
        resolve:{
            loadMyCtrl:function ($ocLazyLoad) {
                return $ocLazyLoad.load({
                    name:'ownerAddController',
                    files:['public/js/owner/add.js']
                })
            }
        }
    });

    $stateProvider.state('ownerEdit',{
        url: '/owner/edit/:ownerId',
        templateUrl: 'views/owner/edit.html',
        resolve:{
            loadMyCtrl:function ($ocLazyLoad) {
                return $ocLazyLoad.load({
                    name:'ownerEditController',
                    files:['public/js/owner/edit.js']
                })
            }
        }
    });
    
});

myApp.controller('MainCtroller',function($scope,$location){
    //console.log($location.url());
    var url = $location.url();
    $scope.currentTab = url.substr(1);
    $scope.changeTab = function(tabname){  
        $scope.currentTab = tabname;
    };  
});

```

3、添加业主信息页面
```
<!--cms-fd/views/owner/add.html-->
<div id="home" ng-controller="ownerAddController">
    <h2>添加业主信息</h2>
    <form role="form">
        <div class="form-group">
            <label for="">用户名</label>
            <input name="username" type="text" class="form-control username" id="" ng-model="username">
        </div>
        <div class="form-group">
            <label for="">密码</label>
            <input name="password" type="password" class="form-control password" id="" ng-model="password">
        </div>
        <div class="form-group">
            <label for="">邮箱</label>
            <input name="password" type="email" class="form-control email" id="" ng-model="email">
        </div>
        <div class="form-group">
            <label for="">昵称</label>
            <input name="nickname" type="text" class="form-control nickname" id="" ng-model="nickname">
        </div>
        <button type="submit" class="btn btn-primary submit" ng-click="submit()">确认添加</button>
    </form>
</div>

```

3、添加业主信息页面js
```
/*
 *cms-fd/public/js/owner/add.js
*/
angular.module('myApp').controller('ownerAddController', function ($scope,$http,$httpParamSerializer) { 
    $scope.submit = function(){
        var param = {
            username: $scope.username,
            password: $scope.password,
            email: $scope.email,
            nickname: $scope.nickname
        };
        $http({
            method:'POST',
            url:'/cms-bd/index.php/Home/Owner/add',
            headers:{
                'Content-Type':'application/x-www-form-urlencoded'
            },
            dataType: 'json',
            data: $httpParamSerializer(param)
        }).then(function successCallback(response) {
            console.log(response.data);
            layer.msg(response.data.ext);
        }, function errorCallback(response) {
            console.log(response.data);

        });
    }
});
```

## thinkphp部分
在cms-bd/Application/Home/Controller中新建OwnerController.class.php，编写add函数。
```
// 增加业主
public function add(){
    if(!$_POST['username'] || !$_POST['password'] || !$_POST['email'] || !$_POST['nickname']){
        $result = array(
            'code' => '0',
            'ext' => '参数不足' 
        );
        echo json_encode($result,JSON_UNESCAPED_UNICODE);
        return;
    }
    $data['username'] = $_POST['username'];
    $data['password'] = md5($_POST['password']);
    $data['email'] = $_POST['email'];
    $data['nickname'] = $_POST['nickname'];
    $data['create_at'] = date('Y-m-d H:i:s');
    $data['update_at'] = date('Y-m-d H:i:s');

    $owner = D('owner');
    if($owner->create($data)){
        $id = $owner->add();
        if($id){
            $owner_temp = $owner->where("id='$id'")->find();
            $result = array(
                'code'=> '0',
                'ext'=> 'success',
                'obj'=>$owner_temp
            );
            echo json_encode($result,JSON_UNESCAPED_UNICODE);
        }
    }
}
```

# 查看业主信息列表和删除业主信息
业务逻辑：进入业主信息列表页时，发送http请求给后端，获取到业主信息列表，然后显示到业主信息列表页上。
单击某条记录后面的“删除”按钮，弹出确认提示框。确认删除，则发送http请求给后端，获取返回值，如果删除成功，则从页面移除该条记录。

## angular部分
1、业主信息列表页面
```
<!--cms-fd/views/owner/list.html-->
<div id="owner-list" ng-controller="ownerListController">
    <h2>业主列表</h2>
    <ul>
        <li ng-repeat="owner in ownerList">
            <span>用户名：{{owner.username}}，昵称：{{owner.nickname}}</span>
            <button ng-click="edit(owner.id)">修改</button>
            <button ng-click="delete(owner.id,$index)">删除</button>
        </li>
    </ul> 
</div>

```

2、业主信息列表页js
```
/*
 *cms-fd/public/js/owner/list.js
*/
angular.module('myApp').controller('ownerListController', function ($scope,$http,$httpParamSerializer,$state) { 
    $http({
        method: 'POST',
        url: '/cms-bd/index.php/Home/Owner/listAll',
        headers:{
            'Content-Type':'application/x-www-form-urlencoded'
        },
        dataType: 'json',
        data: $httpParamSerializer({})
    }).then(function successCallback(response){
        console.log(response.data);
        $scope.ownerList = response.data;
    }, function errorCallback(response){
        console.log(response.data);
    });

    $scope.edit = function(ownerId){
        $state.go('ownerEdit', {ownerId: ownerId});
    }

    $scope.delete = function(ownerId,index){
        var layerIndex = layer.confirm('确认删除？', {
            btn: ['是的','取消'] //按钮
        }, function(){
            $http({
                method:'POST',
                url:'/cms-bd/index.php/Home/Owner/delete',
                headers:{
                    'Content-Type':'application/x-www-form-urlencoded'
                },
                dataType: 'json',
                data: $httpParamSerializer({ownerId: ownerId})
            }).then(function successCallback(response) {
                console.log(response.data);
                if(response.data.code == '0'){
                    $scope.ownerList.splice(index,1);
                }
                layer.close(layerIndex);
            }, function errorCallback(response) {
                console.log(response.data);
                layer.close(layerIndex);
            });
            
        }, function(){
            //layer.msg('取消');
        });
    }
});
```

## thinkphp部分
添加listAll函数和delete函数。
```
// 业主列表
public function listAll(){
    $owner = D('owner');
    $resultArr = $owner->where('state=0')->order('update_at desc,id desc')->select();
    echo json_encode($resultArr,JSON_UNESCAPED_UNICODE);
}

// 删除业主
public function delete(){
    $ownerId = $_POST['ownerId'];
    $data['state'] = 1;

    $owner = D('owner');
    $success = $owner->where("id='$ownerId'")->save($data);
    if($success){
        $result = array(
            'code'=> '0',
            'ext'=> 'success'
        );
        echo json_encode($result);
    }else {
        $result = array(
            'code'=> '1',
            'ext'=> 'fail'
        );
        echo json_encode($result);
    }
}
```

# 修改业主信息
业务逻辑：在业主信息列表页，单击某条记录后的“修改”按钮，跳转到修改业主信息页面。修改完成后，单击“确认修改”按钮，跳转回业主信息列表页。

## angular部分
1、修改业主信息页面
```
<!--cms-fd/views/owner/edit.html-->
<div id="owner-edit" ng-controller="ownerEditController">
    <h2>修改业主信息</h2>
    <form role="form">
        <div class="form-group">
            <label for="">用户名</label>
            <input name="username" type="text" class="form-control username" id="" ng-model="owner.username">
        </div>
        <div class="form-group">
            <label for="">密码</label>
            <input name="password" type="password" class="form-control password" id="" ng-model="owner.password">
        </div>
        <div class="form-group">
            <label for="">邮箱</label>
            <input name="password" type="email" class="form-control email" id="" ng-model="owner.email">
        </div>
        <div class="form-group">
            <label for="">昵称</label>
            <input name="nickname" type="text" class="form-control nickname" id="" ng-model="owner.nickname">
        </div>
        <button type="submit" class="btn btn-primary submit" ng-click="submit()">确认修改</button>
    </form>
</div>

```

2、修改业主信息js
```
/*
 *cms-fd/public/js/owner/edit.js
*/
angular.module('myApp').controller('ownerEditController', function ($scope,$http,$httpParamSerializer,$stateParams,$state) { 
    $http({
        method:'POST',
        url:'/cms-bd/index.php/Home/Owner/findById',
        headers:{
            'Content-Type':'application/x-www-form-urlencoded'
        },
        dataType: 'json',
        data: $httpParamSerializer({ownerId: $stateParams.ownerId})
    }).then(function successCallback(response) {
        console.log(response.data);
        $scope.owner = response.data.obj;
    }, function errorCallback(response) {
        console.log(response.data);
    });

    $scope.submit = function(){
        $http({
            method:'POST',
            url:'/cms-bd/index.php/Home/Owner/edit',
            headers:{
                'Content-Type':'application/x-www-form-urlencoded'
            },
            dataType: 'json',
            data: $httpParamSerializer($scope.owner)
        }).then(function successCallback(response) {
            console.log(response.data);
            $state.go('ownerList');
        }, function errorCallback(response) {
            console.log(response.data);
        });
    }
});
```

## thinkphp部分
添加findById和edit两个函数。
```
// 根据id查找业主
public function findById(){
    $ownerId = $_POST['ownerId'];
    $owner = D('owner');
    $ownerObj = $owner->where("id='$ownerId'")->find();
    if($ownerObj){
        $result = array(
            'code'=> '0',
            'ext'=> 'success',
            'obj'=> $ownerObj
        );
        echo json_encode($result,JSON_UNESCAPED_UNICODE);
    }else{
        $result = array(
            'code'=> '1',
            'ext'=> '没有找到记录'
        );
        echo json_encode($result,JSON_UNESCAPED_UNICODE);
    }  
}

// 修改业主
public function edit(){
    if(!$_POST['id'] || !$_POST['username'] || !$_POST['password'] || !$_POST['email'] || !$_POST['nickname']){
        $result = array(
            'code' => '0',
            'ext' => '参数不足' 
        );
        echo json_encode($result,JSON_UNESCAPED_UNICODE);
        return;
    }
    $id = $_POST['id'];
    $data['username'] = $_POST['username'];
    $data['password'] = md5($_POST['password']);
    $data['email'] = $_POST['email'];
    $data['nickname'] = $_POST['nickname'];
    $data['update_at'] = date('Y-m-d H:i:s');

    $owner = D('owner');
    $success = $owner->where("id='$id'")->save($data);
    if($success){
        $owner_temp = $owner->where("id='$id'")->find();
        $result = array(
            'code'=> '0',
            'ext'=> 'success',
            'obj'=>$owner_temp
        );
        echo json_encode($result,JSON_UNESCAPED_UNICODE);
    }else {
        $result = array(
            'code'=> '1',
            'ext'=> '用户不存在'
        );
        echo json_encode($result,JSON_UNESCAPED_UNICODE);
    }
}
```


# thinkphp关闭右下角Trace信息
thinkphp默认开启调试模式，返回值时总会跟着一个小图标，默认在页面的右下角小图标。在部署阶段，需要把它关闭。
1、在入口文件index.php加入
```
define("APP_DEBUG", false);
```

2、在Application/Common/config.php 配置文件中加入
```
'SHOW_PAGE_TRACE' => false
```

3、删除Application下的Runtime文件夹。

# 后记
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/thinkphp-and-angularjs/result.gif)
增删查改跑通，这个demo也算是比较完整了。在实际开发的时候，很多地方还要做调整。
比如导航栏的一些坑、更好页面布局、更友好的提示和跳转、安全性校验、数据库表设计等等等等。

angular非常容易产生页面缓存，如果遇到很奇葩的坑，比如修改了某个页面，但是刷新无效。不要犹豫，先清下浏览器缓存。

# 书签
Php 5.6 "Automatically populating $HTTP_RAW_POST_DATA is deprecated
https://github.com/piwik/piwik/issues/6465

PHP 5.6: “Automatically populating $HTTP_RAW_POST_DATA is deprecated and will be removed in a future version.”
https://www.bram.us/2014/10/26/php-5-6-automatically-populating-http_raw_post_data-is-deprecated-and-will-be-removed-in-a-future-version/

angularjs 请求后端接口请求了两次
http://jingyan.baidu.com/article/49ad8bce42a2415834d8fa97.html

AngularJs + angular-ui-router + bootstrap 实现基础导航栏
http://blog.csdn.net/a416311458/article/details/51497230

Angular结合Bootstrap3的导航菜单
http://www.tuicool.com/articles/ayqqmi




