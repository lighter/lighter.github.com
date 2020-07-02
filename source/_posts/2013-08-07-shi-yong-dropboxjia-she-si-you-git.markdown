---
layout: post
title: "使用Dropbox架設私有Git"
date: 2013-08-07 09:11
comments: true
tags: 
- Git
---

首先需要在專案資料夾中初始化Git

{% codeblock %}
$ git init
{% endcodeblock %}

接著在Dropbox中建立你要共享Git的資料夾，並且輸入以下的指令來初始化

{% codeblock %}
$ git init --bare
{% endcodeblock %}

回到專案資料夾中，加入Dropbox這個遠端的repo

{% codeblock %}
$ git remote add origin ~/Dropbox/XXXX
{% endcodeblock %}

接著將專案推倒Dropbox中

{% codeblock %}
$ git push origin master
{% endcodeblock %}

在另外一台電腦則需要將Dropbox的Git專案clone下來

{% codeblock %}
$ git clone ~/Dropbox/XXXX
{% endcodeblock %}

如果A已經變動了專案並且push到Dropbox了，B要將Dropbox變動的取出來可使用

{% codeblock %}
$ git fetch origin master
{% endcodeblock %}


