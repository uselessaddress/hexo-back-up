title: 仿淘宝图片放大器
toc: true
date: 2016-01-29 20:55:17
tags:
- jQuery
- 前端
- 插件
categories: 设计开发
---
# 原理
两个盒子，盒子1放小图片，盒子2放对应的大图片。盒子1里的图片正常显示，盒子2里的图片隐藏。在盒子1的图片标签中，加入大图的数据链接。当鼠标在盒子1上移动时，通过鼠标在盒子1的位置计算出盒子2中应该显示的大图的部分。

<!--more-->

以下代码中，利用到了jquery.jqzoom.js插件。

# 代码

```
<div id="preview" class="spec-preview"> 
    <span class="jqzoom">
        <img jqimg="images/b1.jpg" src="images/s1.jpg" />
    </span> 
</div>
```

```
$(function(){
    $(".jqzoom").jqueryzoom({xzoom:380,yzoom:380});
});
```


# 参考文档
jquery.jqzoom.js图片放大镜
http://www.cnblogs.com/sydeveloper/p/3796330.html

ImageZoom 图片放大效果
http://www.cnblogs.com/cloudgamer/archive/2010/04/01/ImageZoom.html

[jQuery-实现图片的放大镜显示效果](http://www.zhangxinxu.com/wordpress/2009/08/jquery-%E5%AE%9E%E7%8E%B0%E5%9B%BE%E7%89%87%E7%9A%84%E6%94%BE%E5%A4%A7%E9%95%9C%E6%98%BE%E7%A4%BA%E6%95%88%E6%9E%9C/)

jquery插件 放大镜
http://www.jq-school.com/Article.aspx?kid=41

jQzoom简介
http://www.oschina.net/p/jqzoom?fromerr=tUayHgqO
