title: 瀑布流特效
toc: true
date: 2016-01-28 20:55:17
tags:
- jQuery
- 前端
- 插件
categories: 设计开发
---
# 原理
假设页面上有三列图片，当我们下拉时，最短（或最长）的一列图片展示完的时候，就要请求加载新的图片，获取到的新的图片放到最短一列图片的下面。

尝试过用原生js或jquery实现瀑布流，源码放在本文最后。下面记录一种使用imageloaded和masonry插件实现的瀑布流，更简便。

<!--more-->

# imageloaded和masonry
ejs代码：
```
<div id="meitulist" class="meitulist">
    <% data.obj.pictureList.forEach(function(picture){ %>
        <a href="/inspiration/imagedetail/<%= picture.albumId%>" class="item">
            <img src="<%= picture.url%>" />
        </a>
    <% })%>
</div>
```

每次加载图片并插入后，执行resetImage()函数。

```
function resetImage(){
    var width = document.documentElement.clientWidth;
    var column = width>320? 3 : 2;
    var itemWidth = Math.ceil((width-(column+1)*10)/column);
    $('.meitulist .item').css('width',itemWidth);
    $('.meitulist .item img').css({
        'width':itemWidth
        // 'height': itemWidth
    })

    window.imagesLoaded('#meitulist', function() {
        var msnry = new Masonry('#meitulist',{
            'columnWidth': itemWidth,
            'itemSelector': '.item',
            'isAnimated':true,
            // 'percentPosition':true,
            'gutter': 10
        });
    });
}
```

# js和jquery源码
https://github.com/voidking/front-end-demo/tree/master/%E7%80%91%E5%B8%83%E6%B5%81

# 参考文档
瀑布流特效
http://www.cnblogs.com/Leo_wl/p/4306295.html

Masonry官网
http://masonry.desandro.com/

mosonry项目地址：
https://github.com/desandro/masonry

imagesLoaded官网
http://imagesloaded.desandro.com/

imagesloaded项目地址：
https://github.com/desandro/imagesloaded
