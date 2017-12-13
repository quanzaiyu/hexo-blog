---
title: Node.js应用篇 - 使用Socket.io搭建一个聊天室
categories:
  - Node.js
  - Node.js应用篇
tags:
  - 命令行
---



# sock.io简介

​    socket.io一个是基于Nodejs架构体系的，支持websocket的协议用于时时通信的一个软件包。socket.io 给跨浏览器构建实时应用提供了完整的封装，socket.io完全由javascript实现，包括服务器端和客户端。[示例](https://github.com/quanzaiyu/socket.io-chatroom) 



# 环境搭建

```bash
mkdir sock.io-test && cd sock.io-test
npm init
npm i express@4.15.2 socket.io -S
```

## 编写简单测试Demo

这里创建两个文件，`index.js`为服务器入口文件，`index.html`为客户端入口文件。

**index.js**

```js
var app = require('express')();
var http = require('http').Server(app);
var io = require('socket.io')(http);

app.get('/', function(req, res){
  res.sendFile(__dirname + '/index.html');
});

// 监听用户连接
io.on('connection', function(socket){
  console.log('a user connected');
  // 监听用户消息
  socket.on('chat message', function(msg){
    console.log('message: ' + msg);
  });
  // 监听用户下线
  socket.on('disconnect', function(){
    console.log('user disconnected');
  });
});

http.listen(3000, function(){
  console.log('listening on *:3000');
});
```

**index.html**

```html
<!doctype html>
<html>

<head>
  <title>Socket.IO chat</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { font: 13px Helvetica, Arial; }
    form { background: #000; padding: 3px; position: fixed; bottom: 0; width: 100%; }
    form input { border: 0; padding: 10px; width: 90%; margin-right: .5%; }
    form button { width: 9%; background: rgb(130, 224, 255); border: none; padding: 10px; }
    #messages { list-style-type: none; margin: 0; padding: 0; }
    #messages li { padding: 5px 10px; }
    #messages li:nth-child(odd) { background: #eee; }
  </style>
</head>

<body>
  <ul id="messages"></ul>
  <form action="">
    <input id="m" autocomplete="off" />
    <button>Send</button>
  </form>
  <script src="/socket.io/socket.io.js"></script>
  <script src="https://code.jquery.com/jquery-1.11.1.js"></script>
  <script>
    $(function () {
      var socket = io('http://localhost:3000');
      $('form').submit(function(){
        socket.emit('chat message', $('#m').val());
        $('#m').val('');
        return false;
      });
    });
  </script>
</body>

</html>
```

运行`node index`可以看到终端打印出各种提示信息，用户上线、发送消息、下线监听。[完整demo下载](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/res/chat-websocket.zip) 



# socket.io应用

## 广播

**index.js**中添加 

```js
socket.on('chat message', function(msg){
  io.emit('chat message', msg);
});
```

这里，每次接收到客户端发送的消息，再将消息传递到客户端。

**index.html**中添加 

```js
$(function () {
  var socket = io();
  $('form').submit(function(){
    socket.emit('chat message', $('#m').val());
    $('#m').val('');
    return false;
  });
  socket.on('chat message', function(msg){
    $('#messages').append($('<li>').text(msg));
  });
});
```

首先，提交表单按钮按下后，将消息发送到服务器端，客户端同时进行监听，由于服务器又将消息传递出来，客户端显示此消息。



# 一些示例

```bash
https://github.com/bsspirit/chat-websocket
https://github.com/socketio/socket.io
```






# 参考资料

[粉丝日志: Socket.io在线聊天室](http://blog.fens.me/nodejs-socketio-chat/) 

[sock.io官网开发文档](https://socket.io/get-started/chat/) 

[WeChat: 利用 socket.io 实现消息实时推送](https://mp.weixin.qq.com/s/554sGduixi-P3OyFW3gsoA) [相关github项目](https://github.com/noiron/socket-message-push) 

