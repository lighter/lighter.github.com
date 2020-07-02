---
layout: post
title: "Core Data-Part2"
date: 2012-06-02 14:36
comments: true
tags: 
- iPhone
- core data
---

上一篇中，僅對於基本的Core Data的流程有個初步的概念，但是光看<code>NSLog</code>訊息還是有點生疏，而且依照我目前的程度，是很少去修改到<code>AppDelegate</code>的部份，所以接著嘗試試著修改<code>Master Detail Application</code>看看。

最後的結果是希望可以在<code>Table View</code>中看到我新增的姓名，點選該姓名可以看到該姓名的電話。

{% img /images/iPhone/coredata2-1.png 216px 390px %}
{% img /images/iPhone/coredata2-2.png 216px 390px %}
{% img /images/iPhone/coredata2-3.png 216px 390px %}

<!-- more -->

**Step 1.**建立<code>Master Detail Application</code>的專案，專案名稱為core data example。

**Step 2.**開啟<code>core_data_example.xcdatamodeld</code>，將原本專案預設的<code>Entity</code>刪除，另外新增一個<code>Phone</code>的<code>Entity</code>，並新增兩個<code>Attributes</code>，name、number，如下圖。

{% img /images/iPhone/coredata2-4.png %}

**Step 3.**新增<code>Managed Object Model</code>，對著專案右鍵<code>New File</code>➔<code>iOS</code>➔<code>Core Data</code>➔<code>NSManagedObjectsubclass</code>，接著會自動新增二個檔案，<code>Phone.m</code>跟<code>Phone.h</code>。

如果沒有直接自動產生，應該會出現以下的畫面，只需要勾選並且下一步即可。

{% img /images/iPhone/coredata2-5.png %}
{% img /images/iPhone/coredata2-6.png %}

**Step 4.**修改<code>MainStoryboard.storyboard</code>。

在<code>Navigation</code>的右方加入一個<code>Bar Button Item</code>，點選新增是會跑到上面的畫面，讓我們新增姓名還有電話，下面則是點選姓名時可以看到該姓名的電話。因此需要新增一個<code>AddDetailViewController</code>來控制該新增畫面。在此就不多贅述。

{% img /images/iPhone/coredata2-7.png %}

**Step 5.**編輯<code>AddDetailViewController</code>。

1. import<code>Phone.h</code>，因為需要透過它來得知<code>Entity</code>的<code>name</code>、<code>number</code>。
2. import<code>AppDelegate.h</code>，因為需要使用它裡面的<code>NSManagedObjectContext</code>，所以在此無需重複定義。
3. 二個<code>UITextField</code>顧名思義就是對應到畫面上二個Text使用的。
4. <code>app</code>就是要用來引用<code>NSManagedObjectContext</code>。
5. <code>save</code>就是在新增的畫面的Navigation右方，按下save按鈕儲存資料。

{% codeblock AddDetailViewController.h lang:objc %}
#import <UIKit/UIKit.h>
#import "Phone.h"
#import "AppDelegate.h"
@interface AddDetailViewController : UIViewController
{
    UITextField *name_;
    UITextField *number_;
}
@property (strong, nonatomic) IBOutlet UITextField *name;
@property (strong, nonatomic) IBOutlet UITextField *number;
@property (retain, nonatomic) AppDelegate *app;

-(IBAction)save:(id)sender;
@end
{% endcodeblock %}

第5行，取得AppDelegate的<code>delegate</code>

第6行，透過app取得AppDelegete的<code>NSManagedObjectContext</code>

第7行，定義要加入的Entity是Phone。

第9行，用一個<code>if</code>檢查欄位的值是否為空白，當不是空白的時候才寫入。

第18~22，當寫入發生錯誤時才會執行。

{% codeblock AddDetailViewController.m lang:objc %}
@synthesize name = name_, number = number_, app;

-(IBAction)save:(id)sender
{
    app = [[UIApplication sharedApplication] delegate];
    NSManagedObjectContext *context = [app managedObjectContext];
    Phone *phone = [NSEntityDescription insertNewObjectForEntityForName:@"Phone" inManagedObjectContext:context];
    
    if (name_.text.length <= 0 || number_.text.length <= 0 ) {
        NSLog(@"Nope");
    }
    else {
        phone.name = name_.text;
        phone.number = [NSNumber numberWithInteger:[number_.text integerValue]];
        [self.navigationController popViewControllerAnimated:YES];
    }
    
    NSError *error;
    
    if (![context save:&error]) {
        NSLog(@"Woops");
    }
}
{% endcodeblock %}

**Step 6.**編輯<code>MasterViewController.m</code>。

這裡僅需要搜尋<code>Event</code>，並且改成<code>Phone</code>取代(因為Entity已經被我換掉了)，還有搜尋<code>timeStamp</code>，並且改成<code>name</code>取代(因為Attribute已經不是timeStamp，我們在<code>Table View</code>中希望呈現姓名，因此改成<code>name</code>)。

最後記得要將<code>viewDidLoad</code>內的方法刪除，僅留下第一行即可(如下)。

{% codeblock MasterViewController.m lang:objc %}
- (void)viewDidLoad
{
    [super viewDidLoad];
	// Do any additional setup after loading the view, typically from a nib.
}
{% endcodeblock %}

**Step 7.**編輯<code>DetailViewController</code>

因為比原本的專案多了一個<code>UILabel</code>元件，所以要多定義一個，並且找到<code>configureView</code>方法，加入第8行設定<code>detailNumber</code>，而<code>valueForKey</code>記得要修改成跟<code>Entity</code>對應的name、number。

{% codeblock DetailViewController.h lang:objc %}
#import <UIKit/UIKit.h>
@interface DetailViewController : UIViewController

@property (strong, nonatomic) id detailItem;

@property (strong, nonatomic) IBOutlet UILabel *detailDescriptionLabel;
@property (strong, nonatomic) IBOutlet UILabel *detailNumber;
@end
{% endcodeblock %}

{% codeblock DetailViewController.m lang:objc %}
@synthesize detailNumber = _detailNumber;

- (void)configureView
{
    // Update the user interface for the detail item.
    if (self.detailItem) {
        self.detailDescriptionLabel.text = [[self.detailItem valueForKey:@"name"] description];
        self.detailNumber.text = [[self.detailItem valueForKey:@"number"] description];
    }
}
{% endcodeblock %}

Step 8.建立關連

最後就是將元件跟<code>IBOutlet</code>、<code>IBAction</code>建立關連，在此就不多做贅述了。

範例下載：[Download here](https://lighter@github.com/lighter/Core-Data-Example.git)




