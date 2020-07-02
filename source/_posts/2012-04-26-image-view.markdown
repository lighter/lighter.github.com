---
layout: post
title: "Image View"
date: 2012-04-26 06:09
comments: true
tags: 
- iPhone
---
在iPhone上要呈現圖需透過Image View這個元件來呈現，接著以簡單的範例來呈現。

該範例以簡單的Button事件來改變Image View呈現的圖片。

{% img /images/iPhone/imageViews1.jpeg %}
{% img /images/iPhone/imageViews2.jpeg %}

<!-- more -->

**Step 1.**建立<code>SingleView</code>專案，專案命名為<code>Image View</code>。

**Step 2.**編輯MainStoryboard.storyboard。

加入<code>Image View</code>、<code>Round Rect Button</code>兩個元件，元件的大小調整、Title這就不多做描述。

{% img /images/iPhone/imageViews3.jpeg %}

**Step 3.**加入圖片檔。

加入兩張圖片到該專案下(此範例僅使用兩張圖片作為範例)。

{% img /images/iPhone/imageViews4.jpeg %}

**Step 4.**編輯ViewController.h。

<ol>
<li><code>imageView_</code>是要用來顯示圖片。</li>
<li><code>currentImage</code>是用來判斷目前要顯示哪張圖片。</li>
<li><code>switchImage</code>為Button事件，用來改變圖片。</li>
</ol>

{% codeblock lang:objc %}
@interface ViewController : UIViewController
{
    UIImageView *imageView_;
    NSInteger currentImage;
}
@property (strong, nonatomic) IBOutlet UIImageView *imageView;
-(IBAction)switchImage:(id)sender;
@end
{% endcodeblock %}

**Step 5.**編輯ViewController.m

<ol>
<li>將<code>currentImage</code>設為0。</li>
<li>並指定<code>imageView</code>的image為哪一個圖片檔(<code>imageNamed</code>)。</li>
</ol>

{% codeblock lang:objc %}
@synthesize imageView;
- (void)viewDidLoad
{
    [super viewDidLoad];  
    currentImage = 0;
    imageView.image = [UIImage imageNamed:@"t1.jpg"];
}
{% endcodeblock %}

<code>switchImage</code>首先判斷<code>currentImage</code>是否為0(零)，條件成立時，將會改變<code>currentImage</code>，並且改變<code>imageView</code>的image，當下次在執行<code>switchImage</code>時，一樣會先判斷<code>currentImage</code>的值，並改變<code>imageView</code>的image。

{% codeblock lang:objc %}
-(IBAction)switchImage:(id)sender
{
    if(currentImage == 0){
        currentImage = 1;
        imageView.image = [UIImage imageNamed:@"t2.jpg"];
    }
    else {
        currentImage = 0;
        imageView.image = [UIImage imageNamed:@"t1.jpg"];
    }
}
{% endcodeblock %}

**Step 6.**建立關連

{% img /images/iPhone/imageViews5.jpeg %}