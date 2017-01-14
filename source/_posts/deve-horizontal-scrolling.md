---
title: 横向滚动效果
toc: true
date: 2016-06-24 17:57:50
tags: 
- 前端
- JavaScript
categories: 设计开发
---
html部分：
```
<div class="tab-body">
    <ul>
        <li data-key="0">全部</li>
        <li data-key="1">攻略</li>
        <li data-key="2">案例</li>
        <li data-key="3">故事会</li>
        <li data-key="4">特别策划</li>
        <li data-key="5">喜舍杯</li>
        <li data-key="6">家居轶事</li>
    </ul>
</div>
```

<!--more-->

sass部分：
```
.tab-body{
    background: #fff;
    overflow-x: scroll;
    ul{
        width: 1000%;
        li{
            display: inline-block;
            vertical-align: top;
            height: 3.5rem;
            line-height: 3.5rem;
            font-size: 1.3rem;
            color: #666;
            padding: 0 .8rem 0 .8rem;
            margin-right: 1.8rem;
            &.active{
                border-bottom: 4px solid #FF5500;
                color: #FF5500;
            }
        }
    }
}
```

js部分：
```
// 初始type是全部
$('.tab-body li').first().addClass('active');
var type = 0;
// 设置导航条宽度
var totalWidth = 0;
$('.tab-body li').each(function(index, el) {
    totalWidth += $(el).width()+parseInt($(el).css('margin-right'));
});
$('.tab-body ul').css({
    'width': (totalWidth + 50)+ 'px'
});
// 切换type
$('.tab-body li').on('click',function(){
    $('.tab-body li').removeClass('active');
    $(this).addClass('active');
    type = $(this).attr('data-key');
    var param = {
        articleType: type,
        pageNo: 1,
        pageSize: 4
    }
    $.ajax({
        url: '/inspiration/home/api',
        type: 'POST',
        dataType: 'json',
        data: param,
        success: function(data){
            //把新获得的数据插入到页面
        },
        error: function(xhr){
            console.log(xhr)
        }
    });         
});
```

# 效果图
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/horizontal-scrolling.jpg)