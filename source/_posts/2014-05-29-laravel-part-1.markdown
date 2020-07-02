---
layout: post
title: "laravel part 1"
date: 2014-05-29 13:16
comments: true
tags: 
- php 
- laravel
---

## 安裝 Laravel

要在自己的電腦上先安裝[composer](https://getcomposer.org/)。

然後在終端機下這樣的指令

{% codeblock %}
composer create-project laravel/laravel ProjectName
{% endcodeblock %}

安裝完成後開啟網頁輸入網址 `http://localhost/laravel_sample/`，你會看到下面這樣的畫面

{% img /images/laravel_sample/laravel-1.jpg %}

預設應該是在`public`資料夾下，所以改成這個路徑`http://localhost/laravel_sample/public/index.php`，但...還是一樣失敗了   囧rz

{% img /images/laravel_sample/laravel-2.jpg %}

這似乎是`storage`資料夾權限的問題，所以用終端機切換到`app`這個路徑就可以看到`storage`資料夾，然後輸入下面的指令。

> 如果有權限上的問題在前面加個`sudo`

{% codeblock %}
chmod -R 777 storage
{% endcodeblock %}

再次重整網頁！成功啦！

{% img /images/laravel_sample/laravel-3.jpg %}

接著我想把public這層路徑拔掉，所以把這層資料夾的`.htaccess`, `index.php`, `favicon.ico`這三個主要的檔案搬到外面，接著開啟`index.php`做些小修整，主要是修改兩個路徑而已，程式碼如下：

{% codeblock index.php lang:php %} 
require __DIR__.'/bootstrap/autoload.php';
$app = require_once __DIR__.'/bootstrap/start.php';
{% endcodeblock %}

把路徑修改成 `http://localhost/laravel_sample/`

{% img /images/laravel_sample/laravel-4.jpg %}



