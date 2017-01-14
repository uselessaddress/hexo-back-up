---
title: 使用百度地图实现发帖时的定位
toc: true
date: 2016-06-20 19:31:41
tags:
- 毕设
- Node
- 百度地图
categories: 设计开发
---
# 功能描述
在发布帖子界面，用户可以选择是否在发布帖子时显示位置。位置图标默认灰色，不显示位置；用户选择位置后，位置图标变成绿色，同时把位置信息显示在发布帖子界面上。

![选择是否显示位置](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/baidumap/choose.png)
![显示位置](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/baidumap/position.png)

<!--more-->

# 原理
百度地图，提供了四种定位的方式，分别是根据浏览器定位、根据IP定位、根据城市名定位、根据经纬度定位。本功能模块中，采用根据浏览器定位的方式。

浏览器通过js请求百度地图API，获取到地址信息后，发送给我的服务器。

# 代码
## html
引入百度地图api，定义一个id，用来初始化BMap。
```
<script type="text/javascript" src="http://api.map.baidu.com/api?v=2.0&ak=BC46e2b24290dba4d0267e2430f512fe"></script>

<div id="allmap" style="display: none;"></div>
```

## js
新建BMap对象，给一个初始化坐标，然后浏览器通过百度地图API获取到位置信息，获取到位置信息后，和帖子的其他信息一起，发送给服务端。

```
// 百度地图定位
var baidu_position = {};
var map = new BMap.Map("allmap");
var point = new BMap.Point(118.895144,31.92596);
map.centerAndZoom(point,12);

var geolocation = new BMap.Geolocation();
geolocation.getCurrentPosition(function(r){
    if(this.getStatus() == BMAP_STATUS_SUCCESS){
        var mk = new BMap.Marker(r.point);
        map.addOverlay(mk);
        map.panTo(r.point);
        //alert('您的位置：'+r.point.lng+','+r.point.lat);
        console.log(r);
        baidu_position = r;
    }
    else {
        alert('failed'+this.getStatus());
    }
},{enableHighAccuracy: true})

```

# 源码
https://github.com/voidking/nodeforum/blob/master/public/js/post/post-add.js

# 书签
百度地图API
http://developer.baidu.com/map/reference/

百度地图API示例
http://developer.baidu.com/map/jsdemo.htm#d0_2

百度LSB API开发指南
http://developer.baidu.com/map/wiki/index.php?title=uri/api/web

拾取坐标系统
http://api.map.baidu.com/lbsapi/getpoint/index.html

根据标注点坐标范围计算显示缩放级别zoom自适应显示地图
http://www.aichengxu.com/view/2456553

百度地图API详解和运用
http://blog.csdn.net/binyao02123202/article/details/7955803