---
title: ReactNative、Ionic和NativeScript
toc: true
date: 2016-06-23 15:35:07
tags:
- 前端
categories: 设计开发
---
# 前言
最近几年，利用HTML5技术开发跨平台app，越来越流行。各种框架层出不穷，小编选择了其中三个，来对比研究一下究竟什么样的框架才更好。

<!--more-->

# ReactNative
React Native 使你能够使用基于 JavaScript 和 React 一致的开发体验在本地平台上构建世界一流的应用程序体验。React Native 把重点放在所有开发人员关心的平台的开发效率上——开发者只需学习一种语言就能轻易为任何平台高效地编写代码。Facebook 在多个应用程序产品中使用了 React Native，并将继续为 React Native 投资。

优势：
1、虽然不能做到一处编码到处运行，但是基本上即使是两套代码，也是相同的jsx语法，使用js进行开发。用户体验，高于html，开发效率较高。
2、flexbox 布局，据说比native的自适应布局更加简单高效。
3、可实现在线更新，AppStore审核政策调整：允许运行于JavascriptCore的动态加载代码。
4、更贴近原生开发。

劣势：
1、（引）对开发人员要求较高，不是懂点web技术就行的，当官方封装的控件、api无法满足需求时 就必然需要懂一些native的东西去扩展，扩展性仍然远远不如web，也远远不如直接写Native code。
2、（引）官方说得很隐晦：learn once, write anywhere。人家可没说run anywhere。事实上，从官方的api来看SliderIOS，SwitchIOS..等等这些控件，之后势必会出现SliderAndroid，SwitchAndroid...，也就是很可能针对不同的平台会需要写多套代码。
3、发展还不成熟，目前很多ui组件只有ios的实现，android的需要自己实现。
4、（引）从Native到Web，要做很多概念转换，势必造成双方都要妥协。比如web要用一套CSS的阉割版，Native通过css-layout拿到最终样式再转换成native原生的表达方式（比如iOS的Constraint\origin\Center等属性），再比如动画。另外，若Android和iOS都要做相同的封装，概念转换就更复杂。
5、文档还不够完整，学习起来困难。


# Ionic

IONIC是目前最有潜力的一款 HTML5 手机应用开发框架。通过 SASS 构建应用程序，它提供了很多 UI 组件来帮助开发者开发强大的应用。 它使用 JavaScript MVVM 框架和 AngularJS 来增强应用。提供数据的双向绑定，使用它成为 Web 和移动开发者的共同选择。Ionic是一个专注于用WEB开发技术，基于HTML5创建类似于手机平台原生应用的一个开发框架。Ionic框架的目的是从web的角度开发手机应用，基于PhoneGap的编译平台，可以实现编译成各个平台的应用程序。

优势：
1、ios 和 android 基本上可以共用代码，纯web思维，开发速度快，简单方便，一次编码，到处运行，如果熟悉web开发，则开发难度较低。
2、文档很全，系统级支持封装较好，所有UI组件都是有html模拟，可以统一使用。
3、可实现在线更新 允许加载动态加载web js。
4、文档多，开发者多，视频教程多。容易学习，遇到问题容易解决，技术成熟。

劣势：
1、占用内存高一些（不过手机内存都大了不影响），不适合做游戏类型app，web技术无法解决一切问题，对于比较耗性能的地方无法利用native的思维实现优势互补，如高体验的交互，动画等。

# NativeScript
和 ReactNative 相比，NativeScript 最大的特点是可以获得100%的原生 API 。也就是说，开发者可以通过 JavaScript 获取和原生开发语言同样多的原生接口。

优势：
1、使用 JavaScript 直接访问所有原生 API。
2、系统新功能0延时支持。
3、第三方原生库全部支持。

劣势：
1、NativeScript和React不同，无法与原生项目融合，即你只能纯写个NativeScript的应用，不可能把它抽离出来作为某原生应用的一部分来出现。虽然说它和React的出发点一致都是"用Web APP的开发速度打造Native App的体验"，但是实际上，它算是鸡肋吧，拿它来写个展示App或者简单的应用还是不错的。
2、NativeScript中虽然已经支持了很多组件，比如说tabview、srcollview、button，但是提供的组件方法、属性过少，整个框架还不是很丰满。


# 后记
利用HTML5技术开发跨平台app，最重要的是两点：第一点，一套代码，所有平台都可以运行；第二点，利用HTML5技术实现，尽量少的用到其他技术。至于性能，肯定是不如原生应用的，只能期待框架以后的优化。
ReactNative，很多时候都要写两套代码，不满足第一点；开发时要用到很多原生的android和ios技术，不满足第二点。
Ionic，两点皆满足，但是更适合开发web类app，不适合开发游戏。
NativeScript，两点皆满足。

综上，个人认为，Ionic和NativeScript更值得学习。如果非要二选一，我选择NativeScript。理由很简单，ReactNative的研发人数更少，参考文档、书籍和视频更少，要做，就做先行者。

# 书签
## ReactNative
整理了一份React-Native学习指南
http://www.tuicool.com/articles/zaInUbA

RativeScript的工作原理：用JavaScript调用原生API实现跨平台 – OurJS
http://top.css88.com/archives/656

React Native中文社区
http://bbs.reactnative.cn/

React 入门实例教程
http://www.ruanyifeng.com/blog/2015/03/react.html

WebViewJavascriptBridge详细使用
http://www.tuicool.com/articles/Q3AVnq2

WebViewJavascriptBridge 原理分析
http://www.2cto.com/kf/201503/384998.html

教你怎么屏蔽掉在移动端的宽带运营商的流量劫持，屏蔽无耻的广告
https://my.oschina.net/zxcholmes/blog/596192

ReactJS 傻瓜教程
https://zhuanlan.zhihu.com/p/19896745?columnSlug=FrontendMagazine

React | A JAVASCRIPT LIBRARY FOR BUILDING USER INTERFACES
https://facebook.github.io/react/index.html

## Ionic
Ionic: Advanced HTML5 Hybrid Mobile App Framework
http://ionicframework.com/

Ionic中文文档教程
http://www.ionic.wang/js_doc-index.html

ionic react-native native 优劣势对比
http://www.ionic.wang/article-index-id-69.html


## NativeScript
使用NativeScript和Angular2构建跨平台APP
https://zhuanlan.zhihu.com/p/21458458

Cross-Platform Native Development with Javascript
https://www.nativescript.org/

NativeScript源码
https://github.com/NativeScript

NativeScript中文手册 - GitBook
https://www.gitbook.com/book/flowforever/nativescript-cn-book/details
