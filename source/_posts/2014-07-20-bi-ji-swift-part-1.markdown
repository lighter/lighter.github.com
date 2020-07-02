---
layout: post
title: "[筆記]swift part 1"
date: 2014-07-20 15:26:30 +0800
comments: true
tags: 
- swift
- xcode
- iPhone
---

最近看了一下`swift`，順手記錄一下，一開始當然是最基本的語法囉，在新的`xcode`中出現了`playground`，這是一個很適合用來練習基本的語法，不用特地去建立一個大專案，也不用特地去編譯整個專案才能得到結果，這感覺跟其他語言在終端機下執行很像。以下就是簡單的記錄

<!-- more -->

一開始建立好`playground`的檔案就可以看到他已經為你建立好基本的範例了！但這裡並不需要使用到`Cocoa`，因為基礎的語法練習似乎還不會用到內建的東西，所以其實可以把它刪除。

{% img  /images/swift/swift-1.jpg %}

## var(變數)
變數的定義格式如下

```
var VAR_NAME:TYPE = ASSIGN_VALUE
```

一開始需要使用`var`來定義這是一個變數，至於變數的形態你可以自行定義或是交由編譯器來辨別，然後變數顧名思義就是會變得數，所以之後變數的直可以任意給予新的值。變數的值可以變，但變數的形態不能變，看到`str3`為`string`的形態，後面要assign一個數字就會出現錯誤。

{% img /images/swift/swift-2.jpg %}

## let(常數)
常數的定義與變數不同，是使用`let`這個keyword，而且常數只能在一開始assign值，事後並不能更改他

{% img /images/swift/swift-3.jpg %}

## for(回圈)

下面這個範例很明顯要印出1到5的數字但不包含5(也就是1到4)，但這裡有個小小不一樣的地方，在之前的版本使用`1..5`就可以了，這根`ruby`很像，但是後來在beta3就不能使用了，要改用`..<`

{% img /images/swift/swift-4.jpg %}

如果單存只是想要跑回圈並無任何變數可以將`i`取代為`_`(底線)

{% img /images/swift/swift-5.jpg %}

現在要包含最後一個數字的範圍也要跑，可以改用`...`(三個點)

{% img /images/swift/swift-6.jpg %}

如果不想遞增的跑回圈，例如基數偶數，可以改用下面這種方式來寫，這跟以前原本的`for`回圈一樣，差別就是不用小括號

{% img /images/swift/swift-7.jpg %}

## if(如果...)

`if`，如果條件成立，就會進去執行，這很簡單啦！就不再費言了！

{% img /images/swift/swift-8.jpg %}

我覺得比較特別也比較好的地方是這個！以往`1`也就是會被認作為`true`，所以不小心該變數的值就是`1`，那這樣判斷一定會通過，但是你並不是因為該變數為`1`就想讓它通過。但swift似乎不允許這樣，所以當你這樣定義的時候就會出現錯誤訊息，如下圖

{% img /images/swift/swift-9.jpg %}

## switch(選擇)

這裡可以看到我將`money`變數定義為`3_000_000`，這個`_`底線，在swift終是被允許的，主要是讓數字容易閱讀。而swift裡的`switch case`內是不需要寫`break`的，它自行會為每個`case`做`break`。

{% img /images/swift/swift-10.jpg %}

多條件的要執行同一個的情況呢？直接在`case`內定義多個條件，用逗點隔開，甚至你可以使用範圍的方式`..<`，如下圖

{% img /images/swift/swift-11.jpg %}

## Fallthrough(貫穿)

如果你希望繼續往下執行下一個`case`，你可以使用`fallthrough`，就會繼續往下一個`case`執行，但下一個如果沒有`fallthrough`就會停止，也就是說他只會執行到下一個就停止，除非你每一個`case`內都有宣告

{% img /images/swift/swift-12.jpg %}

## Tuple(元組、組值) use with Switch

這給我的感覺有點像是陣列，但其實不是，我覺得這很好用，尤其是在回傳值得時候。下面這個範例結合`switch`來使用，這邊定義一個`tuple`的變數，裡面存放了兩個數字！很簡單吧

{% img /images/swift/swift-13.jpg %}

也可以使用條`where`條件檢查

{% img /images/swift/swift-14.jpg %}

## Labeled Statements(帶標簽的語句)

這個我覺得蠻有趣的！可以去控制區塊的回圈，直接看code可能比較清楚了

{% img /images/swift/swift-15.jpg %}

## Array(陣列)

這邊我覺得比較不一樣的是，陣列可以用`+=`直接在陣列後面加上(也就是串聯起來)，其他都蠻好理解的！

{% img /images/swift/swift-16.jpg %}

清空陣列

{% img /images/swift/swift-17.jpg %}

重複建立同樣的值，如果要將兩個陣列串聯起來，一樣使用`+`號就可以，但需注意這兩個陣咧的形態必須都一樣，下列這個範例一個是`Int`另一個是`Double`那這樣就不能串聯起來了。

{% img /images/swift/swift-18.jpg %}

## Dictionary(字典)

定義方式有兩種，如下圖

{% img /images/swift/swift-19.jpg %}

字典的操作也很簡單

{% img /images/swift/swift-20.jpg %}

