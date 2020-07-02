---
layout: post
title: "Socket.io"
date: 2012-08-23 01:56
comments: true
tags: 
- NodeJS
---

[Socket.io](http://socket.io/)的目的是提供即時通訊的套件，在官方網站上可以看到一些簡單的範例程式碼，但是我還是記錄一下簡單的操作。首先必須安裝Socket.io。

{% codeblock %}
npm install socket.io
{% endcodeblock %}

Server端程式碼
{% codeblock server.js lang:javascript %}
var http = require('http'),
    fs = require('fs'),
    server,
    io;

server = http.createServer(function(req, res){
  fs.readFile(__dirname + '/index.html', function(err, data){
    res.writeHead(200);
    res.end(data);
  });
});

server.listen(8080);

//引入socket.io並且聽取server的服務
io = require('socket.io').listen(server);

//當連線時(connection)執行的動作
io.sockets.on('connection', function(socket){

  //讓Client端監聽'news'方法，並傳送值({hello:'world'})
  socket.emit('news', {hello:'world'});

  //監聽Client發送的'my other event'，並將Client端發送的值印出
  socket.on('my other event', function(data){
    console.log(data);
  });
});
{% endcodeblock %}

Server端與Client端對照<code>.on</code>與<code>.emit</code>兩個方法內的名稱，可看出Server端與Client的互相對應關係。

Client端
{% codeblock index.html lang:html %}
<script src='/socket.io/socket.io.js'></script>
<script>
 var socket = io.connect('http://localhost:8080');
 socket.on('news', function(data){
  console.log(data);
  socket.emit('my other event', {my: 'data'});
 });
</script>
{% endcodeblock %}