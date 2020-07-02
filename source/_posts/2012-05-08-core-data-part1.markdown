---
layout: post
title: "Core Data-Part1"
date: 2012-05-08 10:52
comments: true
tags: 
- iPhone
---
<code>Core Data</code>主要分成三個部分
<ol>
<li>Managed Object Context</li>
<li>Persistent Store Coordinator</li>
<li>Persistent Object Store</li>
</ol>

<code>Managed Object Context</code>這類別記載了我們的App在記憶體中所有的<code>Entity</code>，當你要求Core Data載入物件時，你必須先向<code>Managed Object Context</code>提出要求。

...假如不存在記憶體中的話，它會向<code>Persistent Store Coordinator</code>發出請求，試著嘗試尋找它。

<code>Persistent Store Coordinator</code>的任務是追蹤<code>Persistent Object Store</code>，而<code>Persistent Object Store</code>實際知道如何讀寫資料。

<code>Managed Object Model</code>則是用來處理資料的，這些元件都知道要如何處理資料。

整個流程大致上如下圖：

{% img /images/iPhone/coredatas1.jpeg %}

<!-- more -->

**Step 1.**建立<code>Empty Application</code>，專案名稱為core data example

{% img /images/iPhone/coredatas2.jpeg %}

記得要將<code>Use Core Data</code>打勾。

{% img /images/iPhone/coredatas3.jpeg %}

**Step 2.** 編輯core_data_sample.xcdatamodeld

首先開啟<code>core_data_sample.xcdatamodeld</code>這個檔案，點選中間下方的<code>Add Entity</code>，並輸入<code>Phone</code>(註：第一個字要大寫)，接著在右邊可以找到<code>Attributes</code>，點選<code>＋</code>加號，加入兩個屬性分別為<code>name</code>、<code>number</code>，其型態分別為<code>String</code>
、<code>Integer 32</code>。

{% img /images/iPhone/coredatas4.jpeg %}

**Step 3.**建立<code>Phone.m、Phone.h</code>

點選專案資料夾，右鍵➔New File。

{% img /images/iPhone/coredatas5.jpeg %}

選擇iOS➔Core Data➔NSManagedObject subclass，接著都直接下一步。

{% img /images/iPhone/coredatas6.jpeg %}

接著可以在專案的資料夾內會出現<code>Phone.m、Phone.h</code>

{% img /images/iPhone/coredatas7.jpeg %}

在這兩個檔案可看到裡面的變數都是跟xcdatamodeld內的有相關連的

{% codeblock Phone.h lang:objc %}
#import <Foundation/Foundation.h>
#import <CoreData/CoreData.h>

@interface Phone : NSManagedObject

@property (nonatomic, retain)  NSString * name;
@property (nonatomic, retain) NSNumber * number;

@end
{% endcodeblock %}

這邊的<code>@dynamic</code>主要是告知 compiler 不要產生<code>setter、getter</code>，而是由程式本身來實作甚至是直到 Runtime 時以 Dynamic Loading 方式連結。

{% codeblock Phone.m lang:objc %}
#import "Phone.h"

@implementation Phone

@dynamic name;
@dynamic number;

@end
{% endcodeblock %}

**Step .4**編輯AppDelegate.m

在該檔案內自行新增一個<code>creatData</code>方法
{% codeblock AppDelegate.m lang:objc %}
-(void)creatData{
    NSManagedObjectContext *context = [self managedObjectContext];
    
    Phone *detail = [NSEntityDescription insertNewObjectForEntityForName:@"Phone" inManagedObjectContext:context];
    detail.name = @"Mary";
    detail.number = [NSNumber numberWithInt:323232123];

    NSError *error;
    if(![context save:&error]){
        NSLog(@"error");
    }
    
    //撈取資料
    NSFetchRequest *request = [[NSFetchRequest alloc] init];
    NSEntityDescription *entity = [NSEntityDescription entityForName:@"Phone" inManagedObjectContext:context];
    [request setEntity:entity];
    
    NSArray *array = [context executeFetchRequest:request error:&error];
    for (Phone *pho in array) {
        NSLog(@"Name: %@", pho.name);
        NSLog(@"Number: %@", pho.number);
    }
}
{% endcodeblock %}

{% codeblock AppDelegate.m lang:objc %}
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    [self creatData];   //加入這行
    self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
    self.window.backgroundColor = [UIColor whiteColor];
    [self.window makeKeyAndVisible];
    return YES;
}
{% endcodeblock %}

最後編譯後，可以在Debug的地方看到<code>NSLog</code>的訊息。

參考資料：

1. [iPhone Core Data 1 - Intro](http://www.youtube.com/watch?v=QBrkavVJsw0&feature=relmfu)
2. [@synthesize vs @dynamic, what are the differences?](http://stackoverflow.com/questions/1160498/synthesize-vs-dynamic-what-are-the-differences)
3. [The Objective-C Programming Guide 入門筆記](http://rintarou.dyndns.org/2011/07/12/the-objective-c-programming-guide-%E5%85%A5%E9%96%80%E7%AD%86%E8%A8%98/)
4. [iPhone开发之CoreData(基础篇)](http://blog.csdn.net/zyc851224/article/details/7387805)
