---
layout: post
title: "JSON資料於TableView與Pull Refresh"
date: 2013-01-10 17:11
comments: true
tags: 
- iPhone
---

我透過JSON的方式來取得資料，並且可以從網路上取得，並且當資料有變動的時候，透過`Table View`向下拉的方式來更新資料。我的JSON資料是透過php程式來撰寫。首先先來建置一個簡單的資料表。如下圖，在`Table View`的部分會呈現名字，當點選名字後會看到該名字的電話號碼。

{% img /images/json/json1.png 216px 390px %}

<!-- more -->

### php程式碼

{% codeblock lang:php %}
<?php
include('db.php');
$query = mysql_query('SELECT * FROM user');
$json_data = array();
while($row = mysql_fetch_assoc($query)) {
  $json_data[] = $row;
}
echo json_encode($json_data);
{% endcodeblock %}

**Step 1.**建立<code>Master Detail Application</code>專案。

建立完成專案後先開始編輯<code>MasterViewController.h</code>。建立一個<code>NSMutableArray</code>型態的<code>json_array</code>物件。

{% codeblock MasterViewController.h lang:objc %}
#import <UIKit/UIKit.h>
@interface MasterViewController : UITableViewController
@property (strong, nonatomic) NSMutableArray *json_array;
@end
{% endcodeblock %}

**Step 2.**編輯<code>MasterViewController.m</code>

在<code>viewDidLoad</code>方法內加入下面的程式碼

{% codeblock MasterViewController.m lang:objc %}
@implementation MasterViewController
@synthesize json_array;
- (void)viewDidLoad
{
    [super viewDidLoad];
    //1
     NSURL *url = [NSURL
                  URLWithString:@"http://XXX.XXX/json.php"];
    //2
    NSData *jsonData=[NSData dataWithContentsOfURL:url];
    NSError *error = nil;
    
    //3
    json_array = [NSJSONSerialization JSONObjectWithData:jsonData options:
            NSJSONReadingMutableContainers error:&error];
}
{% endcodeblock %}

1. 在上面的程式碼一開始定義一個`NSURL`類別的`url`變數，該變數很明顯的可以看到要連接的網頁網址！
2. 將`url`網址的資料存放到`NSData`類別的`jsonData`變數中。
3. 接著將取得到的結果放到`json`陣列中，當中可以看到`options:NSJSONReadingMutableContainers`這段，這意思是將`jsonData`取得的`JSON`資料轉為`NSMutableDictionary`，之後再取得名稱或電話時可以`objecForKey`的方式來取得。

接著修改<code>Table View Cell</code>的回傳個數

{% codeblock MasterViewController.m lang:objc %}
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
    return json_array.count;
}
{% endcodeblock %}

修改<code>Cell</code>顯示的文字，這邊需要透過<code>objectForKey</code>方法指定Key值為。

{% codeblock MasterViewController.m lang:objc %}
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"Cell" forIndexPath:indexPath];
    cell.textLabel.text = [[json_array objectAtIndex:indexPath.row] objectForKey:@"u_name"];
    return cell;
}
{% endcodeblock %}

傳遞參數也需要修改一下。
{% codeblock MasterViewController.m lang:objc %}
- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender
{
    if ([[segue identifier] isEqualToString:@"showDetail"]) {
        NSIndexPath *indexPath = [self.tableView indexPathForSelectedRow];
        [[segue destinationViewController] setDetailItem:[json_array
                                                          objectAtIndex:indexPath.row]];
    }
}
{% endcodeblock %}

將<code>JSON</code>的資料放入<code>Table View</code>中會如下圖所示(資料內容有新增修改過)。

{% img /images/json/json2.png 216px 390px %}
{% img /images/json/json3.png 216px 390px %}

接下來當我<code>MySql</code>有新增資料，或是刪除資料時，我想要同步更新到<code>Table View</code>中，所以我使用了<code>PullToRefresh</code>這個套件。網路上蠻多人都說<code>EGOTableViewPullRefresh</code>這套，但是它似乎不支援<code>ARC</code>，所以我就改用別的了。

這個套件使用上很簡單！首先下載好它的檔案[Here](https://github.com/sonnyparlin/PullToRefresh)，並加入專案內。

{% img /images/json/json4.png %}

接著加入<code>QuartzCore.framework</code>。

{% img /images/json/json5.png %}

修改<code>MasterViewController.h</code>，程式碼如下：
{% codeblock MasterViewController.m lang:objc %}
#import <UIKit/UIKit.h>
#import "PullToRefreshView.h" //1

@interface MasterViewController : UITableViewController<PullToRefreshViewDelegate> //2
@property (strong, nonatomic) NSMutableArray *json_array;
@property (strong, nonatomic) PullToRefreshView *pull; //3
@end
{% endcodeblock %}

回到<code>MasterViewController.m</code>在<code>viewDidLoad</code>將<code>PullToRefresh</code>加入到<code>Table View</code>中。
{% codeblock MasterViewController.m lang:objc %}
@implementation MasterViewController
@synthesize json_array, pull;
- (void)viewDidLoad
{
    [super viewDidLoad];
	// Do any additional setup after loading the view, typically from a nib.
    NSURL *url = [NSURL
                  URLWithString:@"http://lighter.cp22.secserverpros.com/lighter127/JSON/json.php"];
    NSData *jsonData=[NSData dataWithContentsOfURL:url];
    NSError *error = nil;
    json_array = [NSJSONSerialization JSONObjectWithData:jsonData options:
            NSJSONReadingMutableContainers error:&error];

    //pull refresh
    pull = [[PullToRefreshView alloc] initWithScrollView:(UIScrollView *) self.tableView];
    [pull setDelegate:self];
    [self.tableView addSubview:pull];
}
{% endcodeblock %}

當<code>Table View</code>往下拉的時候會呼叫<code>pullToRefreshViewShouldRefresh:</code>方法。這裡我會呼叫另一個自定的方法<code>reloadTableData</code>，而在該方法內我重新再連一次<code>json.php</code>的網頁並更新<code>json_array</code>的內容，最後才呼叫<code>tableView</code>的<code>reloadData</code>。

當資料更新結束後，呼叫<code>finishedLoading</code>方法，結束<code>Pull Refresh</code>的動作。

{% codeblock MasterViewController.m lang:objc %}
#pragma mark - Table View Pull Refresh

- (void)pullToRefreshViewShouldRefresh:(PullToRefreshView *)view;
{
    [self reloadTableData];
}

- (void)reloadTableData
{
    NSURL *url = [NSURL
                  URLWithString:@"http://XXX.XXX/json.php"];
    NSData *jsonData=[NSData dataWithContentsOfURL:url];
    NSError *error = nil;
    json_array = [NSJSONSerialization JSONObjectWithData:jsonData options:
                  NSJSONReadingMutableContainers error:&error];
    [self.tableView reloadData];
    [pull finishedLoading];
}
{% endcodeblock %}

>註：由於向下捲動<code>Table View</code>的速度比我截圖速度快....我就放棄了...

#參考資料
1. [PullToRefresh iOS 5 and ARC Tutorial](http://sonnyparlin.com/2011/12/pulltorefresh-ios-5-and-arc-tutorial/)
2. [PullToRefresh](https://github.com/sonnyparlin/PullToRefresh)
3. [EGOTableViewPullRefresh](https://github.com/enormego/EGOTableViewPullRefresh)