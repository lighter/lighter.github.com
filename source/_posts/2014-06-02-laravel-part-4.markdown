---
layout: post
title: "laravel part 4"
date: 2014-06-02 09:16:15 +0800
comments: true
tags: 
- laravel 
- php
---

這篇會展是簡單的登入檢查、登入的`Session`建立、登出，這三個；主要是會使用到一些簡單的`route`、`laravel`的驗證、`Session`的建立。廢話不多說，往下看吧。

首先建立一個簡單的`view`用來查看目前登入狀態，建立的路徑是`app/views/show/index.blade.php`，我刻意多開了一個`show`資料夾。

{% codeblock %}
app
  ⌞views
    ⌞show
      ⌞index.blade.php
{% endcodeblock %}

`show/index.blade.php`的code如下(這邊可以先忽略變數怎麼來的...往下看你就可以知道了)：

{% gist 38e234e12e9e98652d5a %}

再來就是增加`route`的設定，開啓`routes.php`，加入下面的code：

{% codeblock routes.php lang:php %}
// 登入
Route::post('login', ['uses' => 'HomeController@doLogin']);

// 登出
Route::get('logout', ['uses' => 'HomeController@doLogout']);

// show
Route::get('show', ['uses' => 'HomeController@show']);
{% endcodeblock %}

這邊我都香要使用的方法都寫在`HomeController`內。因此會依據網址的不同執行`HomeController`內對應的方法。

`HomeController.php`加入的code如下，解釋我都寫在註解上了。

{% codeblock HomeController.php lang:php %}
// 登入
public function doLogin()
{
  // 驗證規則
  $rules = array(
    'email'    => 'required|email', // 必填欄位，email格式
    'password' => 'required|alphaNum|min:3' // 必填欄位，必須是字母或數字，不得小於3位
  );

  // 驗證
  $validator = Validator::make(Input::all(), $rules);

  // 規則驗證失敗
  if ($validator->fails()) {

    // 回到首頁，並回傳錯誤訊息，與所有輸入的欄位，除了密碼
    return Redirect::to('/')
      ->withErrors($validator)
      ->withInput(Input::except('password'));
  }
  else {
    $userdata = array(
        'email'    => Input::get('email'),
        'password' => Input::get('password')
      );

    // 與資料庫驗證
    if (Auth::attempt($userdata)) {

      // 驗證成功，並增加一個session key value值
      Session::put('login_success', 1);

      // 導向show/index.blade.php
      return Redirect::to('/show');
    }
    else {
      return Redirect::to('/');
    }
  }
}

public function doLogout()
{
  Auth::logout();

  // 刪除登入成功的key 值
  Session::forget('login_seuccess');
  return Redirect::to('/');
}

public function show()
{
  // 取得所有session的資料
  $all_session_data = Session::all();
  $data['all_session_data'] = $all_session_data;

  if ( Session::has('login_success') && Auth::check() ) {
    $data['login_status'] = 'success';
  }
  else {
    $data['login_status'] = 'failure';
  }

  // 這邊可以注意$data的login_status這個key值
  // 跟 show/index.blade.php 使用的變數做對應
  $view = View::make('show.index', $data);
  return $view;
}
{% endcodeblock %}

如果登入成功的話，可以看到登入成功的訊息

{% img /images/laravel_sample/laravel-13.jpg %}
