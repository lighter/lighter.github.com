---
layout: post
title: "IE scroll bar hidden not work"
date: 2015-01-12 23:07:39 +0800
comments: true
tags: 
- jquery
---
情境是這樣的，今天我不想要顯示scroll bar所以用css的`overflow-y`隱藏掉，在Chrome測試時寫一個`mousewheel` event是正常的，但是在ie似乎原本的寫法不行，上網google了一下，最後使用這個解法，註冊一個`wheelDelta` event，阿然後一樣...在`mousewheel`事件內來算`scrollTop`，很簡單，但紀錄一下...

{% codeblock lang:javascript %}
$(function(){
  $.event.props.push('wheelDelta');
  $('tbody').on('mousewheel', function(e){
    var delta = e.wheelDelta || -e.detail;
    var scrollTop = $(this).scrollTop();
    $(this).scrollTop(scrollTop += (delta < 0 ? 1 : -1) * 30);
  }); 
});
{% endcodeblock %}

整個範例CODE我放在[GIST](https://gist.github.com/lighter/aac71f62b1fb4f802fbc)紀錄一下。