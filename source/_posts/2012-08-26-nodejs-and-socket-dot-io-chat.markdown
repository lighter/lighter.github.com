---
layout: post
title: "NodeJS and Socket.io chat"
date: 2012-08-26 08:48
comments: true
tags: 
- NodeJS
---

有了簡單的聊天室的概念([Simple Chat With Socket.io](http://lighter.github.com/blog/2012/08/24/simple-chat-with-socket-dot-io/))之後，我就在想，那同時有很多人時，要如何知道現在線上有誰呢？還有每次發言的人是誰呢？有了這些想法，那就開始繼續修改程式碼吧。

## Client端

我將程式碼拆解幾個部分來看，先從Client端來看，一開始是引入<code>bootstrap</code>、<code>jQuery</code>等基本的<code>html</code>宣告
{% codeblock index.html lang:html %}
<!DOCTYPE html>
<html>
<head>
<link rel="stylesheet" href="http://current.bootstrapcdn.com/bootstrap-v204/css/bootstrap-combined.min.css" type="text/css" />
<script src='/socket.io/socket.io.js'></script>
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.8.0/jquery.min.js"></script>
{% endcodeblock %}

再來就是<code>javascript</code>的部份，可看到<code>socket.on</code>監聽的是<code>connect</code>事件，當開啟網頁連現實，會呼應到Server端的<code>add_user</code>事件，並且執行<code>prompt()</code>。這邊暫且先不看到Server端，繼續往下看下去。

{% codeblock index.html lang:javascript %}
<script>
var socket = io.connect('http://localhost:8080');

//Part 1
//一連線時執行
socket.on('connect', function(){
  socket.emit('add_user', prompt("What's your name"));
});
{% endcodeblock %}

當有位新的使用者加入時，目前線上所有使用者，勢必要更新，而使用者列表更新，需透過Server端告知Clint端要執行更新的動作因此在Client的程式碼如下：

下面程式碼可看到<code>update_chat_content</code>如同之前的方式加入對話內容([Here](http://lighter.github.com/blog/2012/08/24/simple-chat-with-socket-dot-io/))；接著<code>update_user</code>一開始先清空使用者列表所有內容，使用<code>$.each</code>一一將使用者重新加入列表中。

{% codeblock index.html lang:javascript %}
//Part 2
//update_chat_content
socket.on('update_chat_content', function(user_name, data){

  //由Server端emit 'update_chat_content' 更新談話內容
  $('#chat_content').append('<div>' + user_name + ':' + data + '</div>');
});

//update_user
socket.on('update_user', function(data){

  //先清空所有使用名稱
  $("#users").empty();

  //將data取出，使用each的方法一個一個將使用者重新加入列表
  $.each(data, function(key, value){
    $("#users").append('<div>' + value + '</div>');
  });

});
{% endcodeblock %}

最後則是發送對話按鈕的觸發事件以及HTML的部份。

{% codeblock index.html lang:javascript %}
//Part 3
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

<div class="container-fluid">
  <div class="row-fluid">
    <div class="span2" id="users">
      <!--Sidebar content-->
    </div>
    <div class="span10">
      <!--Body content-->
      <div class="span12" id="chat_content"></div>
      <div class="input-append">
      <input class="span2" id="send_input" size="16" type="text"><button class="btn" type="button" id="send">Send!</button>
      </div>
    </div>
  </div>
</div>

</body>
</html>
{% endcodeblock %}

## Server端
Server端主要的情況有3個：

1.當Client發送對話訓時

2.當有新加入的使用者時

3.當使用者關閉網頁時

程式碼如下，當有新加入的使用者會呼叫Server端的<code>add_user</code>事件，Server端會將新加入的使用者加入到socket的session內，並呼叫Client端執行<code>update_chat_content</code>事件，並傳遞兩個參數，這是要用來讓Client端知道有新加入的使用者，例如當Tony加入時，在Client端頁面會顯示<code>SERVER:Tomy has connected</code>的訊息告知。其餘的就說明都寫在程式碼的註解上了，直接看程式碼可能會更清楚。

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

//使用者名稱
var user_name = {};

io = require('socket.io').listen(server);

//連線時會執行的內容
io.sockets.on('connection', function(socket){

  //當client端emit 'send_chat' 時執行
  socket.on('send_chat', function(data){
    io.sockets.emit('update_chat_content', socket.user_name, data);
  });

  //當client端emit 'add_user' 時執行
  socket.on('add_user', function(user_name){

    //儲存user_name到socket session
    socket.user_name = user_name;

    //加入user_name 到 user_name list
    user_name[user_name] = user_name;

    //呼叫Client端的 update_chat事件
    socket.emit('update_chat_content', 'SERVER:', socket.user_name + ' has connected');

    //呼叫所有Client端的 update_chat事件
    socket.broadcast.emit('update_chat_content', 'SERVER:',user_name + ' has connected');

    //呼叫Client端的 update_user事件
    io.sockets.emit('update_user', user_name);
  });

  //當Client端斷線時執行
  socket.on('disconnect', function(){

    //從user_name list中刪除使用者
    delete user_name[socket.user_name];

    //呼叫Client端的 update_user事件更新使用者列表
    io.sockets.emit('update_user', user_name);

    //廣播所有Client端的 update_chat_content事件，通知某某使用者離線
    socket.broadcast.emit('update_chat_content', 'SERVER:', socket.user_name + ' has disconnected');
  });
});
{% endcodeblock %}

## 參考資料

[node.js and socket.io chat tutorial](http://psitsmike.com/2011/09/node-js-and-socket-io-chat-tutorial/)
