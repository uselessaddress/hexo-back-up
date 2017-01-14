---
title: ThinkPHP登录状态记录
toc: true
date: 2016-08-28 22:48:56
tags:
- php
- thinkphp
categories: 设计开发
---
# 前言
登录状态的记录，两种思路：一种是在前端cookie中存一个token，每次请求都把token传给后端，用于验证用户身份；另一种是建立session，在session中保存用户信息。

本次开发中，我们采用第二种思路，下面实现一个简单的逻辑：
1、在地址栏输入后台首页地址，如果管理员未登录，则显示后台首页（登录页面）；如果管理员已登录，则跳转到内容管理页面。
2、在地址栏输入内容管理页面地址，如果管理员未登录，则跳转到后台首页；如果管理员已登录，则显示内容管理页面。

<!--more-->

# php代码
```
// 后台首页
public function index(){
    if(isset($_SESSION['admin'])){
        $url = 'http://localhost/volunteer/index.php/Admin/manage';
        header("location: $url");
    }else{
        $this->display();
    }
}

// 登录函数
public function login($name, $password){
    // $name = $_POST['name'];
    // $password = $_POST['password'];
    $admin = M('admin');
    $data = $admin->where("name='$name' AND password='$password'")->find();
    if($data){
        $_SESSION['admin']=$name;
        $result = array(
            'code' => '0',
            'ext' => '登录成功',
            'adminName' => $_SESSION['admin']
        );
        echo json_encode($result);
    }else{
        $result = array(
            'code' => '1',
            'ext' => '用户名或密码错误'
        );
        echo json_encode($result);
    }
}

// 管理页面
public function manage(){
    if(!isset($_SESSION['admin']) || !$_SESSION['admin']){
        $url = 'http://localhost/volunteer/index.php/Admin/Index';
        header("location: $url");
    }else{
        $this->display();
    }
}
```

# js代码
```
$(function(){
    $('#confirm').click(function(e){
        e.preventDefault();
        var name = $('#name').val();
        var password = $('#password').val();
        $.ajax({
            url: '/volunteer/index.php/Admin/Index/login',
            type: 'POST',
            dataType: 'json',
            data: {
                name: name,
                password: password
            },
            success: function(data){
                console.log(data);
                if(data.code == 0){
                    window.location = '/volunteer/index.php/Admin/Index/manage';
                }
            },
            error: function(xhr){
                console.log(xhr);
            }
        });
    }); 
});
```

# 书签
起步 · Bootstrap v3 中文文档
http://v3.bootcss.com/getting-started/#examples