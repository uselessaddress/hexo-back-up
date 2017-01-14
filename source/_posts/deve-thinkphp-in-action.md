---
title: ThinkPHP实战
toc: true
date: 2016-08-28 22:15:11
tags:
- php
- thinkphp
categories: 设计开发
---
# 前言
本着“需要什么就学习什么”的原则，花了几天时间，实践了一下thinkphp框架的使用。整理了一下必须掌握的三个部分：路由控制、模板渲染、增删改查。

<!--more-->

# 准备
在《ThinkPHP开发环境搭建》中，我们已经准备好了thinkphp的开发环境。在此基础上，复制Application中的Home文件夹，重命名为Admin。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/thinkphp-in-action/copy.jpg)
然后，修改Admin/IndexController.class.php文件的namespace为：
```
namespace Admin\Controller;
```
好了，可以进入真正的开发了！

# 路由控制
路由控制，在thinkphp开发手册中对应“架构>URL模式”。thinkphp提供了多种URL模式，本次开发采用标准URL格式。
```
http://serverName/index.php/模块/控制器/操作
```

比如，我们在Admin/IndexController.class.php文件中添加函数：
```
public function firstFunc(){
    echo '第一个函数';
}
```
那么，这个函数的访问路径为`http://localhost/thinkphp/index.php/Admin/Index/firstFunc`。
访问该路径，我们可以在页面上看到返回值“第一个函数”。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/thinkphp-in-action/firstfunc.jpg)

# 模板渲染
模板渲染，在thinkphp开发手册中对应“视图>模板赋值和模板渲染”以及“模板”。
在Admin/View/Index文件夹下，新建文件template.html。在Admin/IndexController.class.php文件中添加函数：
```
public function template(){
    $data = '测试数据';
    $dataArr = array(
        array('name'=>'郝锦','age'=>'24'),
        array('name'=>'小帅','age'=>'22'),
        array('name'=>'小飞','age'=>'22')
    );
    $dataObj = new userInfo();
    $this->assign('data2',$data);
    $this->assign('dataArr',$dataArr);
    $this->assign('dataObj',$dataObj);
    $this->display();
}
```
同时，在文件最后添加一个userInfo类：
```
class userInfo{
    public $name = '郝锦';
    public $age = '24';
    function show(){
        echo '一个函数';
    }
}
```
那么，\$data、\$dataArr和\$dataObj都会通过assign函数传送到template.html页面。template.html页面代码如下：
```
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>模板</title>
</head>
<body>
    <h1>模板</h1>
    <p>{$data2}</p>
    <p>{$dataObj:name}</p>
    <p>{$dataObj:age}</p>
    <volist name='dataArr' id="item">
        {$item.name}&nbsp;{$item.age}<br/>
    </volist>
</body>
</html>
```

最终效果如下：
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/thinkphp-in-action/template.jpg)

注意，函数名和页面名字是相同的，一一对应的。

# 增删查改
增删查改，在thinkphp开发手册中对应“模型>GURD操作”和“专题>数据分页”。

## 数据库配置
利用navicat等工具连接到本地mysql数据库，创建数据库volunteer，在数据库中创建表volun_admin(int id, varchar name, vachar password)和volun_project(int id, varchar title, varchar content)。注意，编码格式选择utf8。

## php配置
在进行数据库增删查改之前，我们需要先连接到数据库。
打开Application/Common/Conf/config.php文件，修改内容如下：
```
<?php
return array(
    //'配置项'=>'配置值'
    'DB_TYPE'=>'mysql', //数据库类型
    'DB_HOST'=>'localhost', //服务器地址
    'DB_NAME'=>'volunteer', //数据库名
    'DB_USER'=>'root', //用户名
    'DB_PWD'=>'', //密码
    'DB_PORT'=>3306, //端口
    'DB_PREFIX'=>'volun_', //数据库表前缀
    'SHOW_PAGE_TRACE' =>true,
);
?>
```

## project表的增删查改
```
// 志愿项目增删查改
public function projectAdd($title, $content){
    $project = D('project');
    $data['title'] = $title;
    $data['content'] = $content;
    if($project->create($data)){
        $id = $project->add();
        if($id){
            $result = array(
                'code'=> '0',
                'ext'=> 'success'
            );
            echo json_encode($result);
        }
    }
}

public function projectEdit($id, $title, $content){
    $project = D('project');
    $data['title'] = $title;
    $data['content'] = $content;
    $success = $project->where("id='$id'")->save($data);
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

public function projectDelete($id){
    $project = D('project');
    $success = $project->where("id='$id'")->delete();
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

public function projectList(){
    $project = D('project');
    $resultArr = $project->select();
    echo json_encode($resultArr,JSON_UNESCAPED_UNICODE);
}

public function projectPage($pageSize,$pageNum){
    $project = D('project');
    $total = $project->count();
    $totalPage = $total%$pageSize ? (int)($total/$pageSize)+1 : (int)($total/$pageSize);
    
    $list = $project->page($pageNum.','.$pageSize)->select();

    if($list){
        $resultArr = array(
            'totalPage'=> $totalPage,
            'pageNum'=> $pageNum,
            'projectList'=> $list

        );
        $result = array(
            'code'=> '0',
            'ext'=> 'success',
            'obj'=> $resultArr
        );
        echo json_encode($result,JSON_UNESCAPED_UNICODE);
    }
}
```

以上方法，在js中，可以使用GET方式请求，也可以使用POST方式请求。

路由控制、模板渲染、增删改查，至此全部跑通，可以进行简单的开发了。至于thinkphp提供的其他更加强大的功能，在需要时查查文档就好。

# 接口测试
上次在给大家演示HttpRequester的时候，验证登录接口，一直出现错误。猜测有四种可能：
1、HttpRequester的问题
2、thinkphp的问题
3、php的问题
4、apache的问题

## HttpRequester的问题
以projectAdd方法为例，在js中使用ajax请求该方法，无论type是GET还是POST，都能成功拿到返回值`{"code":"0","ext":"success"}`。

在HttpRequester中，使用GET方式请求该方法，可以拿到返回值；但是，使用POST方式请求，拿不到返回值，提示“参数错误或者未定义:title”，因为projectAdd方法没有拿到我们传过去的参数。

修改projectAdd方法为：
```
public function projectAdd(){
    $title = $_POST['title'];
    $content = $_POST['content'];
    $project = D('project');
    $data['title'] = $title;
    $data['content'] = $content;
    if($project->create($data)){
        $id = $project->add();
        if($id){
            $result = array(
                'code'=> '0',
                'ext'=> 'success'
            );
            echo json_encode($result);
        }
    }
}
```

再次使用HttpRequester请求projectAdd方法，这次换了个错误：
```
Column 'content' cannot be null [ SQL语句 ] : INSERT INTO `volun_project` (`title`,`content`) VALUES (NULL,NULL)
```

显然，错误的原因依然是projectAdd方法没有拿到我们传过去的参数，导致title和content为空。

使用ajax的POST方式请求该方法，成功拿到返回值。可见，这是HttpRequester的问题，它无法模拟ajax的POST请求。

那么问题来了，ajax的POST和HttpRequester的POST到底有什么区别？很遗憾，暂时没有找到答案。

## thinkphp的问题
在公司开发时，一直使用HttpRequester来验证接口，能用ajax请求的接口，都可以使用HttpRequester来模拟请求。这样看来，就是接口的问题，也就是thinkphp框架的问题！或者，就是php的问题！

## php的问题
在thinkphp文件下，新建了一个test.php文件：
```
<?php
    $title = $_POST['title'];
    $array = array(
        'title'=>$title
    );
    echo json_encode($array);
?>
```
在ajax中，带入参数`{title:'测试标题'}`，POST请求`http://localhost/thinkphp/test.php`，成功拿到返回值。

在HttpRequester中，带入参数`{title:'测试标题'}`，POST请求`http://localhost/thinkphp/test.php`，提示错误“Notice: Undefined index: title in D:\Server\wamp64\www\thinkphp\test.php on line 2”。

修改test.php文件为：
```
<?php
    $title = $_GET['title'];
    $array = array(
        'title'=>$title
    );
    echo json_encode($array);
?>
```
在ajax中，带入参数`{title:'测试标题'}`，GET请求`http://localhost/thinkphp/test.php`，成功拿到返回值。

在HttpRequester中，带入参数`{title:'测试标题'}`，GET请求`http://localhost/thinkphp/test.php`，成功拿到返回值。

由此排除thinkphp的问题。

## apache的问题
还有一种可能，apache的问题，于是把test.php文件放到nginx下，经测试，接口同样拿不到参数。
由此排除apache的问题。

## 小结
综上，HttpRequester无法测试thinkphp的POST接口问题，有两个可能，一个是HttpRequester存在缺陷，另一个是php存在缺陷。


# 后记
无论哪种原因，总之，无法使用HttpRequester的POST请求来验证我们的thinkphp接口了。需要验证接口时，暂时使用GET请求，因为我们的接口两种请求方式都可以拿到返回值。

注意，本应该是POST请求的接口，接口文档中的请求类型依然写POST，ajax的type也使用POST，保证规范性。


# 2016.09.14更新
Thinkphp3.2.3 接收不到json数据
https://segmentfault.com/q/1010000004980405

原来，造成HttpRequester无法测试接口的原因，是Content-Type没有选对。

由于PHP默认只识别`Content-Type: application/x-www.form-urlencoded`标准的数据类型，因此，对型如`application/json`的内容无法解析为`$_POST`数组，故保留原型，交给`$GLOBALS['HTTP_RAW_POST_DATA']`来接收。

`file_get_contents('php://input')`允许读取 POST 的原始数据，和`$GLOBALS['HTTP_RAW_POST_DATA']`比起来，它给内存带来的压力较小，并且不需要任何特殊的php.ini设置。`file_get_contents('php://input')`不能用于`enctype="multipart/form-data"`。


# 书签
序言 - ThinkPHP3.2完全开发手册
http://document.thinkphp.cn/manual_3_2.html

ThinkPHP框架教程 - 猿团
http://edu.yuantuan.com/course/explore/thinkphp

PHP: 语言参考 - Manual
http://php.net/manual/zh/langref.php
