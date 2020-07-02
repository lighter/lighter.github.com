---
layout: post
title: "laravel part 6"
date: 2014-06-07 00:58:18 +0800
comments: true
tags: 
- laravel
- php
---

這個章節就來介紹

1. 帳號列表
2. 建立帳號

這裡會介紹到如何`select`跟`insert`資料,以及`URL`的使用!

<!-- more -->

## 帳號列表

首先開啟`AccountController.php`,輸入以下的code:

{% codeblock AccountController.php lang:php %}
  public function index()
  {
    $accounts = User::all();
    $view = View::make('account.list')
      ->with('accounts', $accounts);
    return $view;
  }
{% endcodeblock %}

這裡可以看到撈取資料非常簡單!這裡使用的是[Eloquent ORM](http://kejyun.github.io/Laravel-4-Documentation-Traditional-Chinese/docs/eloquent/#introduction)的方式,相關介紹官方文件已經說得很清楚了,這邊就不再多贅述了.

1. 簡單的一行`User::all()`,比起以前自己要寫好`sql`語法,然後再使用`db`方法去query,相對之下這少了很多行code,但是我想...複雜的sql語法可能還是要自己寫;跟codeigniter相較之下我覺得這個更簡潔了...
2. 帶入查詢結果到`view`,有寫過codeigniter應該都不陌生..跟`$this->load->view('view', $data)`很相像.

只是我覺得有點怪的是,這樣似乎資料就不再Model了,似乎已經在Controller就可以做掉這些,在codeigniter都會把這些sql的東西寫到特定的Model檔案,然後Controller去load這些Model,至於深入原因有時間再找找看資料好了.
 
Note:如果你的code有錯誤,在網頁出現到下圖的錯誤訊息,但卻沒有顯示code的錯誤問題在哪

{% img /images/laravel_sample/laravel-16.jpg %}

可以開啟`app/config/app.php`這支檔案,找到`debug`這個key值,並把它改為`true`,這樣就可以顯示錯誤訊息了:

{% codeblock app.php lang:php %}
'debug' => true,
{% endcodeblock %}

這邊可以很明確的看到`$accounts = User:all();`少一個分號!

{% img /images/laravel_sample/laravel-17.jpg %}

再來開啟`app/views/account/list.blade.php`輸入下面的code:

{% gist 5abb468d5c547c1305e7 list.blade.php %}

> 下面數字列表請依照code上的註解數字.

1. 用來顯示`session`的訊息, 例如建立帳號時回到這頁要顯示建立成功的字樣,這在之後會用到.
2. 這裡使用'URL`來建立連結, 前面網頁漏漏長的http也不用你寫了.
3. 這裡的`$accounts`跟controller內的key值是相對應的. 因為回傳的是多比陣列並且是依照資料表的column欄位名稱,所以後面都適用`$value->COLUMN_NAME`.
4. 這裡`Show`跟`Edit`都很單純用網址的方式就可以, 刪除就比較特別了, 因為Laravel是遵照`RESTful`, 一般的連結是發出`GET request`, 無法發出`Delete request`,所以需要借由`form`內的`button`來產生`Delete request`(參考[這篇](http://stackoverflow.com/a/19645142/685060))

如果都沒問題的話應該可以看到下面的畫面

{% img /images/laravel_sample/laravel-18.jpg %}

## 建立帳號

首先先來編輯建立帳號的畫面! 開啟`app/views/account/create.blade.php`, 輸入下面的code

{% gist aa6f3eb91ff6f2e0a03e create.blade.php %}

這邊並沒有什麼太複雜的,大概依照註解的號碼解釋一下

1. 有錯誤訊息時,會使用`ul`的條列出你輸入的資料哪裡有問題.
2. 表單要送出的`url`路徑,可以直接指定`controller`,無需其他的參數.
3. 這邊我覺得好用的地方是`Input::old()`,取回就資料,以往我為了要保留住舊資料,所以就將`button`寫成`ajax`的方式送出資料.
4. 表單的送出按鈕.

{% img /images/laravel_sample/laravel-19.jpg %}

接下來就是寫入資料庫的動作,開啟`app/controller/AccountController.php`,因為`laravel`是使用`RESTful`,所以預設會使用`store`這個function. code如下:

{% gist d28b28c4c16ccd04c40b AccountController.php %}

1. 驗證規則: 我個人覺得很方便,不然以往我都是使用`empty`等php的function一個一個條件檢查.而且連Email檢查也不用寫正規式檢查,可以看到密碼的部分`min:3`,最少為3個數字.
2. 返回舊資料: 這樣在返回建立帳號頁面時,也不會遺漏使用原本輸入的資料,就不用全部重新輸入了!
3. 建立帳號: 這邊也不用自己寫sql語法,整個很直覺,指定好sql欄位的值,最後`save`.
4. 返回建立成功訊息: 使用flash的session,至於在哪顯示?可以在`list.blade.php`中找到`@if (Session::has('message'))`這區塊的code,就是用來顯示這邊的`message`.

{% img /images/laravel_sample/laravel-20.jpg %}