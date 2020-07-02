---
layout: post
title: "Laravel part 8"
date: 2014-06-21 22:37:03 +0800
comments: true
tags:
- laravel
- php
---
這章節只是想做個整理，這些只是我根據官方文件還有一些影片自己實做，然後加以整理大概記錄一下，主要是釐清一些過程使用，所以幾乎都不會詳細的描述。如果有問題歡迎一起討論。

目前使用比較多的還是codeigniter，相較之下laravel的debug比ci界面好看多了，還有`artisan`也真的蠻好用的。`Eloquent`真的是很好用，但是我不太確定當複雜的sql查詢時，很多大量的子查詢，還有個家sql提供的內件方法也都不一樣時，是否也可寫的這麼容易?就我目前工作上使用ci，我都是自己寫sql使用bind的方式，至於為什麼要用bind，似乎也是oracle的重複查詢時速度會比較快(因為cache)，有時間再來查查資料吧。

整個完整的code我也放在GIT上供大家參考囉[\[Laravel GIT\]](https://github.com/lighter/Laravel_sample)

1. [Laravel part 1](https://lighter.github.io/2014/05/29/laravel-part-1/) - 安裝 `Laravel`
2. [Laravel part 2](https://lighter.github.io/2014/05/30/laravel-part-2/) - 安裝`jquery`、`bootstrap`
3. [Laravel part 3](https://lighter.github.io/2014/05/31/laravel-part-3/) - 建立資料庫
4. [Laravel part 4](https://lighter.github.io/2014/06/02/laravel-part-4/) - 登入 & 簡易Session操作
5. [Laravel part 5](https://lighter.github.io/2014/06/04/laravel-part-5/) - 簡單的樣板使用
6. [Laravel part 6](https://lighter.github.io/2014/06/07/laravel-part-6/) - 帳號建立 & 列表
7. [Laravel part 7](https://lighter.github.io/2014/06/09/laravel-part-7/) - 帳號修改 & 顯示帳號資訊 & 修改帳號 & 刪除帳號
