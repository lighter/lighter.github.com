---
layout: post
title: "laravel part 7"
date: 2014-06-09 00:02:13 +0800
comments: true
tags: 
- laravel
- php
---
這也是最後一個章節了，將會介紹到

1. 顯示帳號資訊
2. 修改帳號資訊
3. 刪除帳號

<!-- more -->

## 顯示帳號資訊

開啓`AccountController.php`，找到`show`這個方法，修改的code如下

{% codeblock AccountController.php lang:php %}
  public function show($id)
  {
    $account = User::find($id);
    $view = View::make('account.show')
      ->with('account', $account);
    return $view;
  }
{% endcodeblock %}

這邊要搜尋帳號也很簡單，使用`User::find($id)`就可以根據`ID`找到對應的帳號資料，也不用特地去寫些sql語法，但這應該只單存這種簡單的情況下使用吧！如果有多個Table要Join，或是有些特殊的欄位條件要篩選應該就沒這容易了。

最後將這個搜尋結果塞到`view`裡面使用，所以接下來就是編輯view的部分；開啓`app/views/account/show.blade.php`，輸入的code如下

{% gist 941fbf8d6296043346f6 show.blade.php %}

這邊很簡單你要顯示的欄位資料就使用`->`這樣的方式操作，example: `$account->email`

## 修改帳號資訊

編輯會呼叫到`AccountController.php`的`edit`方法，code如下

{% codeblock AccountController.php lang:php %}
  public function edit($id)
  {
    $account = User::find($id);

    $view = View::make('account.edit')
      ->with('account', $account);
    return $view;
  }
{% endcodeblock %}

這邊跟顯示的地方並沒有什麼不同！接著就來看`app/views/account/edit.blade.php`吧

{% gist f5984711c8d1a80d48fe edit.blade.php %}

這邊大致上跟新增很像！唯一比較不一樣的是{% raw %}`{{ Form model }}`{% endraw %}(可以看到註解1的地方)，這邊要指定更新的方法，`account.update`就是呼叫`AccountController`的`update`方法，並帶入`$account->id`帳號ID；還有設定`'method' => 'PUT'`，這是因為要符合`restful`。

接下來就來寫更新資料的`update`方法吧！code如下

{% codeblock AccountController.php lang:php %}
  public function update($id)
  {
    $rules = array(
      'email' => 'required|email',
      'username' => 'required',
      'name' => 'required'
    );

    $validator =  Validator::make(Input::all(), $rules);

    if($validator->fails()) {
      return Redirect::to('account/' . $id . '/edit')
        ->withErrors($validator)
        ->withInput(Input::all());
    }
    else {
      $account = User::find($id);
      $account->email = Input::get('email');
      $account->username = Input::get('username');
      $account->name = Input::get('name');
      $account->save();

      Session::flash('message', '修改成功');
      return Redirect::to('account');
    }
  }
{% endcodeblock %}

這邊幾乎都跟建立資料時一樣，我就不再贅述了。

## 刪除帳號

前一個章節已經建立好了刪除的連結，但是尚未在controller內寫方法，其實也很簡單，刪除預設會去呼叫`destroy`這個方法，所以找到`AccountController.php`內的`destroy`，code如下

{% codeblock AccountController.php lang:php %}
  public function destroy($id)
  {
    $account = User::find($id);
    $account->delete();

    Session::flash('message', '刪除成功');
    return Redirect::to('account');
  }
 {% endcodeblock %}
 
 直接看code就可以理解！找到該筆帳號(根據ID)，最後`delete`！很簡單吧...