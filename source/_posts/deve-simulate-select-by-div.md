---
title: 用div模拟select
toc: true
date: 2016-07-30 16:08:02
tags:
- 前端
- css
categories: 设计开发
---
# 前言
四不四傻？有select不用，干嘛要用div来模拟select呢？下面来看一个问题：

请问不使用chosen等插件，也不使用div模拟select，通过html和css，有没有办法限制select下拉框的高度。默认显示20条option，我想改成5条该怎么处理？
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/select/select.png)

答案是，无解！使用插件？找了十几款selectbox插件，都不满意！要么封装起来麻烦，要么根本不提供限制下拉框长度的功能。

评估了一下，还是自己模拟一个select选择框更靠谱。

<!--more-->

# select日期选择
## html
```
<div class="birthday">
    <label>生日：</label>
    <select name="month" id="month">
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
        <option value="4">4</option>
        <option value="5">5</option>
        <option value="6">6</option>
        <option value="7">7</option>
        <option value="8">8</option>
        <option value="9">9</option>
        <option value="10">10</option>
        <option value="11">11</option>
        <option value="12">12</option>
    </select>月
    <select name="day" id="day">
    </select>日
</div>
```

## js
```
$('#month').change(function(){
    changeDay();
});

function changeDay(){
    var $day = $('#day');
    var month = $('#month').val();
    var data = [];          
    if(month==2){
        for (var i = 1; i <= 29; i++) {
            data.push(i);
        }
    }else if(month==1 || month==3 || month==5 || month==7 || month==8 || month==10 || month==12){
        for (var i = 1; i <= 31; i++) {
            data.push(i);
        }
    }else if(month==4 || month==6 || month==9 || month==11){
        for (var i = 1; i <= 30; i++) {
            data.push(i);
        }
    }
    var html = '';
    data.forEach( function(element, index) {
        html += '<option value='+element+'>'+element+'</option>';
    });
    $day.html(html);
    $day.find('option').first().checked;
}
```

# div日期选择
## html
```
<div class="birthday2">
    <label>生日：</label>
    <div class="simulate-select" id="month-box">
        <input class="input" name="month2" type="hidden" value="1">
        <div class="check">1</div>月
        <ul class="items">
            <li>1</li>
            <li>2</li>
            <li>3</li>
            <li>4</li>
            <li>5</li>
            <li>6</li>
            <li>7</li>
            <li>8</li>
            <li>9</li>
            <li>10</li>
            <li>11</li>
            <li>12</li>
        </ul>
    </div>
    <div class="simulate-select" id="day-box">
        <input class="input" name="day2" type="hidden" value="1">
        <div class="check">1</div>日
        <ul class="items">
        </ul>
    </div>
</div>
```

## scss
```
.simulate-select{
    position: relative;
    display: inline-block;
    .check{
        display: inline-block;
        width: 40px;
        text-align: center;
        border: 1px solid #333;
    }
    .items{
        display: none;
        list-style: none;
        position: absolute;
        top: 100%;
        left: 0;
        height: 100px;
        overflow-y: auto;
        margin: -1px 0 0 0;
        padding: 0;
        width: 40px;
        text-align: center;
        border: 1px solid #333;
        background: #fff;
        li{
            &.active{
                background: #eee;
                color: #fff;
            }
        }
        &.active{
            display: block;
        }
    }
}
```

## js
```
selectMonth2();
changeDay2();
selectDay2();

function selectMonth2(){
    var self = this;
    var $month = $('#month-box');
    $('#month-box').on('click','.check',function(event){
        $('#day-box').find('.items').removeClass('active');
        if($month.find('.items').hasClass('active')){
            $month.find('.items').removeClass('active');
        }else{
            $month.find('.items').addClass('active');
        }
        event.stopPropagation();
    });
    $('#month-box').on('click','li',function(){
        var month = $(this).html();
        $month.find('.input').val(month);
        $month.find('.check').html(month);
        $month.find('.items').removeClass('active');

        changeDay2();
    });
    $(document).click(function(){
        $month.find('.items').removeClass('active');
    });
}

function changeDay2(){
    var $month = $('#month-box');
    var $day = $('#day-box');
    var month = $month.find('.input').val();
    var data = [];      
    if(month==2){
        for (var i = 1; i <= 29; i++) {
            data.push(i);
        }
    }else if(month==1 || month==3 || month==5 || month==7 || month==8 || month==10 || month==12){
        for (var i = 1; i <= 31; i++) {
            data.push(i);
        }
    }else if(month==4 || month==6 || month==9 || month==11){
        for (var i = 1; i <= 30; i++) {
            data.push(i);
        }
    }
    var html = '';
    data.forEach( function(element, index) {
        html += '<li>'+element+'</li>';
    });
    $day.find('.items').html(html);
    $day.find('.check').html('1');
}

function selectDay2(){
    var $day = $('#day-box');
    $('#day-box').on('click','.check',function(event){
        $('#month-box').find('.items').removeClass('active');
        if($day.find('.items').hasClass('active')){
            $day.find('.items').removeClass('active');
        }else{
            $day.find('.items').addClass('active');
        }
        event.stopPropagation();
    });
    $('#day-box').on('click','li',function(){
        var day = $(this).html();
        $day.find('.input').val(day);
        $day.find('.check').html(day);
        $day.find('.items').removeClass('active');
    });
    $(document).click(function(){
        $day.find('.items').removeClass('active');
    });
}

```

## 效果图
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/select/select-box.jpg)

# 源码
https://github.com/voidking/nodebase/blob/master/views/weixin/love.html
https://github.com/voidking/nodebase/blob/master/public/scss/weixin/love.scss
https://github.com/voidking/nodebase/blob/master/public/js/weixin/love.js

# 后记
我相信，一定有更简单的方法实现限制select下拉框的高度。如果在调试时，可以捕捉到下拉框，就可以设置该下拉框的高度了。但是，尝试了各种办法都没有捕捉到，奇怪。不管了，暂时这样，以后发现了更好的解决办法再补上。