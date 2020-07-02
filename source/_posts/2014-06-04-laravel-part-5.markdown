---
layout: post
title: "laravel part 5"
date: 2014-06-04 22:12:39 +0800
comments: true
tags: 
- laravel
- php
---

## 建立新的Controller

這部分主要是介紹樣板使用，主要是可以避免相同的code一再重複，只是要換掉中間的內容！首先我想要建立一個`帳號`的新增修改刪除的頁面，所以我先建立一個`Controller`；一樣使用`artisan`，開啟終端機輸入下列指令：

{% codeblock %}
php artisan controller:make AccountController
{% endcodeblock %}

{% img /images/laravel_sample/laravel-14.jpg %}

<!-- more -->

## 修改Routes

再來就是加入新的`route`啦！開啟`routes.php`，新增的code如下：

{% codeblock routes.php lang:php %}
// Account
Route::resource('account', 'AccountController');
{% endcodeblock %}

這樣一來就會幫我們建立好對應的`RESTFul`。

## 建立新的View

首先我建立一個樣板，這樣大家只要引用他，就可以擁有整個頁面的框架，僅需要各自修改各自的內容即可！

首先我建立的的路徑跟檔案是這樣的：

{% codeblock %}
app
  ⌞ views
    ⌞ layouts
      ⌞ base.blade.php
{% endcodeblock %}

接下在`base.blade.php`輸入下面的code

{% gist 43bda04901e61d152711 base.blade.php %}

需特別留意的是第45行，你會更動的內容都跟這有關係！

接著就是建立各自功能的畫面啦！我的檔案結構如下

{% codeblock %}
app
  ⌞ views
    ⌞ account
      ⌞ create.blade.php // 建立
      ⌞ edit.blade.php   // 修改
      ⌞ list.blade.php   // 列表所有帳號
      ⌞ show.blade.php   // 該帳號細部資訊
{% endcodeblock %}

接著就各自修改這四個檔案吧，很簡單！先舉一個例，先以`list.blade.php`為例，code如下：

{% codeblock list.blade.php lang:php %}
@extends('layouts.base')

@section('content')
  <h1>List Account</h1>
@stop
{% endcodeblock %}

1. `@extends(layouts.base)`就是引用原本的頁面基本的架構，這樣一來你就不用再寫一堆html header了。
2. `@section('content')`跟剛剛的`yield('content')`是做相呼應的！因此`content`可由你自己決定，但最後不要忘記加上`@stop`了。

## 修改AccountController

最後修改`AccountController`，各自對應的方法要顯示的`view`，code如下：

{% codeblock AccountController.php lang:php %}
<?php

class AccountController extends \BaseController {

    /**
     * Display a listing of the resource.
     *
     * @return Response
     */
    public function index()
    {
        $view = View::make('account.list');
        return $view;
    }


    /**
     * Show the form for creating a new resource.
     *
     * @return Response
     */
    public function create()
    {
        $view = View::make('account.create');
        return $view;
    }


    /**
     * Store a newly created resource in storage.
     *
     * @return Response
     */
    public function store()
    {
        //
    }


    /**
     * Display the specified resource.
     *
     * @param  int  $id
     * @return Response
     */
    public function show($id)
    {
        $view = View::make('account.show');
        return $view;
    }


    /**
     * Show the form for editing the specified resource.
     *
     * @param  int  $id
     * @return Response
     */
    public function edit($id)
    {
        $view = View::make('account.edit');
        return $view;
    }


    /**
     * Update the specified resource in storage.
     *
     * @param  int  $id
     * @return Response
     */
    public function update($id)
    {
        //
    }


    /**
     * Remove the specified resource from storage.
     *
     * @param  int  $id
     * @return Response
     */
    public function destroy($id)
    {
        //
    }
}
{% endcodeblock %}

可以注意到`View::make()`內都是用`account.XXX`，`account`代表的就是`account`目錄路徑下的各自的檔案。

如果成功的話可以看到這樣的畫面

{% img /images/laravel_sample/laravel-15.jpg %}

功能的話...to be continued
