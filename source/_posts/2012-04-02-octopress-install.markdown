---
layout: post
title: "octopress install with github"
date: 2012-04-02 21:41
comments: true
tags: 
- Octopress
- Github
---
來記錄一下如何架設Octopress

我使用的是Mac OS X 10.7.3

**Step 1.**建立github page
在[github](https://github.com/)網站建立一個Repositories，project名稱為xxxxx.github.com

**Step 2.**Octopress Setup

可參考[Octopress Setup](http://octopress.org/docs/setup/)

{% codeblock %}
$ rvm install 1.9.2 && rvm use 1.9.2
$ git clone git://github.com/imathis/octopress.git octopress
$ cd octopress    # If you use RVM, You'll be asked if you trust the .rvmrc file (say yes).
$ ruby --version  # Should report Ruby 1.9.2
$ gem install bundler
$ bundle install
$ rake install
{% endcodeblock %}

<!--more-->

安裝的過程中似乎因為我有安裝Xcode所以有出現一些問題，ruby也裝失敗了，而我是參考這兩篇解決的:

1. [在 Mac 上使用 RVM 安裝 Ruby 1.9.3 時錯誤](http://gogojimmy.net/%E6%8E%83%E9%9B%B7/RVM/Ruby%201.9.3/install-ruby-1-dot-9-3-via-rvm-on-mac-problem/)

2. [Octopress安裝筆記](http://blog.visioncan.com/2011/install-octopress/)

**Step 3.**發佈到github

{% codeblock %}
rake setup_github_pages
{% endcodeblock %}

會要你輸入你的github pages repository url(例如：git@github.com:xxxxx/xxxxx.github.com.git)

其實這段在你建立的github網頁中即可找到，按下Enter後在執行下面的指令

{% codeblock %}
$ rake generate
$ rake deploy
{% endcodeblock %}

過個10分鐘左右，會收到github的來信，接著再把檔案push到github上

{% codeblock %}
$ git add .
$ git commit -m 'your message'
$ git push origin source
{% endcodeblock %}

輸入你的github project名稱就可以看到你的網頁了。

如果要在本機端觀看你的網站可輸入
{% codeblock %}
rake preview
{% endcodeblock %}

<http://localhost:4000>即可看到你的網站
