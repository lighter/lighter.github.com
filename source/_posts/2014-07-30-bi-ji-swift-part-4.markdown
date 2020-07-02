---
layout: post
title: "[筆記]swift part 4"
date: 2014-07-30 21:16:15 +0800
comments: true
tags: 
- swift
- xcode
- iPhone
---

## class(類別)

類別整個跟結構的寫法沒有太大的差異，直接看範例吧！

{% img /images/swift/swift-32.jpg %}

<!-- more -->

## 繼承

{% img /images/swift/swift-33.jpg %}

## class func(類別方法)

類別方法可允許不用實立化物件而直接呼叫，只需要在方法前面加上`class`這個keyword；在`struct`則是要用`static`。

{% img /images/swift/swift-34.jpg %}

## class ref

class是參考型別，從下面的範例可以看到`myClass2`是參考`myClass1`，所以當`myClass1`有所改變，`myClass2`也會跟著改變；而struct並不會因為這樣就有所改變。

{% img /images/swift/swift-35.jpg %}