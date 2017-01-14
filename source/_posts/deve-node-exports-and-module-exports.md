---
title: node中exports和module.exports的区别和联系
toc: true
date: 2016-07-19 16:43:25
tags:
- node
- exports
categories: 设计开发
---
# 前言
> exports只是module.exports的辅助方法。你的模块最终返回module.exports给调用者，而不是exports。exports所做的事情是收集属性，如果module.exports当前没有任何属性的话，exports会把这些属性赋予module.exports。如果module.exports已经存在一些属性的话，那么exports中所用的东西都会被忽略。

> 最初时，exports是module.exports的一个引用。
> (exports=global.exports)=(self.exports=module.exports)

<!--more-->

# exports正常导出

```
// caculate.js
exports.add = function (a, b) {
    return a+b;
}
```

```
// main.js
var caculate = require('./caculate');

exports.runAdd = function(req, res){
    var num = caculate.add(5,4);
    console.log(num);
}
```
在main模块中，可以正常调用caculate模块中的add函数。因为exports是module.exports的一个引用，所以`exports.add=function(){}`这个操作，把add函数加到了module.exports指向的对象（函数也是对象）中。而main模块中获取到的，就是caculate的module.exports指向的对象，自然也获取到了add函数。

# exports错误导出
```
// caculate.js
exports = function (a, b) {
    return a+b;
}
```

```
// main.js
var caculate = require('./caculate');

exports.runAdd = function(req, res){
    console.log(caculate);
    //var num = caculate(5, 4);
    //console.log(num);
}
```
如上的导出方法是错误的，我们可以看到打印出的caculate是一个空对象`{}`。因为exports原本是module.exports的一个引用，后来指向了一个函数。而module.exports的指向的对象，初始值就是空对象`{}`，自始至终都没有这个空对象添加属性或函数。所以，当main模块中获取到caculate的module.exports指向的对象时，依然是一个空对象`{}`。

# module.exports导出

```
// caculate.js
exports.add = function (a, b) {
    return a+b;
}

var caculate = {
    delete: function(a, b){
        return a-b;
    },
    multiple: function(a, b){
        return a*b;
    }
};

module.exports = caculate;
```

```
// main.js
var caculate = require('./caculate');

exports.runDelete = function(req, res){
    //var num = caculate.add(5, 4);
    var num = caculate.delete(5, 4);
    console.log(num);
}
```
如上导出方法，在caculate的module.exports指向的对象中，不会包含add函数。虽然`exports.add=function(){}`向module.exports指向的空对象`{}`中添加了一个add函数，但是紧接着，module.exports指向的对象改变了！变成了caculate对象。

# seajs中module.exports和return
seajs中的exports和module.exports的关系，和Node中相同。

在进行插件CMD模块化时，发现module.exports和return基本相同。下面写个小例子：
```
// plugin.js
define(function(require, exports, module){
    module.exports = function(jQuery){
        // require('another-plugin')(jQuery);
        // 依赖jQuery的插件代码
    }
})
```

```
// plugin.js
define(function(require, exports, module){
    return function(jQuery){
        // require('another-plugin')(jQuery);
        // 依赖jQuery的插件代码
    }
})
```

```
seajs.use(['jquery','plugin'],function($,plugin){
    plugin($);//初始化
});
```

# 书签
Modules Node.js v6.3.0 Manual & Documentation
https://nodejs.org/api/modules.html#modules_the_module_object

module.exports 还是 exports？
http://zihua.li/2012/03/use-module-exports-or-exports-in-node/

[nodejs中export与module.export的区别](http://www.blogjava.net/Hafeyang/archive/2012/10/27/diifference_between_exports_and_module_dot_exports_in_nodejs.html)

SeaJS 中的 exports 和模块加载
http://www.tuicool.com/articles/Y3qmAj
