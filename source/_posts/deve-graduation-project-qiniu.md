---
title: 使用七牛管理图片
toc: true
date: 2016-06-20 19:20:41
tags:
- 毕设
- Node
- 七牛
categories: 设计开发
---
# 功能描述
在发布帖子界面，用户可以选择图片上传，图片上传成功后显示在发布帖子界面上，上传失败则提示“上传失败”。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/qiniu/loading.png)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/qiniu/show.png)

<!--more-->

# 原理
七牛是一个优秀的云存储平台，提供图片相关的缩略、剪裁、添加水印等功能。本系统的重心在于社区论坛，所以处理图片的工作就交给七牛了，避免重复造轮子。

在发布帖子页面，用户选择图片后，浏览器获取到图片数据imageData，发送给我的服务器，我的服务器转发图片数据给七牛服务器。上传成功后，七牛服务器返回数据给我的服务器，我的服务器返回数据给浏览器。

浏览器获得返回数据，如果获取到服务器返回的状态true和图片url，则把图片显示在页面上。否则，就把告知用户“上传失败”。

# 代码
## html
```
<div class="form-group">
    <label for="picture" class="col-sm-2 control-label">图片</label>
    <div class="col-sm-9">
        <!--<input id="image"  type="file" multiple>-->
        <input type="file" id="picture" style="display: none;" />
        <span class="icon-picture"></span>
        <span class="icon-loading"></span>
        <span class="addimage">添加图片</span>
    </div>
</div>
<div class="form-group">
    <div class="col-sm-offset-2 col-sm-9">
        <div class="pictures">
            <!--动态插入图片-->
        </div>
    </div>
</div>
```

## js
```
$('.icon-picture').click(function(){
    $('#picture').click();
    $('#picture').unbind().on('change',function(){
        var fileNumber = $('#picture').get(0).files.length;
        if(fileNumber==0){
            return;
        }
        var file = $('#picture').get(0).files[0];
        console.log(file);
        var fileReader = new FileReader();
        fileReader.readAsDataURL(file);
        fileReader.onload = function(e) {
//            var fileType = file.name.substring(file.name.lastIndexOf('.'), file.name.length);
//            var now = new Date();
//            var fileName = now.getTime() + 'langting' + parseInt(Math.random() * 20) + fileType;
            $(".addimage").text("上传中...");
            $('.icon-picture').hide();
            $('.icon-loading').css({
                'display': 'inline-block'
            });
            $.ajax({
                type: 'POST',
                dataType: 'json',
                url: '/qn-upload',
                data: {
                    imageData: e.target.result
                },
                success: function(data) {
                    if (data.state == true) {
                        setTimeout(function() {
                            $(".addimage").text("添加图片");
                            $('.icon-loading').hide();
                            $('.icon-picture').show();
                            var html = template('pic_template',data);
                            $('.pictures').append(html);
                        }, 1500);
                    } else {
                        alert("上传失败");
                    }
                },
                error: function(){

                }
            });
        }
    });
});

// 删除图片
$('.pictures').on('click','.icon-delete',function(){
    $(this).parents('.pic').remove();
});
```

## Node端
```
exports.qn_upload = function(req,res){
    var qn = require('qn');
    var client = qn.create({
        accessKey: 'JEBuh6qG9FPI6atoycgdoypwOZJWuzYk1YXnC-6c',
        secretKey: 'IBAa_7Mkj2_ROefIRcwVjcVEK9PVFDvzrtPiL9nO',
        bucket: 'forum',
        domain: 'http://7xstti.com2.z0.glb.clouddn.com'
    });

    var imageData = req.body.imageData;
    var key = uuid.v1();
    imageData = imageData.replace(/^data:image\/\w+;base64,/, "");
    var dataBuffer = new Buffer(imageData, 'base64');
    client.upload(dataBuffer, {
        key: key
    }, function(err, result) {
        if (err) {
            res.json({
                state: false,
                imgname: imageName,
                imgurl: "",
                imghash: ""
            });
        } else {
            res.json({
                state: true,
                imgname: result.key,
                imgurl: result.url,
                imghash: result.hash
            });
        }
    });
}
```

# 源码
https://github.com/voidking/nodeforum/blob/master/views/post/post-add.html
https://github.com/voidking/nodeforum/blob/master/public/js/post/post-add.js
https://github.com/voidking/nodeforum/blob/master/controllers/post.js

# 后记
上传的交互过程，是前端把图片数据传给Node端，Node端转发图片数据到七牛服务器。七牛服务器返回结果数据给Node端，Node端转发结果数据给前端。

这个过程比较麻烦，更好的做法，是前端能够直接把图片数据传给七牛服务器，七牛服务器返回结果给前端。按照七牛给的文档，是可以实现的，感兴趣的小伙伴请研读七牛的《JavaScript SDK使用指南》。

# 书签
七牛开发者中心
http://developer.qiniu.com/

JavaScript SDK使用指南
http://developer.qiniu.com/code/v6/sdk/javascript.html

七牛 Node.js SDK
https://www.npmjs.com/package/node-qiniu

File对象上传图片（nodejs版）
http://www.html-js.com/article/NodejsDemoDemo-go

图片上传时input file change事件多次触发解决
http://www.aichengxu.com/view/78921

用js获取上传前图片的宽高
http://bbs.csdn.net/topics/390768571

HTML5 之文件操作(file)
http://blog.csdn.net/oscar999/article/details/37499743

Using files from web applications
https://developer.mozilla.org/en-US/docs/Using_files_from_web_applications

图片基本处理 (imageView2)
http://developer.qiniu.com/code/v6/api/kodo-api/image/imageview2.html

保存裁剪后的图片的疑问
https://segmentfault.com/q/1010000003892579

[base64百度百科](http://baike.baidu.com/link?url=Dp2Jx27cpeyrnBSdIEIJK_DHMcPATWfzpcTKJ9j62v0J13LZE6jNkGWfmip9wvf-A_x_Fg4NToVVM8Z8r_avLK)

好文推荐：移动端图片格式调研
http://www.cocoachina.com/ios/20151201/14478.html

Plupload
http://www.plupload.com/
