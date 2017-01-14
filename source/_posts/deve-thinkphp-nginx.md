---
title: thinkphp部署到nginx服务器
toc: true
date: 2016-11-13 18:18:15
tags:
- thinkphp
- nginx
- centos
categories: 设计开发
---
# 前言
nginx默认情况下不支持pathinfo模式，从而不能支持ThinkPHP。能访问的，只有首页，其他函数的路径，都无法访问。

<!--more-->

# nginx配置支持pathinfo
首先，查看nginx配置文件的位置，`ps aux | grep nginx`。
然后，进入配置文件所在文件夹，备份配置文件`cp nginx.conf nginx.conf_mybak`。

原nginx.conf内容如下：
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
        server_name localhost;
        root /home/wwwroot/;
        index index.php index.html index.htm;

        location ~ \.php$ {
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            fastcgi_param  PHP_VALUE        open_basedir=$document_root:/tmp/:/proc/;
            include        fastcgi_params;
        }

    }

    include vhost/*.conf;
    
}
```

修改nginx.conf的server部分如下：
```
server {
    listen 80 default_server;
    server_name localhost;
    root /home/wwwroot/;
    index index.html index.htm index.php;

    error_page 404 /404.html;
    location = /404.html {
        return 404 'Sorry, File not Found!';
    }
    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
        root /usr/local/nginx/html; # windows用户替换这个目录
    }

    location / {
        try_files $uri @rewrite;
    }

    location @rewrite {
        set $static 0;
        if  ($uri ~ \.(css|js|jpg|jpeg|png|gif|ico|woff|eot|svg|css\.map|min\.map)$) {
            set $static 1;
        }

        if ($static = 0) {
            rewrite ^/(.*)$ /index.php?s=/$1;
        }

    }

    location ~ /Uploads/.*\.php$ {
        deny all;
    }

    location ~ \.php/ {
       if ($request_uri ~ ^(.+\.php)(/.+?)($|\?)) { }
       fastcgi_pass 127.0.0.1:9000;
       include fastcgi_params;
       fastcgi_param SCRIPT_NAME     $1;
       fastcgi_param PATH_INFO       $2;
       fastcgi_param SCRIPT_FILENAME $document_root$1;
    }

    location ~ \.php$ {
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.ht {
        deny  all;
    }
}
```

最后，重启nginx。`cd /usr/local/nginx/sbin`，`./nginx -s reload`。

访问`http://192.168.56.101`，提示No input file specified.。出错了？不过没有太大影响。

输入函数完整路径，`http://192.168.56.101/thinkphp/index.php/Home/Index/getInfo`，获取信息成功！至此，nginx配置支持pathinfo成功！

# nginx配置支持跨域
紧接着上面的nginx.conf配置文件修改，添加一行即可：
```
add_header Access-Control-Allow-Origin *;
location / {
    try_files $uri @rewrite;
}
```


# No input file specified
紧接着上面的nginx.conf配置文件修改，添加一行即可：
```
http {
    # 其他配置省略
    fastcgi_intercept_errors on;
    include vhost/*.conf;
}
```

# 后记
以上配置，nginx已经支持pathinfo模式，而且支持跨域，而且404报错正常。

# 2016.11.20更新
同样的配置，放在阿里云的CentOS6.5上居然报错！无奈，寻找另外一种配置nginx支持pathinfo模式的方法。只需要在nginx.conf初始配置的基础上，修改四个地方即可：
```
location ~ \.php { #去掉$
    root /home/wwwroot/; #增加这一句
    fastcgi_pass   127.0.0.1:9000;
    fastcgi_index  index.php;
    fastcgi_split_path_info ^(.+\.php)(.*)$; #增加这一句
    fastcgi_param PATH_INFO $fastcgi_path_info; #增加这一句
    fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    fastcgi_param  PHP_VALUE        open_basedir=$document_root:/tmp/:/proc/;
    include        fastcgi_params;
}
```

添加跨域支持：
```
add_header Access-Control-Allow-Origin *;
```

域名绑定：
```
server_name localhost api.voidking.com;
```
然后，在万网添加域名解析A记录到阿里云主机ip地址。

# 书签
最完美ThinkPHP nginx 配置文件
https://my.oschina.net/zhuyajie/blog/523268

Nginx跨域配置，支持DELETE,PUT请求
http://to-u.xyz/2016/06/30/nginx-cors/

Nginx执行php显示no input file specified的处理方法
http://www.ahlinux.com/nginx/3984.html

nginx指定404 No input file specified
http://coolnull.com/238.html

简单配置nginx使之支持pathinfo
http://www.thinkphp.cn/topic/3228.html

