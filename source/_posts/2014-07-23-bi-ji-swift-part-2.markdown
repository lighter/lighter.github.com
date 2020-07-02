---
layout: post
title: "[筆記]swift part 2"
date: 2014-07-23 16:29:37 +0800
comments: true
tags: 
- swift
- xcode
- iPhone
---

## Function(函式，方法)

方法的一開始前面一定有`func`這個關鍵字，我是有點覺得奇怪...何不乾脆用完整個名稱就好了，為什麼要縮減...成`func`。

{% img /images/swift/swift-21.jpg %}

<!-- more -->

## External Parameter Names(外部參數名)

`func`要帶入參數直接在`( )`內寫入，其中比較不同的是否有宣告外部參數，為了code的可讀性，加上外部參數名，可以直接了解該參數的作用，外部參數名跟參數名稱可以不一樣，如果你要將外部參數跟參數名稱設定一樣可以在參數名稱前面加上一個`#`號，這樣可省去重複打一樣的名稱。

{% img /images/swift/swift-22.jpg %}

## 回傳值

一個`func`要有回傳值，直接在`( )`後加上`-> TYPE`

{% img /images/swift/swift-23.jpg %}

如果有多個值要回傳，可以使用`Tuple`

{% img /images/swift/swift-24.jpg %}

方法也可以當作參數傳入

{% img /images/swift/swift-25.jpg %}

## Nested Function(嵌套函式)

`func`回傳除了一般的`Int`, `String`，也可以回傳`func`

{% img /images/swift/swift-26.jpg %}

如果有多個參數要傳入，除了可以考慮組成一個陣列在傳入，或是可以用`...`當作有多個參數要傳入

{% img /images/swift/swift-27.jpg %}

## inout

這其實跟指標很像，下面的範例可以看到`sum`傳入的是`&sum`也就是該參數的位置，所以計算的結果也會改變`sum`的值

{% img /images/swift/swift-28.jpg %}