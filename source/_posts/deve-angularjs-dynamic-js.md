---
title: AngularJS按需加载js
toc: true
date: 2016-09-28 23:57:39
tags:
- angularjs
- 前端
categories: 设计开发
---
# 前言
Angular是一个单页面应用，随着系统的迭代，首屏代码会越来越大，所以对《AngularJS入门》中的代码进行改造，实现AngularJS可以按需加载js和css。
实现这个需求，有三个方案：
1、利用requirejs。
requirejs并不是按照angular规范开发的第三方插件，后期估计会有很多坑，放弃。

2、利用ui-router和ocLazyLoad。
- 每次“页面跳转”都要额外请求js并加载，浪费带宽增加页面加载时间，基本抛弃了预加载。
- 每一个路由都需要配置resolve属性，太low。
- 模块化程度太低，不利于以后代码移植和维护。

3、自己写需要的组件。
最好的方案，然而技术要求太高，放弃。

<!--more-->

综上，第三种方案暂时无法实现，放弃；第一种方案坑太多，放弃；第二种方案也不好，但是相对容易，而且是针对angular的插件，就它了。
```
bower install angular#1.5.8
bower install angular-ui-router
bower install oclazyload
bower install bootstrap
```

# 核心代码

```
<!--dynamic/index.html-->
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="bower_components/bootstrap/dist/css/bootstrap.min.css">
     <style>
        body{
            font-family: "Microsoft Yahei";
        }
        .navigator{
            width: 500px;margin: 0 auto
        }
        .navigator li{
           color: #000;font-size: 14px;
        }
    </style>
    <title>按需加载js</title>
</head>
<body ng-app="myApp">
    <ul class="navigator nav nav-pills">
        <li role="presentation" class="active"><a href="#home" ng-click="isActive($event)">主页</a></li>
        <li role="presentation" class="active"><a href="#page2">Page2</a></li>
        <li role="presentation" class="active"><a href="#page3" ng-click="isActive($event)">Page3</a></li>
    </ul>
    <div ui-view style="width: 500px;margin: 50px auto 0"></div>

<script src="bower_components/angular/angular.min.js"></script>
<script src="bower_components/angular-ui-router/release/angular-ui-router.min.js"></script>
<script src="bower_components/oclazyload/dist/ocLazyLoad.min.js"></script>
<script src="public/js/index.js"></script>
</body>
</html>
```

```
/*
 *dynamic/public/js/index.js
*/
var myApp=angular.module("myApp",["ui.router","oc.lazyLoad"]);
myApp.config(function ($stateProvider,$urlRouterProvider) {
    $urlRouterProvider.when("","/home");

    $stateProvider.state('home',{
        url:"/home",
        templateUrl: 'views/homepage.html',
        controller: 'homeController',
        resolve:{
            loadMyCtrl:['$ocLazyLoad',function ($ocLazyLoad) {
                return $ocLazyLoad.load({
                    name:"homeApp",
                    files:["public/js/homepage.js"]
                })
            }]
         }
    });

    $stateProvider.state('page2',{
        url:"/page2",
        templateUrl:'views/page2.html',
        resolve:{
            loadMyCtrl:function ($ocLazyLoad) {
                return $ocLazyLoad.load({
                    name:'page2App',
                    files:["public/js/page2.js"]
                })
            }
        }
    })
    $stateProvider.state('page3',{
        url:"/page3",
        templateUrl:'views/page3.html',
        resolve:{
            loadMyCtrl:function ($ocLazyLoad) {
                return $ocLazyLoad.load({
                    name:'page3App',
                    files:["public/js/page3.js","public/js/page3-ext.js"]
                })
            }
        }
    })
    
});

```

```
<!--dynamic/views/homepage.html-->
<div id="home" ng-controller="homeController">
    <h1>首页</h1>
    {{content}}
</div>

```

```
/*
 *dynamic/public/js/homepage.js
*/
angular.module('myApp').controller('homeController', function ($scope) { 
    $scope.content = '这是主页的内容';
});

```

完整代码github自取：https://github.com/voidking/angulardemo/tree/master/dynamic

# 书签
RequireJS官方文档
http://requirejs.org/docs/start.html

Dynamically Loading Controllers and Views with AngularJS
http://weblogs.asp.net/dwahlin/dynamically-loading-controllers-and-views-with-angularjs-and-requirejs

angular应用如何实现按需加载
http://www.alloyteam.com/2015/10/angular-application-how-to-load-on-demand/

尝试通过AngularJS模块按需加载搭建大型应用（上）
http://web.jobbole.com/86915/

尝试通过AngularJS模块按需加载搭建大型应用（下）
http://web.jobbole.com/87025/

angularjs ocLazyLoad分步加载js文件,angularjs ocLazyLoad按需加载js
http://m.w2bc.com/article/158713

按需加载 AngularJS 的 Controller
http://beginor.github.io/2014/12/20/angularjs-controller-load-on-demand.html