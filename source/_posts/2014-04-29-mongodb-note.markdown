---
layout: post
title: "mongodb note"
date: 2014-04-29 23:00
comments: true
tags: 
- mongodb
---

記錄一下第一次使用MongoDB發生的蠢問題...

<!-- more -->

使用`brew`安裝

{% codeblock %}
$ brew install mongodb
{% endcodeblock %}

建立`dbpath`預設的路徑資料夾

{% codeblock %}
$ sudo mkdir -p /data/db
{% endcodeblock %}

執行`mongo`

{% codeblock %}
$ mongo
{% endcodeblock %}

這時發生了無法連線的問題

{% img /images/mongodb/mongdb_1.jpg %}

似乎是並沒有啟動`mongodb`的服務，可以用這個指令看看

{% codeblock %}
$ ps -ef | grep [m]ongo
{% endcodeblock %}

啟動`mongod`

{% codeblock %}
$ mongod --dbpath /data/db --journal
{% endcodeblock %}

再開啟一個新的`Terminal`輸入`mongo`，就連到了！

{% codeblock %}
[~] $ mongo
MongoDB shell version: 2.6.0
connecting to: test
> 
{% endcodeblock %}

**Note**
----------------------------- 
1. `mongod.conf`路徑位置`/usr/local/etc/`
2. 預設`mongod.lock`路徑位置`/usr/local/var/mongodb`