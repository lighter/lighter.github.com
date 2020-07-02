---
layout: post
title: "Scroll View"
date: 2012-04-29 17:27
comments: true
tags: 
- iPhone
---
當iPhone的畫面要塞進很多內容時，可以利用<code>Scroll View</code>來捲動畫面瀏覽頁面，例如當畫面需要呈現很多張圖片的時候，一個<code>View</code>只能呈現一張<code>UIImage</code>，因此可以利用<code>Scroll View</code>來呈現。

最後的結果如下圖：(將圖片向右排列，手指將畫面往左拖曳，即可看到其他的圖片)。

{% img /images/iPhone/scrollViews1.jpeg %}
{% img /images/iPhone/scrollViews2.jpeg %}
{% img /images/iPhone/scrollViews3.jpeg %}

<!-- more -->

**Step 1.**建立<code>Single View</code>專案，專案命名為<code>Scroll View</code>。

**Step 2.**編輯MainStoryboard.storyboard。

加入一個<code>Scroll View</code>到View中。

{% img /images/iPhone/scrollViews4.jpeg %}

**Step 3.**編輯ViewController.h。

<ol>
	<li>加入<code>UIScrollViewDelegate</code>的協定。</li>
	<li>定義一個<code>UIScrollView</code>的變數<code>scrollView_</code>。</li>
</ol>

{% codeblock lang:objc %}
@interface ViewController : UIViewController<UIScrollViewDelegate>
{
    UIScrollView *scrollView_;
}
@property(strong, nonatomic) IBOutlet UIScrollView *scrollView;
@end
{% endcodeblock %}

**Step 4.**編輯ViewController.m。

<ol>
	<li>首先要在專案內加入幾張圖片檔案。</li>
	<li>第6行，定義一個<code>NSMutableArray</code>的陣列變數<code>photoName</code>記錄圖片的檔案名稱。</li>
	<li>第7行，再定義一個<code>NSMutableArray</code>的陣列變數<code>photos</code>，一個一個使用<code>objectAtIndex</code>定義<code>UIImage</code>。</li>
	<li>第14行，<code>i</code>變數用調整<code>UIImage View</code>的寬度。</li>
	<li>第15行，使用for迴圈，一個一個定義<code>UIImage</code>。</li>
	<li>第16行，使用<code>UIImageView</code>來放置要顯示的圖片<code>UIImage</code>。</li>
	<li>第17行，定義<code>UIImageView</code>的模式為<code>UIViewContentModeScaleToFill</code>自動縮放填滿畫面。</li>
	<li>第18行，定義<code>clipsToBounds</code>，當值為<code>YES</code>時，畫面會自動切除影像超出Bounds的部份</li>
	<li>第20行，使用<code>CGRectMake</code>定義<code>UIImageView</code>的<code>(x座標, y座標, 寬, 高)</code>。</li>
	<li>第22行，將<code>imageView</code>加入<code>scrollView</code>。</li>
	<li>第25行，定義<code>scrollView</code>的頁面大小<code>CGSizeMake(寬, 高)</code>。</li>
	<li>第26行，定義<code>scrollView_</code>的<code>delegate</code>為<code>self</code>。</li>
</ol>

{% codeblock lang:objc %}
@synthesize scrollView = scrollView_;
- (void)viewDidLoad
{
    [super viewDidLoad];
    
    NSMutableArray *photoName = [NSMutableArray arrayWithObjects:@"a2.jpg", @"a3.jpg", @"a4.jpg", @"a5.jpg", nil];
    NSMutableArray *photos = [NSMutableArray arrayWithObjects:
                                [UIImage imageNamed:[photoName objectAtIndex:0]],
                                [UIImage imageNamed:[photoName objectAtIndex:1]],
                                [UIImage imageNamed:[photoName objectAtIndex:2]],
                                [UIImage imageNamed:[photoName objectAtIndex:3]],
                                nil];
    
    int i = 0;
    for(UIImage *image in photos) {
        UIImageView *imageView = [[UIImageView alloc] initWithImage:image];
        imageView.contentMode = UIViewContentModeScaleToFill;
        imageView.clipsToBounds = YES;
        
        imageView.frame = CGRectMake(320*i++,0 ,320, 460);
        
        [self.scrollView addSubview:imageView];
    }
    
    self.scrollView.contentSize = CGSizeMake(320*i, 460);
    scrollView_.delegate = self;
}
{% endcodeblock %}

**Step 5.**建立關連

{% img /images/iPhone/scrollViews5.jpeg %}