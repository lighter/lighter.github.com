---
layout: post
title: "[筆記] 安裝 Vagrant"
date: 2014-06-27 22:36:58 +0800
comments: true
tags: 
- vagrant
---

安裝環境 OS X 10.9.3

Step 1. 下載 [Vagrant](http://www.vagrantup.com/downloads.html)

Step 2. 下載OS

開啟終端機，建立好一個`vagrant`的資料夾

```
$ mkdir vagrant
$ cd vagrant
```

[Vagrantbox](http://www.vagrantbox.es/)，這邊有很多已經裝好的box，你可以自己找行你需要的。

這邊我安裝的是`Ubuntu precise 64 VirtualBox`，他的網址是`http://files.vagrantup.com/precise64.box`。接著在終端機輸入下面的指令安裝

```
$ vagrant box add Ubuntu64 http://files.vagrantup.com/precise64.box
```

安裝完畢後，需將這個虛擬機初始化並且啟動

```
$ vagrant box init Ubuntu64
$ vagrant up #啟動
```

啟動完成後，需要連線到這台虛擬機

```
$ vagrant ssh
```

## Vagrant 指令

```
$ vagrant halt #關閉
$ vagrant reload #重啟
$ vagrant status #狀態
$ vagrant destroy #刪除
$ vagrant list #列出虛擬機
```

### 參考資料

* [Vagrant](http://www.vagrantup.com/)
* [\[Note\] 使用 Vagrant進行部署練習](http://vinn.logdown.com/posts/2014/05/22/note-use-of-vagrant-deployment)
* [Vagrantbox.es](http://www.vagrantbox.es/)
* [用VM才是好的工程師-vagrant篇(入門版)](http://eva0919.github.io/2013/04/26/%E7%94%A8vm%E6%89%8D%E6%98%AF%E5%A5%BD%E7%9A%84%E5%B7%A5%E7%A8%8B%E5%B8%AB-vagrant%E7%AF%87%E5%85%A5%E9%96%80%E7%89%88/)
* [\[教學\]使用Vagrant練習環境佈署](http://gogojimmy.net/2013/05/26/vagrant-tutorial/)

