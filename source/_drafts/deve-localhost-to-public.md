---
title: 本地服务发布到公网
toc: true
date: 2017-03-25 10:00:00
tags:
- nat
- 反向代理
- nginx
- 内网穿透
categories: 设计开发
---
# 前言
一台本地服务器上的一个服务发布到公网，最简单的办法，参考《微信本地调试》。但是，想要多台本地服务器上的多个服务发布到公网，就需要NAT-DDNS技术了。

<!--more-->

# 架构
假设我们有两个网站，分别是wiki.pandawork.net和jira.pandawork.net，分别部署在本地的两个服务器192.168.199.101的8090端口和192.168.199.102的8080端口。
![]()

# DNS解析
假设阿里云的公网IP为112.74.34.125。
1、登录pandawork.net所在的域名解析网站[dnspod](https://www.dnspod.cn/Login)。
2、添加两个A记录类型，分别是wiki和jira，记录值统一为112.74.34.125。

# 阿里云nginx配置
1、nginx.conf
```
user  root;
worker_processes  3;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        off;

    keepalive_timeout  65;

    server_names_hash_bucket_size 512; 
    
    log_format access '$remote_addr - $remote_user [$time_local] "$request" '
    '$status $body_bytes_sent "$http_referer" '
    '"$http_user_agent" $http_x_forwarded_for';
    
    map $http_upgrade $connection_upgrade {
        default upgrade;
        ''      close;
    }

    include /opt/nginx/conf/sitesconfig/*.conf;
}
```

2、sitesconfig/wiki.pandawork.net.conf
```
server {
    listen 80;
    server_name wiki.pandawork.net;
    charset utf-8;
    location /{
        proxy_set_header   Host             $host;
        proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header  X-Forwarded-For  $proxy_add_x_forwarded_for;
        client_max_body_size       1024m;
        client_body_buffer_size    128k;
        client_body_temp_path      data/client_body_temp;
        proxy_connect_timeout      90;
        proxy_send_timeout         90;
        proxy_read_timeout         90;
        proxy_buffer_size          4k;
        proxy_buffers              4 32k;
        proxy_busy_buffers_size    64k;
        proxy_temp_file_write_size 64k;
        proxy_temp_path            data/proxy_temp;
        
        proxy_pass http://zhuoyin.7766.org:8181;
    }
}
```

3、sitesconfig/jira.pandawork.net.conf
```
server {
    listen 80;
    server_name jira.pandawork.net;
    charset utf-8;
    location /{
        proxy_set_header   Host             $host;
        proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header  X-Forwarded-For  $proxy_add_x_forwarded_for;
        client_max_body_size       1024m;
        client_body_buffer_size    128k;
        client_body_temp_path      data/client_body_temp;
        proxy_connect_timeout      90;
        proxy_send_timeout         90;
        proxy_read_timeout         90;
        proxy_buffer_size          4k;
        proxy_buffers              4 32k;
        proxy_busy_buffers_size    64k;
        proxy_temp_file_write_size 64k;
        proxy_temp_path            data/proxy_temp;
        
        proxy_pass http://zhuoyin.7766.org:8182;
    }
}
```

# DDNS配置
1、登录[公云](http://www.pubyun.com)，进行DDNS配置。

2、创建动态域名zhuoyin.7766.org。

# 路由器配置
## 方法一
1、安装lynx
2、启用lynx
```
lynx -mime_header -auth=用户名:密码 "http://members.3322.net/dyndns/update?system=dyndns&hostname=zhuoyin.7766.org"
```

## 方法二
使用客户端：http://www.pubyun.com/products/dyndns/help/dyn/

# 本地服务器nginx配置


# 后记

# 书签
nat 详解
http://bbs.51cto.com/thread-878322-1.html

思科路由器NAT配置详解
http://yuan2.blog.51cto.com/446689/95209

思科，NAT配置详解
http://sunjie123.blog.51cto.com/1263687/1673697

路由器NAT功能配置简介
http://www.linkwan.com/gb/routertech/routerbase/nat.htm

NAT配置举例
http://www.hillstonenet.com/support/5.0/cn/SG/config_nat_exam.html

网络基本功（十九）：细说NAT原理与配置
https://wizardforcel.gitbooks.io/network-basic/content/18.html

NAT简单实例，教会你如何配置访问内部开发环境
http://blog.csdn.net/zhouhoujia/article/details/50474202

正向代理与反向代理的区别【Nginx读书笔记】
http://blog.csdn.net/m13666368773/article/details/8060481

搭建nginx反向代理用做内网域名转发
http://www.ttlsa.com/nginx/use-nginx-proxy/

在阿里云上部署Nginx实现反向代理
https://edu.cloudcare.cn/courses/368ea51822484ee1af1392dceecd38c8/detail

外网访问内网路由器-3种实现方法
http://www.nat123.com/Pages_8_267.jsp

公云
http://www.pubyun.com/


