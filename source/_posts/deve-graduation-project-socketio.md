---
title: 使用Socket.IO实现实时私信功能
toc: true
date: 2016-06-20 20:29:08
tags: 
- 毕设
- Node
- socketio
categories: 设计开发
---
# 功能描述
在帖子详情页面，单击用户头像，可以发送私信给该用户。在消息页面，输入用户名，可以发送私信给该用户。
如果该用户在线，则把私信实时发送给该用户，如果该用户不在线，则把私信存储到数据库，成为消息记录。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/socketio/send.png)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/socketio/history.png)

<!--more-->

# 原理
WebSocket protocol 是HTML5一种新的协议。使用WebSocket API，浏览器和服务器只需要做一个握手的动作，就形成了一条快速通道。借助这个通道，浏览器和服务器之间就可以互相传送数据。

Socket.IO是一个完全由JavaScript实现、基于Node.js、支持WebSocket的协议用于实时通信、跨平台的开源框架，它包括了浏览器端的JavaScript和服务器端的Node.js。 Socket.IO除了支持WebSocket通讯协议外，还支持许多种轮询（Polling）机制以及其它实时通信方式，并封装成了通用的接口，并且在服务端实现了这些实时机制的相应代码。

页面初始化时，浏览器和服务端建立一个连接，用来发送事件和接收事件。浏览器和服务端建立连接后，服务端会触发一个“adduser”事件，把当前用户添加到在线用户列表中。当用户离开页面时，会触发一个“deleteuser”事件，把当前用户从在线用户列表中删除。
用户A发私信给用户B时，服务端接收到“privatemsg”事件，会把该信息存储到数据库。同时，如果用户B在线，则触发用户B的浏览器的“sendback”事件，把信息发送给用户B的浏览器。用户B的浏览器接收到“sendback”事件，说明收到了新的消息，该消息实时插入到聊天对话框。

# 代码
## js
```
// 即时通讯
// 建立连接
var socket = io.connect();
var from_user = $('#from-user').val();
socket.emit("adduser",from_user,function(data){
    console.log(data);
});

// 发送消息
var from_user = $('#from-user').val();
var to_user = friend_id;
var content = $('#chat .reply-content').val();
var data = {
    from_user: from_user,
    to_user: to_user,
    content: content
};
socket.emit("privatemsg",data,function(data){
    console.log(data);
});

// 接受消息
socket.on("sendback",function(data){
    console.log(data);
});

// 断开连接
window.onunload = function(){
    console.log('删除用户');
    var from_user = $('#from-user').val();
    socket.emit('deleteuser',from_user,function(data){
        console.log(data);
    });
}

```


## Node端
```
var User = require('./models/user-model');
var Message = require('./models/message-model');
var FriendCollect = require('./models/friend-collect-model');

//var mongoose = require('mongoose');
//mongoose.connect('mongodb://localhost/forum');

var users = {};
var socket_event = function(socket) {

    socket.on('disconnect', function () {
        //console.log('user disconnected');
    });

    socket.on("adduser", function (from_user, callback) {
        if (from_user in users) {
            users[from_user] = socket;
            callback(true);
        } else {
            socket.user_id = from_user;
            users[socket.user_id] = socket;
            callback(true);
        }
    });

    socket.on('deleteuser', function (from_user, callback) {
        if (from_user in users) {
            delete users[from_user];
            callback(true);
        }
    });

    socket.on("privatemsg", function (data, callback) {
        var param = {
            from_user: data.from_user,
            to_user: data.to_user,
            content: data.content
        };
        var message = new Message(param);
        message.save();
        if (data.to_user in users) {
            User.findById(data.from_user,function(err,user){
                users[data.to_user].emit('sendback', {'content': data.content,'friend':user});
                callback(true);
            });
        } else {
            callback('对方未登录，已留言');
        }

    });
}

module.exports = socket_event;
```

# 源码
https://github.com/voidking/nodeforum/blob/master/views/message/message.html
https://github.com/voidking/nodeforum/blob/master/public/js/message/message.js
https://github.com/voidking/nodeforum/blob/master/app.js
https://github.com/voidking/nodeforum/blob/master/socket-event.js

# 参考文档
Socket.IO官网
http://socket.io/

Socke.IO项目地址
https://github.com/socketio/socket.io

在线聊天Demo
http://chat.socket.io/
http://socket.io/get-started/chat/

Socket.IO API
http://socket.io/docs/

Nodejs Socket.io 发送私信
http://www.jenkihuang.com/job/2015/10/nodejs-socket-io-privatemsg.html

Socket.IO 和 Node.js 入门
http://www.oschina.net/question/12_54009/?fromerr=pCDquuQy

Socket.io:有点意思
http://www.cnblogs.com/edwardstudy/p/4358202.html