---
title: thinkphp中session跨域问题
toc: true
date: 2016-11-27 12:42:38
tags:
- thinkphp
- session
- nginx
- 跨域
categories: 设计开发
---
# 问题描述
《thinkphp实现短信验证注册》中，小编不止记录了短信验证码的实现方法，同时还记录了图片验证码的实现方法。
本地使用，一切正常；后端项目和前端项目都部署到服务器，一切正常；后端项目部署到服务器，并设置允许跨域访问后，本地前端项目使用服务器上后端项目接口时，问题来了：
首先，使用postman测试获取图片验证码接口和验证图片验证码接口，正常。
然后，在html中使用获取图片验证码接口，正常；最后，在JS中使用验证图片验证码接口，出错！！！

<!--more-->

# 分析
通过问题描述，我们看出，问题出现在跨域上。那么，有两种可能，一种是因为跨域设置不正确；一种是因为thinkphp本身的问题。

采用另外一种跨域配置，问题依然存在。那就是thinkphp本身的问题了，经查找资料，问题定位在thinkphp的session跨域上。

# 跨子域解决办法
其实不管是ThinkPHP还是php本身，在解决session跨域问题的时候都需要设置session.cookie_domain。
针对session跨域这一问题的解决方法主要有以下几种：
第一种情况：如果目录下没有.htaccess这个文件，也就是没有采取url伪静态的话，那么，在conf/config.php的第一行加上：
```
ini_set('session.cookie_domain',".domain.com");//跨域访问Session
```
这时如果你开启了调试，那么可以用！但关闭了调试，就不管用了！

第二种情况：如果你目录下有.htaccess这个文件，那么你在根目录，index.php的第一行加入：
```
<?php ini_set('session.cookie_domain',".domain.com");//跨域访问Session
// 应用入口文件
?>
```
这种方法不管开不开启调试都管用！

然而，我们的问题并不是跨子域的问题，而是完全跨域，所以上述方法无效。

# 完全跨域解决办法
## 获取图片验证码请求
查看获取图片验证码的请求信息，Request Headers为：
```
Accept:image/webp,image/*,*/*;q=0.8
Accept-Encoding:gzip, deflate, sdch
Accept-Language:zh-CN,zh;q=0.8,en-US;q=0.6,en;q=0.4
Connection:keep-alive
Cookie:pma_lang=zh_CN; pma_collation_connection=utf8_unicode_ci; pma_iv-1=wnpO4gv0eQRW1AMHmGr2ww%3D%3D; pmaUser-1=weZPqS0%2BW7nzFUVHRdqcfA%3D%3D
Host:api.voidking.com
Referer:http://localhost/ajax/ajax.html
User-Agent:Mozilla/5.0 (Windows NT 6.3; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.71 Safari/537.36
```

Response Headers为：
```
Access-Control-Allow-Origin:*
Cache-Control:post-check=0, pre-check=0
Cache-Control:private, max-age=0, no-store, no-cache, must-revalidate
Connection:keep-alive
Content-Type:image/png
Date:Sun, 27 Nov 2016 12:10:44 GMT
Expires:Thu, 19 Nov 1981 08:52:00 GMT
Pragma:no-cache
Server:nginx
Set-Cookie:PHPSESSID=721t4sqanvsii550m1dk8gq1o3; path=/; domain=.voidking.com
Transfer-Encoding:chunked
```

## 验证验证码请求
查看验证验证码的请求信息，Request Headers为：
```
Accept:application/json, text/javascript, */*; q=0.01
Accept-Encoding:gzip, deflate
Accept-Language:zh-CN,zh;q=0.8,en-US;q=0.6,en;q=0.4
Connection:keep-alive
Content-Length:9
Content-Type:application/x-www-form-urlencoded; charset=UTF-8
Host:api.voidking.com
Origin:http://localhost
Referer:http://localhost/ajax/ajax.html
User-Agent:Mozilla/5.0 (Windows NT 6.3; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.71 Safari/537.36
```

Response Headers为：
```
Access-Control-Allow-Origin:*
Cache-Control:no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Connection:keep-alive
Content-Encoding:gzip
Content-Type:text/html; charset=UTF-8
Date:Sun, 27 Nov 2016 12:13:21 GMT
Expires:Thu, 19 Nov 1981 08:52:00 GMT
Pragma:no-cache
Server:nginx
Set-Cookie:PHPSESSID=149t0hhs2icqaaemvp39onkgp4; path=/; domain=.voidking.com
Transfer-Encoding:chunked
Vary:Accept-Encoding
```

## 再次获取图片验证码请求
Request Headers为：
```
Accept:image/webp,image/*,*/*;q=0.8
Accept-Encoding:gzip, deflate, sdch
Accept-Language:zh-CN,zh;q=0.8,en-US;q=0.6,en;q=0.4
Cache-Control:max-age=0
Connection:keep-alive
Cookie:pma_lang=zh_CN; pma_collation_connection=utf8_unicode_ci; pma_iv-1=wnpO4gv0eQRW1AMHmGr2ww%3D%3D; pmaUser-1=weZPqS0%2BW7nzFUVHRdqcfA%3D%3D; PHPSESSID=721t4sqanvsii550m1dk8gq1o3
Host:api.voidking.com
Referer:http://localhost/ajax/ajax.html
User-Agent:Mozilla/5.0 (Windows NT 6.3; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.71 Safari/537.36
```

Response Headers为：
```
Access-Control-Allow-Origin:*
Cache-Control:private, max-age=0, no-store, no-cache, must-revalidate
Cache-Control:post-check=0, pre-check=0
Connection:keep-alive
Content-Type:image/png
Date:Sun, 27 Nov 2016 13:26:21 GMT
Expires:Thu, 19 Nov 1981 08:52:00 GMT
Pragma:no-cache
Server:nginx
Transfer-Encoding:chunked
```

## 三次请求比较
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/session-cross-domain/compare.jpg)

第一次获取图片验证码请求，Cookie中没有PHPSESSID，所以，返回信息中有Set-Cookie。第二次获取图片验证码请求，Cookie中含有PHPSESSID，所以，返回信息中没有了Set-Cookie。而且第一次请求返回信息Set-Cookie中的PHPSESSID，和第二次请求请求信息Cookie中的PHPSESSID是相同的。

而验证图片验证码的ajax请求，没有Cookie，自然也没有PHPSESSID，所以，返回信息中也有Set-Cookie。

可见，我们需要在前端做一些修改，使之发送请求时带着Cookie。

## 前端jquery设置
```
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>jquery</title>
</head>
<body>
    <p>
        <img src="http://api.voidking.com/owner-bd/index.php/Home/CheckCode/getPicCode" alt="">
        <input type="text" id="picCode">
        <input type="button" id="send" value="验证">
    </p>
<script src="http://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"></script>
<script>
    $(function(){
        $('#send').click(function(){
            //console.log(document.cookie);
            $.ajax({
                url: 'http://api.voidking.com/owner-bd/index.php/Home/CheckCode/checkPicCode',
                type: 'POST',
                crossDomain: true,
                xhrFields: {
                    withCredentials: true
                },
                dataType: 'json',
                data: {code: $('#picCode').val()},
                success: function(data){
                    console.log(data);
                },
                error: function(xhr){
                    console.log(xhr);
                }
            });
        });
    });
</script>
</body>
</html>
```

请求时报错如下：
```
A wildcard '*' cannot be used in the 'Access-Control-Allow-Origin' header when the credentials flag is true. Origin 'http://localhost' is therefore not allowed access. The credentials mode of an XMLHttpRequest is controlled by the withCredentials attribute.
```

出现了跨域报错，可见后端也需要做一些修改，使之可以接收跨域Cookie。


## 后端nginx设置

```
add_header Access-Control-Allow-Origin http://localhost;
add_header Access-Control-Allow-Credentials true;
```
注意：
服务器端Access-Control-Allow-Credentials参数为true时，Access-Control-Allow-Origin参数的值不能为\*。

后端nginx设置后，jquery的ajax请求正常了，可以携带Cookie，后端正常接收数据并返回数据。

由于angular的ajax请求不同于jquery，所以，我们还需要研究一下angular怎么发送携带Cookie的跨域请求。

## 前端angular设置
```
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>angular</title>
    <script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script>
</head>
<body ng-app="myApp" >
    <p ng-controller="myCtrl">
        <img src="http://api.voidking.com/owner-bd/index.php/Home/CheckCode/getPicCode" alt="">
        <input type="text" id="picCode" ng-model="picCode">
        <input type="button" ng-click="send()"  value="验证">
    </p>
<script>
    var app = angular.module('myApp', []);
    app.controller('myCtrl', function($scope, $http, $httpParamSerializer) {
        $scope.send = function(){
            $http({
                method:'POST',
                url:'http://api.voidking.com/owner-bd/index.php/Home/CheckCode/checkPicCode',
                headers:{
                    'Content-Type':'application/x-www-form-urlencoded'
                },
                withCredentials: true,
                dataType: 'json',
                data: $httpParamSerializer({code: $scope.picCode})
            }).then(function successCallback(response) {
                console.log(response.data);
                $scope.username = response.data.username;
            }, function errorCallback(response) {
                console.log(response.data);
            });
        }
    });
</script>

</body>
</html>
```

# nginx配置文件
结合《thinkphp部署到nginx服务器》中nginx的配置，最终nginx配置配置文件nginx.conf文件内容如下：
```
error_log  logs/error.log  error ;
pid logs/nginx.pid;
user  www;
worker_processes  auto;
worker_rlimit_nofile 51200;

events {
    use epoll;
    worker_connections  51200;
}


http {
    client_body_buffer_size 32k;
    client_header_buffer_size 2k;
    client_max_body_size 2m;
    default_type application/octet-stream;
    log_not_found off;
    server_tokens off;
    include       mime.types;
    gzip on;
    gzip_min_length  1k;
    gzip_buffers     4 16k;
    gzip_http_version 1.0;
    gzip_comp_level 2;
    gzip_types       text/plain text/css text/xml text/javascript application/x-javascript application/xml application/rss+xml application/xhtml+xml application/atom_xml;
    gzip_vary on;
    #error_page   500 502 503 504  /50x.html; 
    log_format  access  '$remote_addr - $remote_user [$time_local] "$request" '
              '$status $body_bytes_sent "$http_referer" '
              '"$http_user_agent" $http_x_forwarded_for';

    server {
        listen 80 default_server;
        server_name localhost api.voidking.com;
        root /home/wwwroot/;
        index index.php index.html index.htm;
    
        add_header Access-Control-Allow-Origin http://localhost;
        add_header Access-Control-Allow-Credentials true;

        location ~ \.php {
        root /home/wwwroot/;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
        fastcgi_split_path_info ^(.+\.php)(.*)$;
            fastcgi_param PATH_INFO $fastcgi_path_info;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            fastcgi_param  PHP_VALUE        open_basedir=$document_root:/tmp/:/proc/;
            include        fastcgi_params;
        }

    }

    include vhost/*.conf;
    
}

```

# 后记
至此，大功告成，session跨域问题完美解决。

# 书签
ThinkPHP框架实现session跨域问题的解决方法
http://www.jb51.net/article/51722.htm

ThinkPHP二级域名session共享问题
http://www.thinkphp.cn/topic/6380.html

php 跨域、跨子域，跨服务器读取session
http://blog.csdn.net/kylinbl/article/details/7634075

跨服务器Session共享的四种方法
http://blog.sina.com.cn/s/blog_5f3d71430100jv7q.html

Angular通过CORS实现跨域方案
https://my.oschina.net/blogshi/blog/303758
