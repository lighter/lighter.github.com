---
layout: post
title: "[筆記]swift part 3"
date: 2014-07-26 11:18:42 +0800
comments: true
tags: 
- swift
- iPhone
- xcode
---

## Struct(結構)

在`swift`，結構基本上跟類別(class)很像，直接先看簡單的範例吧！下面建立了一個`car`的結構，裡面包含了`color`和`number`，接著實立化這個`car`結構，整個跟類別很像吧！

{% img /images/swift/swift-29.jpg %}

<!-- more -->

接著再一個簡單的範例，`struct`裡面使用了別的`struct`，以及`struct`內含有方法

{% img /images/swift/swift-30.jpg %}

接這個範例能夠顯現`struct`跟類別不同的地方，在下面這個範例中可以看到`strunct`內的方法，會去操作內部的屬性，也就是`myName`，只要會去操作到內部的屬性值，在方法前面都需要加上`mutating`這個 keyword !

{% img /images/swift/swift-31.jpg %}

這是我參考stackoverflow這篇答案的[[http://stackoverflow.com/a/24035861/685060](http://stackoverflow.com/a/24035861/685060)]，以下是我大意理解的節錄

> 結構跟類別(class)很像，不同的地方是結構有兩種模式，` immutable`/`mutable`，而類別通常的操作是用`reference`，而這種操作方式算是`mutable`，因為使用`reference`的方式操作，如果又是`immutable`的情況，這樣顯得非常困難。



