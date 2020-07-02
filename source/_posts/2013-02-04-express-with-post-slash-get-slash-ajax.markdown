---
layout: post
title: "express with post/get/ajax"
date: 2013-02-04 16:55
comments: true
tags: 
- NodeJS
---

使用NodeJS版本為<code>v0.8.18</code>

在終端機中輸入
{% codeblock %}
express test -e
{% endcodeblock %}

開啟<code>package.json</code>檔案，將版本都修改為<code>*</code>號。
{% codeblock lang:json %}
{
  "name": "application-name",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "start": "node app"
  },
  "dependencies": {
    "express": "*",
    "ejs": "*"
  }
}
{% endcodeblock %}

完成後到終端機輸入
{% codeblock %}
npm install -l
{% endcodeblock %}

在<code>view</code>資料夾內新增<code>sign.ejs</code>與<code>sign2.ejs</code>兩個檔案，這兩個檔案是用來接收<code>post</code>與<code>get</code>的資料並且顯示出來。

<code>index.ejs</code>程式碼如下
{% codeblock index.ejs lang:html %}
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
    <script src="//ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js"></script>
    <script type="text/javascript">
      jQuery(document).ready(function($) {
        $('#send').on('click',function(){
          var data = {
            'account': $('#ajax_account').val(),
            'pass': $('#ajax_pass').val()
          };
          $.post('/sign3', data ,function(data2) {
            $('#ajax').html(data2);
            console.log(data2);
          });
        });
      });

    </script>
  </head>
  <body>
    <h1><%= title %></h1>
    <p>Welcome to <%= title %></p>
    <h1>GET Method</h1>
    <form action="/sign" method="GET">
      <table>
        <tr>
          <td>Accout<input type="text" name="account" /></td>
        </tr>
        <tr>
          <td>Password<input type="text" name="pass" /></td>
        </tr>
        <tr>
          <td><input type="submit" value="send" /></td>
        </tr>
      </table>
    </form>

    <h1>POST Method</h1>
    <form action="/sign2" method="POST">
      <table>
        <tr>
          <td>Account<input type="account" name="account" id="account"></td>
        </tr>
        <tr>
          <td>Password<input type="pass" name="pass" id="pass"></td>
        </tr>
        <tr>
          <td><input type="submit" value="send"></td>
        </tr>
      </table>
    </form>

    <h1>AJAX Method</h1>
    <form action="#">
      <table>
        <tr>
          <td>Account<input type="account" name="account" id="ajax_account"></td>
        </tr>
        <tr>
          <td>Password<input type="pass" name="pass" id="ajax_pass"></td>
        </tr>
        <tr>
          <td><button type="button" id="send">send</button></td>
        </tr>
      </table>
    </form>

    <div id="ajax"></div>

  </body>
</html>
{% endcodeblock %}

接著開啟根目錄下的<code>app.js</code>加入<code>form</code>的<code>action</code>方法進去。

{% codeblock app.js lang:js %}
app.get('/', routes.index);
app.get('/sign', routes.sign);
app.post('/sign2', routes.sign2);
app.post('/sign3', routes.sign3);
{%endcodeblock %}

定義好對應的方法後，在到<code>routes</code>資料夾內的<code>index.js</code>撰寫方法的執行內容，程式碼如下：

{% codeblock index.js lang:js %}
//GET Method
exports.sign = function(req, res){
  res.render('sign', {
    title: 'Show Result',
    account: req.query.account,
    pass: req.query.pass
  });
};

//POST Method
exports.sign2 = function(req, res){
  res.render('sign2', {
    title: 'Show Result',
    account: req.body.account,
    pass: req.body.pass
  });
};

//AJAX Method
exports.sign3 = function(req, res) {
  res.send('Account:' + req.body.account + ', Pass:' + req.body.pass)
};
{% endcodeblock %}

<code>sign.ejs</code>與<code>sign2.ejs</code>的內容分別為：

{% codeblock sign.ejs lang:html %}
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <h1>Show Result</h1>
    <p>Accout:<%= account%></p>
    <p>Password:<%= pass%></p>
  </body>
</html>

{% endcodeblock %}

{% codeblock sign2.ejs lang:html %}
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <h1>Show Result</h1>
    <p>Accout:<%= account%></p>
    <p>Password:<%= pass%></p>
  </body>
</html>
{% endcodeblock %}

程式碼放在這：[下載](https://github.com/lighter/test)

## 參考資料
[Express 介紹](http://book.nodejs.tw/zh-tw/node_express.html)