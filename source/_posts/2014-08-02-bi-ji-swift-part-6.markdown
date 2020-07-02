---
layout: post
title: "[筆記]swift part 6"
date: 2014-08-02 15:57:18 +0800
comments: true
tags: 
- swift
- xcode
- iPhone
---

## Enums(列舉)

`enum`是定義一個通用型別的一組相關的值！我本身很少用，所以有點陌生...哈，還是來記錄一下，下面的code都是根據apple的文件練習的。

{% codeblock %}

// enum 格式
enum testEnum {

}

// 包含了東西南北的 compassPoint enum
enum compassPoint {
    case East
    case West
    case South
    case North
}

// 縮減為一行
enum compassPoint2 {
    case East, West, South, North
}

var directionToHead = compassPoint.West

// 如果知道是 compassPoint形態，則可以忽略'compassPoint'
// 直接使用 .East
directionToHead = .East

{% endcodeblock %}

<!-- more -->

## Enum with switch

{% codeblock %}

enum compassPoint {
    case East
    case West
    case South
    case North
}

var directionToHead = compassPoint.East

switch directionToHead {
    case .East:
        println("East")
    case .West:
        println("West")
    case .South:
        println("South")
    case .North:
        println("North")
    default:
        println("No value")
}
{% endcodeblock %}

## Associated Value

{% codeblock %}
enum Barcode {
    case UPCA(Int, Int, Int)
    case QRCode(String)
}

var productBarcode = Barcode.UPCA(1, 123_456_789, 222)
productBarcode = .QRCode("ABCDEFGHIJKLMNOP")

switch productBarcode {
    case .UPCA(let numberSystem, let identifier, let check):
        println("UPC-A with value of \(numberSystem), \(identifier), \(check).")
    case .QRCode(let productCode):
        println("QR code with value of \(productCode).")
}

// 輸出 "QR code with value of ABCDEFGHIJKLMNOP."
{% endcodeblock %}

## Row value

{% codeblock %}

enum Number:Int {
    case One = 1
    case Two = 2
    case Three = 3
}

// 縮寫成一行，僅需定義一開始
enum Number2: Int {
    case One = 1, Two, Three
}

// 使用 toRaw 來取得值 1
var numberOne = Number.One.toRaw()

// 將 1 轉回去取得 對應的 enum 值 One
var numberTwo = Number.fromRaw(2)

if let two = Number.fromRaw(2) {
    switch two {
        case .Two:
            println("It is number 2")
        default:
            println("Not found")
    }
}
else {
    println("Not found")
}

{% endcodeblock %}