---
title: AngularJS入门篇
toc: true
date: 2016-09-26 21:53:09
tags:
- angularjs
- 前端
- hexo
categories: 设计开发
---
# 前言
AngularJS是一个JavaScript框架，它通过指令扩展了HTML，且通过表达式绑定数据到 HTML。

顺便一提，什么是框架？比如struts2、spring、hibernate、thinkphp、wordpress等等。
那么，什么是组件？比如jdbc、jquery、swiper、layer、arttemplate等等。
一般来说，那些可复用的、用于简化开发工作的代码集合，大的叫框架，小的叫组件。
有人说jquery是框架？当然可以，大小并没有明确边界。
不要太纠结于概念，如无必要，勿增实体。

本文，主要学习归纳一下Angular的各种特性，包括双向数据绑定、定义应用和控制器、优化模板渲染延迟、自定义指令、作用域、HTTP请求获取数据、自定义服务、依赖注入、路由控制等。最后，会给出一个综合实例。

<!--more-->


# 双向数据绑定
单向数据绑定的原理：模板+数据=>视图。
![单向数据绑定](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/angurlarjs-start/单向数据绑定.jpg)
目前大多数前端框架都是单向数据绑定，比如jQueryUI、BackBone、Flex。

双向数据绑定原理：模板+数据=>视图，模板+视图=>数据。
![双向数据绑定](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/angurlarjs-start/双向数据绑定.jpg)

Angular采用的，就是双向数据绑定。

```
<!--helloworld.html-->
<!DOCTYPE html>
<html ng-app>
<head>
    <meta charset="UTF-8">
    <title>双向数据绑定</title>
    <script src="http://code.angularjs.org/angular-1.0.1.min.js"></script>
</head>
<body>
    Hello {{'World'}}!<br/>
    Your name: <input type="text" ng-model="yourname" placeholder="World">
    <hr>
    Hello {{yourname || 'World'}}!
</body>
</html>
```

# 定义应用和控制器
angular对象，是Angular的根对象。类似于express框架中的express对象，类似于seajs框架的seajs对象，类似于浏览器的window对象。
如果说angular对象是Angular中的班主任，那么应用（或者叫模块，app）就是Angular中的班长！而班主任不常出没，管事的就是班长。
控制器（controller），就是普通同学小明，负责控制Angular应用程序中的数据。
![angularjs模块](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/angurlarjs-start/angularjs模块.jpg)

```
<!--app.html-->
<!DOCTYPE html>
<html ng-app="myApp">
<head>
    <meta charset="UTF-8">
    <title>定义应用和控制器</title>
    <style>
        [ng\:cloak], [ng-cloak], [data-ng-cloak], [x-ng-cloak], .ng-cloak, .x-ng-cloak {
          display: none !important;
        }
    </style>
</head>
<body>
    <div ng-controller="myCtrl">

    名: <input type="text" ng-model="firstName"><br>
    姓: <input type="text" ng-model="lastName"><br>
    <br>
    姓名: <span>{{firstName + " " + lastName}}</span><br>
    姓名2: <span class="ng-cloak">{{fullName()}}</span><br>
    姓名3: <span ng-bind="fullName()"></span>
    </div>

    <script src="http://code.angularjs.org/angular-1.0.1.min.js"></script>
    <script>
        var app = angular.module('myApp', []);
        app.controller('myCtrl', function($scope) {
            $scope.firstName= "John";
            $scope.lastName= "Doe";
            $scope.fullName = function() {
                return $scope.firstName + " " + $scope.lastName;
            }
        });
    </script>
</body>
</html>
```

# 优化模板渲染延迟
在定义应用和控制器的例子中，我们看到，页面上先出现了表达式，之后才出现我们期望的结果。解决这个问题，常用的有两个办法。一个是使用ng-bind，另一个是添加ng-cloak样式。

# 自定义指令
```
<!--directive.html-->
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>自定义指令</title>
    <script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script> 
</head>
<body ng-app="myApp">
    <runoob-directive></runoob-directive>
    <div runoob-directive></div>
    <div class="runoob-directive"></div>
    <!-- 指令: runoob-directive -->

    <script>
        var app = angular.module("myApp", []);
        app.directive("runoobDirective", function() {
            return {
                //restrict : "A",
                //restrict : "C",
                //restrict : "M",
                //replace : true,
                template : "<h1>自定义指令!</h1>"
            };
        });
    </script>
</body>
</html>
```

# 作用域
```
<!--scope.html-->
<!DOCTYPE html>
<html ng-app="myApp">
<head>
    <meta charset="utf-8">
    <title>作用域</title>
    <script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script>
</head>
<body>
    <div ng-controller="myCtrl">
        <h1>姓氏为 {{lastname}} 家族成员:</h1>
        <ul>
            <li ng-repeat="x in names">{{x}} {{lastname}}</li>
        </ul>
    </div>

    <script>
        var app = angular.module('myApp', []);
        app.controller('myCtrl', function($scope, $rootScope) {
            $scope.names = ["Emil", "Tobias", "Linus"];
            $rootScope.lastname = "Refsnes";
        });
    </script>

    <p>注意 $rootScope 在循环对象内外都可以访问。</p>

</body>
</html>
```

上面的例子中，$scope的作用域为myCtrl这个ng-controller的范围，$rootScope的作用域为myApp这个ng-app的范围。

# HTTP请求获取数据
## 获取本地数据
```
<!--http.html-->
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>HTTP请求</title>
    <script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script>
</head>
<body ng-app="myApp" >
    <div ng-controller="myCtrl"> 
        <h1>欢迎你！{{username}}</h1>
    </div>
    <p> $http 服务向服务器请求信息，返回的值放入变量 "username" 中。</p>
<script>
    var app = angular.module('myApp', []);
    app.controller('myCtrl', function($scope, $http) {
      $http.get("http.json").then(function (response) {
          $scope.username = response.data.username;
      });
    });
</script>

</body>
</html>

```

http.json中的内容为：
```
{
    "username":"voidking" 
}
```

需要注意的是，本例需要在服务器中访问。因为Angular的HTTP请求封装了XMLHttpRequest，而XMLHttpRequest的使用需要服务器环境。

## 获取服务器数据
```
<!--http2.html-->
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>HTTP请求服务器数据</title>
    <script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script>
</head>
<body ng-app="myApp" >

    <div ng-controller="myCtrl"> 
    <h1>欢迎你！{{username}}</h1>

    </div>

    <p> $http 服务向服务器请求信息，返回的值放入变量 "username" 中。</p>

<script>
    var app = angular.module('myApp', []);
    app.controller('myCtrl', function($scope, $http, $httpParamSerializer) {
        $http({
            method:'POST',
            url:'/angulardemo/http.php',
            headers:{
                'Content-Type':'application/x-www-form-urlencoded'
            },
            dataType: 'json',
            data: $httpParamSerializer({username:'voidking'})
        }).then(function successCallback(response) {
            console.log(response.data);
            $scope.username = response.data.username;
        }, function errorCallback(response) {
            console.log(response.data);
        });;
    });
</script>

</body>
</html>

```

新建http.php，内容如下：
```
<?php 
    $username = $_POST['username'];
    $result = array(
        'code' => '0',
        'ext' => 'success',
        'username' => $username
    );
    echo json_encode($result);
?>
```


# 自定义服务

```
<!--service.html-->
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>自定义Service</title>
    <script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script>
</head>
<body ng-app="myApp" >
    <div ng-controller="myCtrl">
        <p>自定义服务，用于转换16进制数：</p>
        <p>255 的16进制是:</p>
        <h1>{{hex}}</h1>
        <hr>
        <p>在获取数组 [255, 251, 200] 值时使用过滤器:</p>
        <ul>
          <li ng-repeat="x in counts">{{x | myFormat}}</li>
        </ul>
    </div>
<script>
    var app = angular.module('myApp', []);
    app.service('hexafy', function() {
        this.myFunc = function (x) {
            return x.toString(16);
        }
    });
    app.controller('myCtrl', function($scope, hexafy) {
        $scope.hex = hexafy.myFunc(255);
        $scope.counts = [255, 251, 200];
    });
    app.filter('myFormat',['hexafy', function(hexafy) {
        return function(x) {
            return hexafy.myFunc(x);
        };
    }]);
</script>

</body>
</html>

```

当创建了自定义服务，并连接到应用上后，我们可以在控制器，指令，过滤器或其他服务中使用它。

# 依赖注入
AngularJS 提供很好的依赖注入机制。什么是依赖注入？wiki 上的解释是：依赖注入（Dependency Injection，简称DI）是一种软件设计模式，在这种模式下，一个或更多的依赖（或服务）被注入（或者通过引用传递）到一个独立的对象（或客户端）中，然后成为了该客户端状态的一部分。
该模式分离了客户端依赖本身行为的创建，这使得程序设计变得松耦合，并遵循了依赖反转和单一职责原则。与服务定位器模式形成直接对比的是，它允许客户端了解客户端如何使用该系统找到依赖。

```
<!--di.html-->
<!DOCTYPE html>
<html> 
<head>
   <meta charset="utf-8">
   <title>AngularJS依赖注入</title>
</head>
   
<body ng-app="mainApp" >
   <h2>AngularJS 简单应用</h2>
   <div ng-controller="CalcController">
      <p>配置：{{constant}}</p>
      <p>输入一个数字: <input type = "number" ng-model = "number" /></p>
      <button ng-click = "square()">X<sup>2</sup></button>
      <p>结果: {{result}}</p>
   </div>
   <hr>
   <div ng-controller="CalcController2">
      <p>再输入一个数字: <input type = "number" ng-model = "number" /></p>
      <button ng-click = "square()">X<sup>2</sup></button>
      <p>结果: {{result}}</p>
   </div>
   
<script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script>

<script>
   var mainApp = angular.module("mainApp", []);

   mainApp.config(function($provide) {
      // 创建一个名叫MathService的provider
      $provide.provider('MathService', function() {
         this.$get = function() {
            var factory = {};         
            factory.multiply = function(a, b) {
               return a * b;
            }
            return factory;
         };
      });
   });

   // 创建一个名叫defaultInput的value
   mainApp.value("defaultInput", 5);

   // 创建一个名叫constant的constant value
   mainApp.constant("constant", "constant value");


   // 将MathService、defaultInput、constant注入到控制器
   mainApp.controller('CalcController', function($scope, MathService, defaultInput, constant) {
      $scope.number = defaultInput;
      $scope.constant = constant;

      $scope.result = MathService.multiply($scope.number,$scope.number);

      $scope.square = function() {
         $scope.result = MathService.multiply($scope.number,$scope.number);
      }
   });


   /*--------以下是CalcController2的内容--------*/

   // 创建一个名叫MathService2的factory
   mainApp.factory('MathService2', function() {
      var factory = {};
      
      factory.multiply = function(a, b) {
         return a * b;
      }
      return factory;
   });
   
   // 创建一个名叫CalcService2的service，并且注入MathService2
   mainApp.service('CalcService2', function(MathService2){
      this.square = function(a) {
         return MathService2.multiply(a,a);
      }
   });

   // 将CalcService2注入到控制器
   mainApp.controller('CalcController2',function($scope,CalcService2){
      $scope.number = 6;
      $scope.result = CalcService2.square($scope.number);
      $scope.square = function() {
         $scope.result = CalcService2.square($scope.number,$scope.number);
      }
   });
    
</script>
   
</body>
</html>

```

provider()函数是用来创建provider对象的标准方法。

实际上，value()、constant()、factory()、service()全都是用来创建一个provider对象的方法，它们提供了一种方式来定义一个provider，而无需输入所有的复杂的代码。

# 路由控制
AngularJS 路由允许我们通过不同的 URL 访问不同的内容。
通过 AngularJS 可以实现多视图的单页Web应用（single page web application，SPA）。
通常我们的URL形式为http://runoob.com/first/page ，但在单页Web应用中AngularJS 通过 # + 标记 实现，例如：
```
http://runoob.com/#/first
http://runoob.com/#/second
http://runoob.com/#/third
```

当我们点击以上的任意一个链接时，向服务端请的地址都是一样的 (http://runoob.com/)。 因为 # 号之后的内容在向服务端请求时会被浏览器忽略掉。 所以我们就需要在客户端实现 # 号后面内容的功能实现。 AngularJS 路由 就通过 # + 标记 帮助我们区分不同的逻辑页面并将不同的页面绑定到对应的控制器上。

![路由](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/angurlarjs-start/路由.png)

AngularJS 模块的 config 函数用于配置路由规则。通过使用 configAPI，我们请求把$routeProvider注入到我们的配置函数并且使用$routeProvider.whenAPI来定义我们的路由规则。
$routeProvider 为我们提供了 when(path,object) & otherwise(object) 函数按顺序定义所有路由，函数包含两个参数:
第一个参数是 URL 或者 URL 正则规则。
第二个参数是路由配置对象。

```
<!--router.html-->
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>路由控制</title>
</head>

<body ng-app="ngRouteExample" class="ng-scope">
    <div> 
        <div id="navigation">  
        <a href="#/home">Home</a>
        <a href="#/about">About</a>
    </div>
      
    <div ng-view="">
    </div>

<script type="text/ng-template" id="embedded.home.html">
    <h1> Home </h1>
</script>

<script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script>
<script src="http://apps.bdimg.com/libs/angular-route/1.3.13/angular-route.js"></script>

<script type="text/javascript">
    angular.module('ngRouteExample', ['ngRoute'])
    .controller('HomeController', function ($scope, $route) { $scope.$route = $route;})
    .controller('AboutController', function ($scope, $route) { $scope.$route = $route;})
    .config(function ($routeProvider) {
        $routeProvider.
        when('/home', {
            templateUrl: 'embedded.home.html',
            controller: 'HomeController'
        }).
        when('/about', {
            templateUrl: 'about.html',
            controller: 'AboutController'
        }).
        otherwise({
            redirectTo: '/home'
        });
    });
</script>

</body>
</html>
```

```
<!--about.html-->
<h1> About </h1>
```

# 综合
```
<!--complex.html-->
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>综合实例</title>
</head>
<body ng-app="ngRouteExample" class="ng-scope">
    <div> 
        <div id="navigation">  
        <a href="#/page1">Page1</a>
        <a href="#/page2">Page2</a>
    </div>     
    <div ng-view="">
    </div>

<script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script>
<script src="http://apps.bdimg.com/libs/angular-route/1.3.13/angular-route.js"></script>

<script type="text/javascript">
    
    var myApp = angular.module('ngRouteExample', ['ngRoute']);
    myApp.controller('Page1Controller', function ($scope, $route) { 
        $scope.$route = $route;
        $scope.content = '这是page1的内容';
    });
    myApp.controller('Page2Controller', function ($scope, $route) { 
        $scope.$route = $route;
        $scope.content = '这是page2的内容';
    })

    myApp.config(function ($routeProvider) {
        $routeProvider.
        when('/page1', {
            templateUrl: 'complex-page1.html',
            controller: 'Page1Controller'
        }).
        when('/page2', {
            templateUrl: 'complex-page2.html',
            controller: 'Page2Controller'
        }).
        otherwise({
            redirectTo: '/page1'
        });
    });
</script>
</body>
</html>
```

```
<!--complex-page1.html-->
<div id="page1" ng-controller="Page1Controller">
    <h1>Page1</h1>
    <p>{{content}}</p>
</div>
```

```
<!--complex-page2.html-->
<div id="page2" ng-controller="Page2Controller">
    <h1>Page2</h1>
    <p>{{content}}</p>
</div>
```

# 后记
至于输入验证、事件、动画、API等，本文不再讨论，用到时自行查阅文档。
本文完整源码地址：https://github.com/voidking/angulardemo

记录一个hexo的坑：如果文中出现了双括号，而且双括号没有被代码块包含，那么解析会报错，无法生成页面。

查找到的解决办法：
```
{% raw %}
内容
{% endraw %}
```

经测试，无效，就用汉字代替好了。

# 书签
AngularJS实战
http://www.imooc.com/learn/156

AngularJS 教程 | 菜鸟教程
http://www.runoob.com/angularjs/angularjs-tutorial.html

AngularJS中文网
http://www.apjs.net/

AngularJS中文社区
http://angularjs.cn/

图灵社区: 合集 : AngularJS入门教程
http://www.ituring.com.cn/minibook/303

AngularJS: API: API Reference
https://docs.angularjs.org/api

ngCloak
https://docs.angularjs.org/api/ng/directive/ngCloak

AngularJS : Why ng-bind is better than 双括号 in angular?
http://stackoverflow.com/questions/16125872/angularjs-why-ng-bind-is-better-than-in-angular

Metronic3.3网页模板在线演示
http://metronic.kp7.cn/

框架到底是个什么东西？
https://www.zhihu.com/question/32069908

理解AngularJS中的依赖注入
http://sentsin.com/web/663.html

Hexo的一个小BUG(Template render error)
http://www.jianshu.com/p/738ebe02029b
