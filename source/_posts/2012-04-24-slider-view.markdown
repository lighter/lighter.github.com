---
layout: post
title: "Slider View"
date: 2012-04-24 14:30
comments: true
tags: 
- iPhone
---
Slider在iPhone比較常見是用來調整螢幕的明暗度，或是在某些圖片編輯app內用來調整RGB的比例，我們使用一個簡單的範例來操控Slider。

{% img left /images/iPhone/sliderViews1.jpeg %}簡單的範例最後要達到的結果如左圖，拖拉Slider可以觀察到數值隨著Slider的變化跟著改變。

<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
<!-- more -->
**Step 1.**建立一個<code>SingleView</code>的專案，專案的名稱為Slider View

**Step 2.**編輯MainStoryboard.storyboard

加入<code>Slider</code>、<code>Label</code>兩個元件，當中的元件屬性設定你可以隨自己喜好去做設定。

{% img /images/iPhone/sliderViews2.jpeg %}

比較需要注意的是<code>Slider</code>的屬性設定；設定它的最大值、最小值以及預設值。

{% img /images/iPhone/sliderViews3.jpeg %}

**Step 3.**編輯ViewController.h

定義<code>UISlider</code>、<code>UILabel</code>兩個型態變數，<code>setSliderValue:</code>方法為Slider被拖拉時，會改變<code>UILabel</code>的數值。
{% codeblock lang:objc %}
@interface ViewController : UIViewController
{
    UISlider *slider_;
    UILabel *label_;
}
@property(strong, nonatomic)IBOutlet UISlider *slider;
@property(strong, nonatomic)IBOutlet UILabel *label;

-(IBAction)setSliderValue:(id)sender;
@end
{% endcodeblock %}

**Step 4.**編輯ViewController.m

<code>currentValue</code>用來取得<code>Slider</code>的值；取得值的結果在放到<code>NSString</code>型態的變數value，最後將value的結果指定給<code>UILabel</code>
{% codeblock lang:objc %}
@synthesize slider = slider_, label = label_;

-(IBAction)setSliderValue:(id)sender
{
    float currentValue;
    currentValue = [self.slider value];
    
    NSString *value = [[NSString alloc] initWithFormat:@"Value is %2.f", currentValue];
    self.label.text = value;
}
{% endcodeblock %}

在一開始<code>UILabel</code>是沒有數值的，只有當你拖拉Slider才會出現值，如果要讓一近到畫面時就要有初始值，可以複製<code>setSliderValue:</code>中的方法到<code>viewDidLoad</code>即可。

**Step 5.**最後建立<code>IBOutlet</code>、<code>IBAction</code>的關聯

{% img /images/iPhone/sliderViews4.jpeg %}