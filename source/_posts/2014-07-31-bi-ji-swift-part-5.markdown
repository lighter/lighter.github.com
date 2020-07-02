---
layout: post
title: "[筆記]swift part 5"
date: 2014-07-31 21:41:58 +0800
comments: true
tags: 
- swift
- xcode
- iPhone
---

## closure

`closure`就有如同`c`、`objective-c`的block。`function`其實也可以當作參數一樣傳遞，下面用簡單的範例做練習。

{% img /images/swift/swift-36.jpg %}

<!-- more -->

`closure`的格式如下

{% codeblock %}
{ (parameters) -> returnType in
    statements
}
{% endcodeblock %}

單行的表示式`closure`可以省略`return`，參數的部分甚至可以省略寫成`$0`代表第一個參數，`$1`代表第二個參數，以此類推，下面用簡單的範例表示

{% img /images/swift/swift-37.jpg %}

下面這個範例只是單純的介紹寫法而已！

`sort`這個方法預設是做了升序排列，如果你希望降序排列，可以傳入一個function(`closure`)進去。

* `寫法1`很單純，傳入已經寫好的function。
* `寫法2`則是將方法名稱、參數、回傳值省略了，並且使用`$0`表示第一個參數，`$1`為第二個以以此類推。
* `寫法3`則是把參數也給省略了，直接使用`<`符號，swift則可以自動幫你推斷要將這兩個參數做`<`的邏輯。

{% img /images/swift/swift-38.jpg %}

## lazy(<del>懶惰的?</del>緩慢的) with closure

什麼是`lazy`?，`就是當你真的需要使用，才真的會分配(記憶體)給你使用`。至於以前`objcetive-c`的寫法可以參考這篇文章[Lazy Initialization with Swift](http://mikebuss.com/2014/06/22/lazy-initialization-swift/)，我覺得她寫很明確，所以我就用我理解的意思大概描述一遍，至於傳統的`objective-c`的寫法我就不贅述了，畢竟這邊是swift的note。

`lazy`在swift的使用方式如下

{% codeblock %}

lazy var lazyVar = [String]()

{% endcodeblock %}

只需要在`var`前面加上`lazy`這個keyword，還有一點要注意的是`lazy`並不能使用`let`，因為常數在初始化前是必需要有值的。

> 原本的寫法是`@lazy`後來修改為`lazy`，把小老鼠`@`給省略了。

如果你希望你的變數有會因為一些邏輯判斷而有不同的值，這時可以用`closure`來完成，下面這段code直接貼在playground是無法使用的，他會告訴你`lazy`必須是`struct`或`class`的成員。

{% codeblock %}

lazy var lazyStr:String = {
    // do you want
    return "This is a test"
}()

{% endcodeblock %}

在什麼時候你會需要使用使用`lazy`呢?如果你的變數的值，是需要等到物件初始化完成後才能明確的定義，這時你就會需要使用，下面使用一個簡單的範例；下面範例中可以看到`[unowned self]`，這是要避免`strong reference cycle`。

{% codeblock %}

class testClass {
    var name:String

    lazy var sayHiTo:String = {
        [unowned self] in
        return "Hi~ \(self.name)"
    }()

    init(name:String) {
        self.name = name

    }
}

var myClass = testClass(name: "Maya")
// 實立化一個myClass後，這時sayHiTo變數 is nil

myClass.sayHiTo
// 當sayHiTo這個變數被呼叫到才真正的給予值

{% endcodeblock %}

