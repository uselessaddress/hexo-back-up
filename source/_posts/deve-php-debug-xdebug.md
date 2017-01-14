---
title: PHP调试之Xdebug
toc: true
date: 2016-08-16 10:09:37
tags:
- php
- 调试
- xdebug
categories: 设计开发
---
# 前言
该怎么强调调试的重要性呢？我们生病的时候，要去看医生，医生会通过各种仪器对我们进行检查，定位病因，然后给我们治疗。程序也会生病，生病的时候，作为医生（开发者）的我们，就要通过各种办法定位bug，然后修改代码。定位bug并且修改代码的过程，就是调试。

PHP的调试工具有很多，本文记录一下Xdebug的使用方法。

<!--more-->

# Xdebug
## 安装配置
1、官网下载：https://xdebug.org/download.php ，需要注意的是，不要下载带nts的版本，否则会报错找不到php5.dll。
2、将下载的文件重命名php_xdebug.dll。
3、剪切php_xdebug.dll到php的ext目录下。
4、编辑php.ini文件，加入如下信息：
```
[Xdebug]
zend_extension="D:\Server\wamp64\bin\php\php5.6.19\ext\php_xdebug.dll"
xdebug.remote_enable=on
xdebug.remote_handler="dbgp"
xdebug.remote_host="127.0.0.1"
xdebug.remote_port=9001
```

## demo
《ThinkPHP开发环境搭建》中，我们已经搭建好了thinkphp的开发环境。在此基础上，我们在thinkphp/ThinkPHP文件夹下，新建文件index.php，内容如下：
```
<?php
    require 'abc.php';
?>
```

重启WAMP，在浏览器地址栏中输入：`http://localhost/thinkphp/ThinkPHP`，可以看到出错信息变成了彩色。

## xdebugclient
以上，已经可以看到详细的错误信息。但是，这时我们还不能打断点，需要借助一个IDE才能。幸运的是，sublime就可以充当这个IDE。

1、安装插件xdebugclient。
2、打开需要调试的程序。
3、选择sublime导航的Project，save project as，生成一个.sublime-project的文件，修改其为：
```
{
    "folders":
    [
        {
            "follow_symlinks": true,
            "path": "D://Server//wamp64//www//thinkphp"
        }
    ],
    "settings":
    {
        "xdebug":
        {
            "close_on_stop": true,
            "path_mapping":
            {
            },
            "port": 9001,
            "super_globals": true,
            "url": "http://localhost/thinkphp/ThinkPHP/index.php"
        }
    }
}
```

5、打开index.php，ctrl+f8，添加一个断点。
6、ctrl+shift+p，输入xdebug，选择start debugging(launch browser)。
## xdebug helper

chrome浏览器安装xdebug helper，安装地址为：https://chrome.google.com/webstore/detail/eadndfjplgieldjbigjakmdgkmoaaaoc ，IDE key配置为other，sublime.xdebug。

# 后记
最终，安装失败，打断点不生效。后来发现，WAMP集成环境，php.ini有两份，php下面有一份，apache下面有一份。生效的那一份在apache下面，路径为`D:\Server\wamp64\bin\apache\apache2.4.18\bin\php.ini`，而且，已经集成好了xdebug。修改配置，依然无效，心中一万只草泥马奔腾而过。。。未完待续。。。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/xdebug/xdebug0.jpg?imageView2/0/w/700)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/xdebug/xdebug1.jpg?imageView2/0/w/700)

# 书签
有哪些 PHP 调试技巧？
https://www.zhihu.com/question/20348619

如何调试PHP程序
http://www.howzhi.com/group/php/discuss/468

Xdebug文档
https://xdebug.org/docs/

php使用Xdebug进行调试
http://www.phpddt.com/php/xdebug.html

用 Xdebug 修正 PHP 应用程序中的错误
http://www.ibm.com/developerworks/cn/opensource/os-php-xdebug/index.html

Xdebug 配置
http://www.cnblogs.com/dreamhome/p/3218744.html

神器sublime2配置xdebug调试PHP
http://lobert.iteye.com/blog/2068638

