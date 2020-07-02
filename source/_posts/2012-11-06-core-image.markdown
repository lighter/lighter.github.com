---
layout: post
title: "Core Image"
date: 2012-11-06 19:01
comments: true
tags: 
- iPhone
---
`Core Image`是一項Mac OS X的技術，利用機器的繪圖處理器來作影像相關特效。於2004年8月的世界開發者大會(WWDC)中展示。(引用自[維基百科Core Image](http://zh.wikipedia.org/wiki/Core_Image))

我們可以透過`Core Image`的框架(framework)來調整圖片的顏色、亮度…等。而Core Image包含了很多的濾鏡的效果，這些都可以在Document中找到。

而`Core Image`有幾個重要的類別

1. `CIContext`: 所有Core Image的處理都透過CIContext完成。這有點類似於Core Graphics或OpenGL的Context。
2. `CIImage`: 該類別可持有圖片檔案。它可透過UIImage、圖片檔案或者是像素檔案來建立。
3. `CIFilter`: 濾鏡類別擁有字典(dictionary)，可以指定濾鏡效果的，例如自然飽和度(vibrance)、色彩反轉(color inversion)、剪裁(cropping)...等。

<!-- more -->

**Step 1. 加入`CoreImage.framework`**

加入CoreImage.framework，並且準備好一張圖片加入到專案中。

{% img /images/CoreImage/1.png %}

**Step 2. 編輯`MainStoryboard.storyboard`**

加入

1. UIImageView
2. Round Rect Button

{% img /images/CoreImage/2.png %}

這邊加入3個按鈕，分別用來呈現不同的效果。

**Step 3. 編輯ViewController**

首先編輯`ViewController.h`，程式碼如下：

{% codeblock ViewController.h lang:objc %}
#import <UIKit/UIKit.h>

@interface ViewController : UIViewController
{
    CIContext *context;
    CIFilter *filter;
    CIImage *inputImage;    //輸入的圖片
    CIImage *outputImage;   //輸出濾鏡效果的圖片
}
@property (strong, nonatomic) IBOutlet UIImageView *imageView;
- (IBAction)filter1:(id)sender; //濾鏡效果1
- (IBAction)filter2:(id)sender; //濾鏡效果2
- (IBAction)filter3:(id)sender; //濾鏡效果3
@end
{% endcodeblock %}

`ViewController.m`的程式碼如下：

首先在`viewDidLoad`方法指定圖片給`imageView`。
{% codeblock ViewController.m lang:objc %}
@implementation ViewController
@synthesize imageView;
- (void)viewDidLoad
{
    [super viewDidLoad];
    UIImage *img = [UIImage imageNamed:@"test.jpg"];
    imageView.image = img;
    inputImage = [[CIImage alloc] initWithImage:[UIImage imageNamed:@"test.jpg"]];
}
. . . . . 略
{% endcodeblock %}

接著開始撰寫濾鏡效果的按鈕方法，這邊只介紹一個，程式碼如下：

{% codeblock ViewController.m lang:objc %}
- (IBAction)filter1:(id)sender {
	//1
    inputImage = [[CIImage alloc] initWithImage:[UIImage imageNamed:@"test.jpg"]];
    
    //2
    context = [CIContext contextWithOptions:nil];
    
    //3
    filter = [CIFilter filterWithName:@"CIColorPosterize"];
    [filter setValue:inputImage forKey:@"inputImage"];
    [filter setValue:[NSNumber numberWithFloat:3.0] forKey:@"inputLevels"];
	
	 //4
    outputImage = [filter outputImage];
    imageView.image = [UIImage imageWithCGImage:[context createCGImage:outputImage fromRect:outputImage.extent]];
}
{% endcodeblock %}

1. 指定輸入的圖片。
2. `CIContext`指定要使用CPU或是GPU來處理，這裡使用預設值就可以，所以指定為`nil`。
3. 設定濾鏡效果，後面會介紹如何知道有哪些濾鏡效果及參數的設定。
4. 透過`outputImage`這個方法可以取得濾鏡效果處理後的`CIImage`，最後將處理過後的圖片指定給`imageView`。

**Step 4. 編譯專案**

在一開始可以看到`UIImageView`的原始圖案，點下濾鏡效果按鈕後可以看到圖片改變了！！

{% img /images/CoreImage/3.png 216px %}
{% img /images/CoreImage/4.png 216px %}

就這麼簡單吧！但是一開始困擾我的問題是，我怎麼知道CoreImage有提供哪些濾鏡效果呢？其實這在Document都有詳細的說明，但要如何尋找呢？下面的方法其實我也是看過人家影片教學，才知道有這樣的尋找方式！我覺得非常方便。

首先開啟到`ViewController.m`，並找到`CIFilter`，為了強調要選取它，所以在此將它反白，不然將游標移到`CIFilter`上就可以了。接著選取到右上方的`Show Quick Help Inspector`，在下方可以看到`Core Image Filter Reference`，如下圖：

{% img /images/CoreImage/5.png %}

點選`Core Image Filter Reference`後會跳出一個新的視窗，會顯示`Filter`效果有哪些，如下圖：

{% img /images/CoreImage/6.png %}

接著先找尋到先前使用到的`CIColorPosterize`這個濾鏡效果，點選下去

{% img /images/CoreImage/7.png %}

點選後會看到`CIColorPosterize`這個濾鏡效果的參數有哪些，這個效果的參數只有兩個，分別為`inputImage`與`inputLevels`，除了參數，下方也會有簡單的圖示呈現該濾鏡的效果如何，如下圖：

{% img /images/CoreImage/8.png %}

在這邊可以看到這兩個參數跟`forKey`值很像，這邊的方式就是透過setValue指定參數的值，在指定`forKey`值來指設定參數，所以當濾鏡效果不同時，會有不一樣的`forKey`值，這都需要參考到Document的說明！

{% codeblock lang:objc %}
[filter setValue:inputImage forKey:@"inputImage"];
[filter setValue:[NSNumber numberWithFloat:3.0] forKey:@"inputLevels"];
{% endcodeblock %}



#參考資料#
1. [Beginning Core Image in iOS 5 Tutorial](http://www.raywenderlich.com/5689/beginning-core-image-in-ios-5)
2. [Beginning Core Image in iOS 6](http://www.raywenderlich.com/22167/beginning-core-image-in-ios-6)
3. [Filter4Cam 學習之 Core Image Tutorial](http://itouchs.blogspot.tw/2011/12/filter4cam-core-image-tutorial.html)


