---
layout: post
title: "Segmented Controller"
date: 2012-05-01 10:07
comments: true
tags: 
- iPhone
---
<code>Segmented Controller</code>最典型的應用就是在行事曆的日、月、週切換，呈現的資料會根據日月週來分別呈現，下面使用<code>Segmented Controller</code>來切換網頁呈現網站內容。

以下使用3個<code>Segments</code>來做Google、Yahoo、Youtube的切換。

{% img /images/iPhone/segmentedController1.jpeg %}
{% img /images/iPhone/segmentedController2.jpeg %}
{% img /images/iPhone/segmentedController3.jpeg %}

<!-- more -->

**Step 1.**建立<code>SingleView</code>專案，專案命名為<code>Segmented Contrller</code>。

**Step 2.**編輯MainStoryboard.storyboard。

該專案會使用到的元件有：
<ol>
	<li><code>Toolbar</code></li>
	<li><code>Web View</code></li>
	<li><code>TextField</code></li>
	<li><code>Segmented Controller</code></li>
	<li><code>Activity Indicator View</code></li>
</ol>

<code>Activity Indicator View</code>的設定如同之前[Web View](http://lighter.github.com/blog/2012/04/24/web-view/)一樣。

<code>Segmented Controlle</code>在預設中只有兩個選項，要增加選項可在<code>Attributes inspector</code>的<code>Segments</code>設定成你要的數量，接著在下方<code>Segment</code>可以設定<code>Segment 0</code>的<code>Title</code>，根據你選擇到第幾個<code>Segment</code>可以個別去設定<code>Title</code>。

{% img /images/iPhone/segmentedController4.jpeg %}
{% img /images/iPhone/segmentedController5.jpeg %}
{% img /images/iPhone/segmentedController6.jpeg %}

**Step 3.**編輯ViewController.h。

定義的變數如同[Web View](http://lighter.github.com/blog/2012/04/24/web-view/)內的，這裡多一個<code>UISegmentedControl</code>，<code>hitSegmented</code>是<code>Segmented Controller</code>的觸發事件。

{% codeblock lang:objc %}
@interface ViewController : UIViewController<UIWebViewDelegate>
{
    UIWebView *webView_;
    UITextField *urlTextField_;
    UIActivityIndicatorView *activityView_;
    
    UISegmentedControl *segmented_;
}

@property (strong, nonatomic) IBOutlet UIWebView *webView;
@property (strong, nonatomic) IBOutlet UITextField *urlTextField;
@property (strong, nonatomic) IBOutlet UIActivityIndicatorView *activityView;

@property (strong, nonatomic) IBOutlet UISegmentedControl *segmented;

-(IBAction)hitReturn:(id)sender; //點選鍵盤的return的觸發事件

-(IBAction)hitSegmented:(id)sender;//點選Segmented Controller的事件
{% endcodeblock %}

**Step 4.**編輯ViewController.m。

載入網頁的過程也如同[Web View](http://lighter.github.com/blog/2012/04/24/web-view/)的說明，因此在<code>viewDidLoad</code>、<code>hitReturn</code>方法都與[Web View](http://lighter.github.com/blog/2012/04/24/web-view/)相同；<code>UIActivityIndicatorView</code>的操作也一樣僅需將[Web View](http://lighter.github.com/blog/2012/04/24/web-view/)中的<code>webViewDidStartLoad</code>、<code>webViewDidFinishLoad</code>複製過來即可。

這裡則是多了一個<code>hitSegmented</code>的方法，載入網頁的過程都是使用相同的方法。
{% codeblock lang:objc %}
-(IBAction)hitSegmented:(id)sender
{
    NSString *urlString;	//網頁字串
    
    NSInteger segmentedNum;	//Segmented的選項
    
    segmentedNum = segmented_.selectedSegmentIndex;	//取得點選的Segmented Index
    
    switch (segmentedNum) {
        case 0:
            urlString = @"http://www.google.com";
            break;
        case 1:
            urlString = @"http://www.yahoo.com.tw";
            break;
        case 2:
            urlString = @"http://www.youtube.com.tw";
            break;
        default:
            break;
    }
    self.urlTextField.text = urlString;
    NSURL *url = [NSURL URLWithString:urlString];
    NSURLRequest *urlRequest = [NSURLRequest requestWithURL:url];
    [self.webView loadRequest:urlRequest];
}
{% endcodeblock %}

**Step 5.**建立關連。

{% img /images/iPhone/segmentedController7.jpeg %}

