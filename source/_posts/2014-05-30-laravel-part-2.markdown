---
layout: post
title: "laravel part 2"
date: 2014-05-30 23:16
comments: true
tags: 
- php
- laravel
---

第二部分就是套用[bootstrap](http://getbootstrap.com/)和[jquery](http://jquery.com/)，我將檔案放到專案根目錄下的`public`資料夾內。或者你想引用CDN的也可以，這樣你就不用下載任何東西。

> jquery其實應該是用不到，只是習慣都會載入而已，接下來也不會有任何event事件需要寫！

所以我的檔案結構如下

```
laravel_sample
	⌞app
	⌞public
		⌞bootstrap-3.1.1-dist
		⌞jquery-1.11.1.min.js	
```

接著我想把首頁換掉，換成登入的畫面，所以先在`app/views`下建立一個`login.blade.php`，至於`blade`是什麼，請參考[laravel](http://laravel.com/docs)的官方文件吧。

然後輸入下面的code：

{% gist fac184def5041dd0cdae login.blade.php %}

接著修改`routes.php`，把預設首頁換成`login`，code如下：

{% codeblock routes.php lang:php %}
Route::get('/', function()
{
	return View::make('login');
});
{% endcodeblock %}

重新整理網頁，就可以看到登入的畫面啦！

{% img /images/laravel_sample/laravel-5.jpg %}