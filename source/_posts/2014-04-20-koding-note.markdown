---
layout: post
title: "Koding Note"
date: 2014-04-20 09:40
comments: true
tags: 
- koding
---

之前就知道Koding這平台，也申請帳號了一陣子，但一直沒有特別去使用。最近因為想說來玩玩看吧，而且又是免費的，空間也很夠用！以下是自己記錄一下使用上遇到的一些問題，大概記錄一下。

列出已經預先幫你安裝的東西，從下圖就可以看到已經幫我們安裝的`phpmyadmin`

{% codeblock %}
> help programs
{% endcodeblock %}

{% img /images/koding/koding-2014-04-20_09-50-47.jpg %}

可以看到你的phpmyadmin url及基本的mysql指令，你可以使用它提供的指令去修改密碼

{% codeblock %}
> help phpmyadmin
{% endcodeblock %}

{% img /images/koding/koding-2014-04-20_10-00-13.jpg %}

修改mysql密碼

{% codeblock %}
mysqladmin -u root password YOUR_NEW_PASSWORD
{% endcodeblock %}

修改mysql root的帳號

{% codeblock %}
> mysql -u root -p
> Enter password:
mysql> use mysql;
mysql> update user set user='admin' where user='root';
mysql> flush privileges;
{% endcodeblock %}

<!-- more -->

## 安裝 Laravel

安裝`composer`([連結](https://getcomposer.org/download/))

{% codeblock %}
> cd
> cd Web/
> curl -sS https://getcomposer.org/installer | php
{% endcodeblock %}

{% img /images/koding/koding-2014-04-20_17-31-50.jpg %}

安裝`Laravel`([連結](http://laravel.com/docs/quick#installation))

{% codeblock %}
> php composer.phar create-project laravel/laravel your-project-name --prefer-dist
{% endcodeblock %}

{% img /images/koding/koding-2014-04-20_17-38-03.jpg %}

安裝`php5-mcrypt`

{% codeblock %}
> sudo apt-get install php5-mcrypt
{% endcodeblock %}

{% img /images/koding/koding-2014-04-20_17-53-27.jpg %}

設定`sessions`資料夾權限，路徑`/home/UserNmae/Web/laravel-sample/app/storage/sessions`

{% img /images/koding/koding-2014-04-20_17-57-42.jpg %}

最後試著在瀏覽器輸入`http://YourUserName.kd.io/laravel-sample/public/`，就可以看到`Laravel`的預設網頁。

{% img /images/koding/koding-2014-04-20_18-07-17.jpg %}
