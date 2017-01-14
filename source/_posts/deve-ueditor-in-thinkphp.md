---
title: 把UEditor整合进ThinkPHP
toc: true
date: 2016-09-09 12:31:46
tags:
- php
- thinkphp
- ueditor
- 图片上传
- 富文本编辑器
categories: 设计开发
---
# 前言
> UEditor是由百度web前端研发部开发所见即所得富文本web编辑器，具有轻量，可定制，注重用户体验等特点，开源基于MIT协议，允许自由使用和修改代码...

<!--more-->

巾帼志愿者项目，需要发布一些带图片的文章，使用UEditor很合适。同时，UEditor还可以当做图片上传插件使用。下面，我们把UEditor整合进ThinkPHP框架中。

# 准备
1、登录UEditor官网，下载PHP版本（UTF-8版）的代码。
2、解压文件，重命名文件夹utf8-php为ueditor。
3、拷贝ueditor到项目中的Public/libs文件夹下。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/ueditor-in-thinkphp/tree.jpg)
4、修改ueditor/php/config.json里的路径。

# 编辑器
## 新建表
在数据库中新建表volun_culture，content字段的类型选择longtext。

## Controller
在Application/Admin/Controller/TestController.class.php中，添加函数ueditor、cultureAdd和ueditorshow。
```
public function ueditor(){
    $this->display();
}

public function cultureAdd($title, $content){
    $data['title'] = $title;
    $data['content'] = $content;
    $culture = D('culture');
    if($culture->create($data)){
        $id = $culture->add();
        if($id){
            $data = array(
                'code'=>'0',
                'id'=>$id
            );
            echo json_encode($data);
        }
    }
}

public function ueditorshow(){
    $culture = D('culture');
    $cultureArr = $culture->select();
    $this->assign('cultureArr',$cultureArr);
    $this->display();
}

```


## 页面
在Application/Admin/View/Test文件夹中，新建文件ueditor.html和ueditorshow.html。
```
<!--ueditor.html-->
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>UEditor</title>
</head>
<body>
    <!-- 加载编辑器的容器 -->
    <script id="container" name="content" type="text/plain">
        这里写你的初始化内容
    </script>
    <button id="getContent">获取内容</button>
    <button id="saveContent">保存</button>
    <script src="/volunteer/Public/libs/jquery/jquery.min.js"></script>
    <!-- 配置文件 -->
    <script type="text/javascript" src="/volunteer/Public/libs/ueditor/ueditor.config.js"></script>
    <!-- 编辑器源码文件 -->
    <script type="text/javascript" src="/volunteer/Public/libs/ueditor/ueditor.all.js"></script>
    <!-- 实例化编辑器 -->
    <script type="text/javascript">
        $(function(){
            var ue = UE.getEditor('container');
            $('#getContent').click(function(){
                var html = ue.getContent();
                alert(html);
            });

            $('#saveContent').click(function(){
                var html = ue.getContent();
                var param = {
                    title: '测试',
                    content: html
                };
                $.ajax({
                    url: '/volunteer/index.php/Admin/Test/cultureAdd',
                    type: 'POST',
                    dataType: 'json',
                    data: param,
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
显示效果如下：
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/ueditor-in-thinkphp/ueditor.jpg)
点击“获取内容”：
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/ueditor-in-thinkphp/alert.jpg)
我们可以看到，上传的图片路径为`/volunteer/Public/libs/ueditor/php/upload/image/*`，这个路径是我们在准备工作的第四步中配置的。查看一下该路径，果然可以找到上传的图片。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/ueditor-in-thinkphp/image.jpg)
点击“保存”：
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/ueditor-in-thinkphp/ajax.jpg)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/ueditor-in-thinkphp/database.jpg)


```
<!--ueditorshow.html-->
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>文章展示</title>
</head>
<body>
    <volist name="cultureArr" id="item">
        <div class="article">
            <h2>{$item.title}</h2>
            {$item.content}
        </div>
        <hr/>
    </volist>
</body>
</html>
```

显示效果如下：
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/ueditor-in-thinkphp/ueditorshow.jpg)


# 图片上传插件
## Controller
在Application/Admin/Controller/TestController.class.php中，添加函数imgupload。
```
public function imgupload(){
    $this->display();
}
```

## 页面
在Application/Admin/View/Test文件夹中，新建文件imgupload.html。
```
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>图片上传</title>
</head>
<body>
    <tr>
        <th>上传图片</th>
        <td>
            <input type="text" id="path" />
            <input type="button" id="addPic" value="上传图片"/>
        </td>
    </tr>
    <div id="myeditor" style="display: none;"></div>

<js file="__ROOT__/Public/libs/ueditor/ueditor.config.js"/>
<js file="__ROOT__/Public/libs/ueditor/ueditor.all.js"/>
<js file="__ROOT__/Public/libs/jquery/jquery.min.js"/>
<script>
    $(function(){
        var editor = UE.getEditor('myeditor');
        editor.ready(function () {
            //editor.setDisabled();
            editor.hide();
            editor.addListener('beforeInsertImage', function (t, arg) {
                $("#path").val(arg[0].src);
                //$("#preview").attr("src", arg[0].src);
                console.log(arg);
            });
        });
        $('#addPic').click(function(){
            var myImage = editor.getDialog("insertimage");
            myImage.open();
        });
    });
</script>
</body>
</html>
```

# UEditor优化
## 上传图片弹框延迟
问题描述：在最新版Chrome浏览器中，点击上传图片时，等待非常久，大概10秒左右，才弹出选择图片的对话框。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/ueditor-in-thinkphp/upload.jpg)

解决方案：
打开ueditor.all.js，找到：
```
accept="image/*"
```
修改为：
```
accept="image/jpg,image/jpeg,image/png,image/gif"
```

## 点击选择图片弹框延迟
问题描述：和上传图片弹框延迟基本相同，但是，点击的按钮有所差别。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/ueditor-in-thinkphp/upload2.jpg)

解决方案：
打开dialogs/images/images.js，找到：
```
accept: {
    title: 'Images',
    extensions: acceptExtensions,
    mimeTypes: 'image/*'
}
```
修改为：
```
accept: {
    title: 'Images',
    extensions: acceptExtensions,
    mimeTypes: 'image/jpg,image/jpeg,image/png,image/gif'
}
```

# 后记
为什么我会知道修改哪个js文件？两个原因：

第一，查找资料，资料指明了修改ueditor.all.js（ueditor.all.min.js）和webuploader.js（webuploader.min.js）。

第二，按照资料给的方法，并没有解决我的问题。仔细研读资料，关键在于accept参数的修改。问题没有解决，极大的可能是accept参数没有修改正确。检查后，确信accept参数已经修改正确。清掉缓存，确保修改后的代码生效。然而，问题依然没有解决。那么，只剩下一种可能，版本问题，accept参数也许移动到其他js中了。

右键“单击选择图片”按钮，检查，选择Event Listener，便可以定位到相关的js。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/ueditor-in-thinkphp/check.jpg)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/ueditor-in-thinkphp/event.jpg)


# 书签
UEditor - 首页
http://ueditor.baidu.com/website/

UEditor Docs
http://fex-team.github.io/ueditor/#start-config

Web Uploader
http://fex.baidu.com/webuploader/

WebUploader UEditor chrome 点击上传文件选择框会延迟几秒才会显示 反应很慢
http://www.cnblogs.com/liangjiang/p/5799984.html

FAQ · fex-team/ueditor Wiki · GitHub
https://github.com/fex-team/ueditor/wiki/FAQ
