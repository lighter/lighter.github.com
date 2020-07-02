---
layout: post
title: "Simple chat with Socket.io"
date: 2012-08-24 13:32
comments: true
tags: 
- NodeJS
---

如果已我所學不多的情況下要製作一個簡單的聊天室，我大概能想到的方法就是將這些對話內容存入MySql，然後背後透過Ajax一直去撈取資料庫的內容然後一直更新畫面上的對話內容，這樣想想就覺得好麻煩…

最近看到Socket.io這簡直真是好用，下面是一個簡單的聊天室程式碼。

## Server端
{% codeblock Server.js lang:javascript %}
var http = require('http'),
    fs = require('fs'),
    server,
    io;

server = http.createServer(function(req, res){
  fs.readFile(__dirname + '/index.html', function(err, data){
    res.writeHead(200, {'Content-Type': 'text/html'});
    res.end(data);
  });
});

server.listen(8080);
io = require('socket.io').listen(server);

//連線時會執行的內容
io.sockets.on('connection', function(socket){

  //當client端emit 'send_chat'時執行
  socket.on('send_chat', function(data){
    io.sockets.emit('update_chat_content', data);
  });

});
{% endcodeblock %}

## Client端
{% codeblock index.html lang:html %}
<!DOCTYPE html>
<html>
<head>
<link rel="stylesheet" href="http://current.bootstrapcdn.com/bootstrap-v204/css/bootstrap-combined.min.css" type="text/css" />
<script src='/socket.io/socket.io.js'></script>
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.8.0/jquery.min.js"></script>
<script>
 var socket = io.connect('http://localhost:8080');
 socket.on('update_chat_content', function(data){

  //由Server端emit 'update_chat_content' 更新談話內容
  $('#chat_content').append('<div>' + data + '</div>');
 });
 
 $(function(){
  $("#send").on('click', function(){

    //取得input text欄位的值後，將欄位內容清除
    var message = $("#send_input").val();
    $("#send_input").val('');

    //呼叫Server端的emit 'send_chat'
    socket.emit('send_chat', message);
  });
 });
</script>
</head>
<body>

<div class="row-fluid" style="text-align: center">
  <div class="span12" id="chat_content"></div>
  <div class="input-append">
    <input class="span2" id="send_input" size="16" type="text"><button class="btn" type="button" id="send">Send!</button>
  </div>
</div>

</body>
</html>
{% endcodeblock %}

其實只要觀察一下<code>socket.emit</code>與<code>socket.on</code>裡面監聽的名稱就可以看出Server端與Client的執行時間順序了；最後執行<code>node Server.js</code>就可以看到一個簡單的聊天室。