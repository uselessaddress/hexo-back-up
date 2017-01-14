---
title: 用div模拟input和textarea
toc: true
date: 2016-06-18 16:08:02
tags:
- 前端
- css
categories: 设计开发
---
# 前言
先看需求：在输入评论的时候，当评论长度超过一行时，自动换行，同时评论框变高，效果如下。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/input/input0.jpg)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/input/input1.jpg)

然而，input并没有自动换行属性，也就无法实现这个效果；使用textarea，写js控制row？想想就不够优雅！

经评估，使用div模拟输入框，是最佳方案。

<!--more-->

# html
```
<div class="input-body">
    <div class="left">
        <div class="input">
            <p id="input-comment" contenteditable="true"></p>
            <p id="input-placeholder">请输入评论内容</p>
        </div>              
    </div>
    <div class="right">
        <span id="send">发送</span>
    </div>  
</div>
<div class="input-body-back"></div>
```

# scss
```
.input-body{
    position: fixed;
    border-top: 1px solid #e5e5e5;
    border-bottom: 1px solid #e5e5e5;
    width: 100%;
    bottom: 0;
    background: #f7f7f7;
    font-size: 1.2rem;
    .left{
        display: inline-block;
        vertical-align: bottom;
        width: 80%;
        text-align: center;
        padding: .5rem;
        .input{
            position: relative;
            #input-comment{
                display: inline-block;
                width: 92%;
                min-height: 2rem;
                line-height: 2rem;
                text-align: left;
                padding: .2rem .5rem;
                background: #fff;
                border: 1px solid #e5e5e5;
                border-radius: 3px;
                outline: 0;
            }   
            #input-placeholder{
                position: absolute;
                top: .5rem;
                left: .8rem;
                color: #8E8E93;
            }
        }       
    }
    .right{
        display: inline-block;
        vertical-align: bottom;
        width: 15%;
        height: 2.5rem;
        text-align: center;
        font-weight: bold;
        #send{
            margin-right: 1rem;
            color: #8E8E93;
            &.active{
                color: #146AF3;
            };
        }
    }
}
.input-body-back{
    height: 4rem;
}
```

# js
```
$('#input-comment').on('input propertychange',function(){
    var str = $(this).html();
    if(str == ''){
        $('#send').removeClass('active');
        $('#input-placeholder').show();
    }else{
        $('#send').addClass('active');
        $('#input-placeholder').hide();
    }
});

$('#send').on('click',function(){
    if(!$(this).hasClass('active')){
        return;
    }
    var comment = $('#input-comment').html();
    if(comment.length > 500){
        alert('字数过多，请删减');
        return;
    }
    // ajax请求
});
```


# 书签
div模拟textarea文本域轻松实现高度自适应
http://www.zhangxinxu.com/wordpress/2010/12/div-textarea-height-auto/