---
title: 微信小程序基础
toc: true
date: 2016-12-04 13:26:17
tags:
- 微信
- 小程序
categories: 设计开发
---
# 前言
> 张小龙在朋友圈里这样解释到：小程序是一种不需要下载安装即可使用的应用，它实现了应用“触手可及”的梦想，用户扫一扫或者搜一下即可打开应用，也体现了“用完即走”的理念，用户不用关心是否安装太多应用的问题。应用将无处不在，随时可用，但又无需安装卸载。

<!--more-->

老师帮助申请了一个小程序，接下来，小编决定利用小程序做一个应用。有以下五个想法：
1、公交查询系统
2、餐厅排号系统
3、餐厅点餐系统
4、书籍借阅交易系统
5、小范围问答系统

无论做哪个，都需要对腾讯提供的小程序API进行一个较全面的了解，本文就记录一下学习过程中的重点。

# 项目结构
使用微信开发者工具，新建小程序项目，生成一些初始文件，结构如下。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/weapp-base/structure.jpg)

| 文件 | 必填 | 作用 |
| ----- | ----- | ----- |
| app.js | 是 | 小程序逻辑 |
| app.json | 是 | 小程序公共设置 |
| app.wxss | 否 | 小程序公共样式表 |

| 文件类型 | 必填 | 作用 |
| js | 是 | 页面逻辑 |
| wxml | 是 | 页面结构 |
| wxss | 否 | 页面样式表 |
| json | 否 | 页面配置 |

app.json文件来对微信小程序进行全局配置，决定页面文件的路径、窗口表现、设置网络超时时间、设置多 tab 等。

以下是一个包含了所有配置选项的简单配置app.json ：
```
{
  "pages": [
    "pages/index/index",
    "pages/logs/index"
  ],
  "window": {
    "navigationBarTitleText": "Demo"
  },
  "tabBar": {
    "list": [{
      "pagePath": "pages/index/index",
      "text": "首页"
    }, {
      "pagePath": "pages/logs/logs",
      "text": "日志"
    }]
  },
  "networkTimeout": {
    "request": 10000,
    "downloadFile": 10000
  },
  "debug": true
}
```

# 加载过程
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/weapp-base/order.jpg)
由上图可以看出，加载过程为：
1、app.js中的onLaunch和onShow函数被调用。
2、按照app.json中的配置，注册pages参数中的页面。
3、按照app.json中的配置，跳转到pages参数中的第一个页面（index页面）。
4、index.js中的onLoad和onShow函数被调用。
5、使用index.js的data初始化页面。
6、index.js中onReady函数被调用。

# 逻辑层
## 注册程序
App() 函数用来注册一个小程序。接受一个 object 参数，其指定小程序的生命周期函数等。

object参数说明：

| 属性 | 类型 | 描述 | 触发时机 |
| ----- | ----- | ----- | ----- |
| onLaunch | Function | 生命周期函数--监听小程序初始化 | 当小程序初始化完成时，会触发 onLaunch（全局只触发一次） |
| onShow | Function | 生命周期函数--监听小程序显示 | 当小程序启动，或从后台进入前台显示，会触发onShow |
| onHide | Function | 生命周期函数--监听小程序隐藏 | 当小程序从前台进入后台，会触发 onHide |
| 其他 | Any | 开发者可以添加任意的函数或数据到 Object 参数中，用 this 可以访问 |

## 注册页面
Page() 函数用来注册一个页面。接受一个 object 参数，其指定页面的初始数据、生命周期函数、事件处理函数等。

object 参数说明：

| 属性 | 类型 | 描述 |
| ----- | ----- | ----- |
| data | Object | 页面的初始数据 |
| onLoad | Function | 生命周期函数--监听页面加载 |
| onReady | Function | 生命周期函数--监听页面初次渲染完成 |
| onShow | Function | 生命周期函数--监听页面显示 |
| onHide | Function | 生命周期函数--监听页面隐藏 |
| onUnload | Function | 生命周期函数--监听页面卸载 |
| onPullDownRefresh | Function | 页面相关事件处理函数--监听用户下拉动作 |
| onReachBottom | Function | 页面上拉触底事件的处理函数 |
| 其他 | Any | 开发者可以添加任意的函数或数据到 object 参数中，在页面的函数中用 this 可以访问 |

详见[小程序文档](https://mp.weixin.qq.com/debug/wxadoc/dev/framework/app-service/page.html)


## 模块化
我们可以将一些公共的代码抽离成为一个单独的 js 文件，作为一个模块。模块只有通过 module.exports 或者 exports 才能对外暴露接口。

需要注意的是：
- exports 是 module.exports 的一个引用，因此在模块里边随意更改 exports 的指向会造成未知的错误。所以我们更推荐开发者采用 module.exports 来暴露模块接口，除非你已经清晰知道这两者的关系。
- 小程序目前不支持直接引入 node_modules , 开发者需要使用到 node_modules 时候建议拷贝出相关的代码到小程序的目录中。

```
// common.js
var common = {
    sayHello: function(){
        console.log('hello');
    },
    sayGoodbye: function(){
        console.log('goodbye');
    }
};

module.exports = common;
```

在需要使用这些模块的文件中，使用 require(path) 将公共代码引入。
```
var common = require('../../modules/common/common.js');
Page({
  say: function() {
    common.sayHello();
    common.sayGoodbye();
  }
})
```

## API
小程序开发框架提供丰富的微信原生 API，可以方便的调起微信提供的能力，如获取用户信息，本地存储，支付功能等。
详细介绍请参考[API 文档](https://mp.weixin.qq.com/debug/wxadoc/dev/api/)

# 视图层
## WXML
```
<!-- item.wxml -->
<template name="item">
  <text>{{text}}</text>
</template>
```

```
<!--home.wxml-->
<view class="container">
  <view  bindtap="bindViewTap" class="userinfo">
    <image class="userinfo-avatar" src="{{userInfo.avatarUrl}}" background-size="cover"></image>
    <text class="userinfo-nickname">{{userInfo.nickName}}</text>
  </view>
  <button bindtap="toIndex">跳转index</button>

  <view wx:if="{{id==1}}">第一条</view>
  <view wx:elif="{{id==2}}">第二条</view>
  <view wx:else>其他</view>
  <!--hidden只用于text-->
  <div hidden="{{true}}">
    这是个div
  </div>
  <text hidden="{{true}}">这是个text</text>

  <view wx:for="{{array}}">
    {{index}}:{{item.message}}
  </view>

  <!--定义模板-->
  <template name="myTemplate">
    <view>
      <text> {{index}}: {{msg}} </text>
      <text> Time: {{time}} </text>
    </view>
  </template>
  <!--使用模板-->
  <template is="myTemplate" data="{{...myData}}"/>

  <template name="odd">
    <view> odd </view>
  </template>
  <template name="even">
    <view> even </view>
  </template>

  <block wx:for="{{[1, 2, 3, 4, 5]}}">
    <template is="{{item % 2 == 0 ? 'even' : 'odd'}}"/>
    <template is="{{index % 2 == 0 ? 'even' : 'odd'}}"/>
  </block>
  <block wx:for="{{[1, 2, 3, 4, 5]}}" wx:for-index="itemIndex" wx:for-item="itemData">
    索引{{itemIndex}}，数值{{itemData}}
  </block>

  <view id="tapTest" data-hi="WeChat" bindtap="tapName"> Click me! </view>
  <import src="item.wxml"/>
  <template is="item" data="{{text: 'forbar'}}"/>
</view>
```

```
/*home.js*/
// 获取应用实例
var app = getApp();
var common = require('../../modules/common/common.js');

Page({
  data:{
    id: 4,
    userInfo: {},
    array: [{
      message: 'foo',
    }, {
      message: 'bar'
    }],
    myData: {
      index: 1000,
      msg: '模板',
      time: '16:08'
    }
  },
  onLoad:function(options){
    // 页面初始化 options为页面跳转所带来的参数
    var that = this;
    //调用应用实例的方法获取全局数据
    app.getUserInfo(function(userInfo){
      //更新数据
      that.setData({
        userInfo:userInfo
      })
    });
    common.sayHello();
  },
  onReady:function(){
    // 页面渲染完成

  },
  onShow:function(){
    // 页面显示
    console.log('这是home页的onShow方法');
  },
  onHide:function(){
    // 页面隐藏

  },
  onUnload:function(){
    // 页面关闭

  },
  toIndex: function(){
    wx.navigateTo({
      url: '../index/index'
    });
  },
  tapName: function(event) {
    console.log(event)
  }
})
```

![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/weapp-base/result.jpg)
## WXSS
当成css使用就好。

## 组件
[组件文档](https://mp.weixin.qq.com/debug/wxadoc/dev/component/)

# 后记
小程序利弊各半，是否使用还是要看需求。

利：
- 不用安装，即开即用，用完就走。
- 省流量，省安装时间。
- 相较于原生应用，开发成本更低。
- 推广更容易更简单，更省成本。

弊：
- 寄生于微信，网页没有可移植性。
- 无法取代休闲娱乐办公类原生应用。

最后，引用iH5互动大师创始人孟智平的一段话：
> 真正的小程序的形式只能是H5。最轻（不用下载安装，用了就走）、最灵活（开发难度小，投放周期短，调整更容易）、最通用（标准的Web形态，有浏览器就能打开）。

# 书签
简易教程-小程序
https://mp.weixin.qq.com/debug/wxadoc/dev/index.html

微信小程序全方位深度解析-课程学习-百度传课
http://www.chuanke.com/v4702151-193232-1107660.html

为什么我反对微信小程序？
https://www.huxiu.com/article/171880/1.html?f=myzakercom

我们真的需要网页版App吗？Google PWA的困局
http://www.leiphone.com/news/201606/UEiart497WUzS62u.html
