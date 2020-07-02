---
layout: post
title: "[筆記]swift Part 7"
date: 2014-08-04 23:49:38 +0800
comments: true
tags: 
- swift
- xcode
- iPhone
---

## Subscripts 下標語法

練習範例

{% codeblock %}
struct testStruct {
    let multiplier: Int
    subscript(index:Int) -> Int {
        return multiplier * index
    }
}

let stuct = testStruct(multiplier: 20)
stuct[3]
{% endcodeblock %}

<!-- more -->

{% codeblock %}
import Cocoa

func randomizer(#range:Range<UInt32>) -> UInt32 {
    return range.startIndex + arc4random_uniform(range.endIndex - range.startIndex + 1)
}

struct LevelMaker {
    var grid = Array<Array<UInt32>>()

    mutating func makeGrid() {
        var numColumns = 27
        var numRows = 52
        for column in 0..<numColumns {
            var newRow = Array<UInt32>()
            for row in 0..<numRows {
                newRow.append(randomizer(range:1..<32))
            }
            self.grid.append(newRow)
        }
    }

    subscript(row:Int, column:Int) -> UInt32 {
        return grid[row][column]
    }

    init() {
        makeGrid()
    }
}

var level2 = LevelMaker()
level2.grid
level2.grid[2][5]
{% endcodeblock %}