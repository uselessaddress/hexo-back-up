---
title: 使用Node读写文件
toc: true
date: 2016-07-30 15:48:00
tags:
- node
categories: 设计开发
---
# 前言
七夕将近，公司要整一个活动：七夕礼物测试。大致需求是，用户选择自己的性别、生日和情感状态，点击“惊喜一下”，出现礼物详情。根据性别、生日和情感状态的不同，给出不同的礼物详情，一共有37种不同的礼物。同时，添加统计，统计有多少人参加过测试。

<!--more-->

# 设计
这次活动，不打算让后端参与，所以要Node端自己负责数据的存取。

读取文件有三个方案：
1、数据存在代码中（运行时直接加载到内存）。
2、利用request模块读取。
3、利用fs模块读取。

写入文件有一个方案：利用fs模块写入。

在之前的文章中，已经讨论过使用request模块读取本地文件的方法，本文主要探讨一下使用fs模块读写文件的方法。

# 代码
## 数据文件
在/public/data文件下，新建文件count.json。内容如下：
```
{"count":100}
```

## fs读取文件
```
var fs = require('fs');
var path = require('path');

fs.readFile(path.join(__dirname,'../public/data/count.json'),{encoding:'utf-8'},function(error, data){
    console.log(data);
    var countData = JSON.parse(data);
    console.log(count.countData.count);
});

```
需要解释一下的，是__dirname，在任何模块文件内部，可以使用__dirname变量获取当前模块文件所在目录的完整绝对路径。

path.join([path1],[path2],[...])函数，将多个参数组合成一个path。

## fs写入文件
```
var fs = require('fs');
var path = require('path');

fs.readFile(path.join(__dirname,'../public/data/count.json'),{encoding:'utf-8'},function(error, data){
    //console.log(data);
    var count = parseInt(JSON.parse(data).count);
    count++;
    var countData = {
        count: count
    };
    fs.writeFile(path.join(__dirname,'../public/data/count.json'),JSON.stringify(countData),function(error){
        console.log('success');
    });
});
```

## 完整代码
https://github.com/voidking/nodebase/blob/master/controllers/weixin.js

# 书签
简单的nodejs 文件系统（fs）读写例子
http://www.2cto.com/kf/201411/351586.html

Node.js读取文件内容
http://blog.csdn.net/zk437092645/article/details/9231787

node 的文件操作 
http://blog.chinaunix.net/uid-26672038-id-4139323.html