---
layout: post
title: "laravel part 3"
date: 2014-05-31 17:18:37 +0800
comments: true
tags: 
- laravel
- php
---

接著就是建立資料方面的事情啦！登入總要有先有資料對吧！

## Step 1. Mysql設定

開啓`phpmyadmin`，建立一個新的Database，這邊取名為`laravel_sample`，如下圖

{% img /images/laravel_sample/laravel-6.jpg %}

再來就是設定`laravel`的資料庫連線，開啓`database.php`，找到`mysql`的設定，輸入`database`名稱還有連線的帳號密碼

{% codeblock database.php lang:php %}
'mysql' => array(
	'driver'    => 'mysql',
	'host'      => 'localhost',
	'database'  => 'laravel_sample',
	'username'  => 'root',
	'password'  => 'password',
	'charset'   => 'utf8',
	'collation' => 'utf8_unicode_ci',
	'prefix'    => '',
),
{% endcodeblock %}

## Step 2. 建立Table

`laravel`的`artisan`很方便，這邊你不需要自己在`phpmyadmin`手動一個一個輸入column name和type，也不用自己寫mysql的create，可以透過指令和一些code來幫你完成建立Table。

開啟終端機，切換到專案的路徑下，以我的環境來說，我的的專案是在`/Application/XAMPP/htdocs/laravel_sample/`；然後輸入下面的指令：

`php artisan migrate:make create-users-table`

實際上會幫你建立一個 `時間` 加上 `create-users-table`的一個檔案，如下圖：

{% img /images/laravel_sample/laravel-7.jpg %}

接著開啟這個檔案，這個檔案的路徑會在`app/database/migrations/`下

在這檔案會看到兩個method，一個是用來建立，另一個當然就是拿來還原的囉！建立的程式主要就是用來建立Table，直接看code吧，如下：

{% codeblock %}
/**
 * Run the migrations.
 *
 * @return void
 */
public function up()
{
	Schema::create('users', function(Blueprint $table)
	{
		$table->increments('id');
		$table->string('name', 32);
		$table->string('username', 32);
		$table->string('email', 320);
		$table->string('password', 64);
		$table->timestamps();
	});
}

/**
 * Reverse the migrations.
 *
 * @return void
 */
public function down()
{
	Schema::drop('users');
}
{% endcodeblock %}

再次回到終端機，輸入下面的指令來建立Table

`php artisan migrate`

阿...發生錯誤了！

{% img /images/laravel_sample/laravel-8.jpg %}

似乎是無法跟mysql連線，請試著修改`database.php`的`host`，修改為`127.0.0.1`，或者你可以參考這篇[Laravel setup of migrations using Artisan is failing](http://stackoverflow.com/questions/14219692/laravel-setup-of-migrations-using-artisan-is-failing)，直接指定`unix_socket`的路徑，因為我有在遠端安裝的時候遇過這情形。

{% codeblock database.php lang:php %}
'mysql' => array(
	'driver'    => 'mysql',
	'host'      => '127.0.0.1',
{% endcodeblock %}

修改完再執行一次吧！

{% img /images/laravel_sample/laravel-9.jpg %}

成功囉~

來看看`phpmyadmin`是不是真的出現`users`的Table呢！？

{% img /images/laravel_sample/laravel-10.jpg %}

## Step 3. 建立資料

建立資料的部分也不用你自己到資料庫裡面一筆一筆key，一樣可以透過`artisan`幫你完成，但你要先寫好code！在`app/database/seeds/`下建立一個`UsersTableSeeder.php`(注意：`U`要大寫)。建立完成後先不急著開起它來編輯，先看到`DatabaseSeeder.php`這隻檔案，預設執行時來執行這隻檔案，再由這隻檔案的內容去看你要執行哪隻檔案...直接看code吧！

{% codeblock  DatabaseSeeder.php lang:php %}
public function run()
{
	Eloquent::unguard();

	$this->call('UsersTableSeeder'); // 拿掉原本的註解，注意這邊預設User後是沒有加`s`的記得自己補上，跟檔案名稱對應
}
{% endcodeblock %}

接下來就是編輯你要寫的內容了，開啟`UsersTableSeeder.php`，code如下：

{% codeblock UsersTableSeeder.php lang:php %}
<?php

class UsersTableSeeder extends Seeder
{

    public function run()
    {
        DB::table('users')->delete(); // 每次執行都先清理資料表單
        User::create(array(
            'name' => 'lighter',
            'username' => 'awesome',
            'email' => 'test@gmail.com',
            'password' => Hash::make('awesome'),
        ));

        User::create(array(
            'name' => 'Tom',
            'username' => 'John',
            'email' => 'test2@gmail.com',
            'password' => Hash::make('awesome')
        ));
    }

}
{% endcodeblock %}

完成之後到終端機輸入下面的指令

`php artisan db:seed`

成功囉～這樣以後如果需要很多測試資料，就不用手動一筆一筆操作建立了！而且轉換資料庫也方便很多，不用寫多套SQL語法。

{% img /images/laravel_sample/laravel-11.jpg %}

{% img /images/laravel_sample/laravel-12.jpg %}