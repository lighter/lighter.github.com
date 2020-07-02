---
layout: post
title: "Core Data Relationship"
date: 2012-11-03 17:40
comments: true
tags: 
- iPhone
- core data
---
`Core Data`在前面的範例中並沒有使用到`Relationship`，如果你的資料結構是需要使用到兩個資料表時，這時可以透過`Relationship`，而且操作起來…我覺得方便很多！假如有表1跟表2是有相關連了，以往我在操作MySQL的時候，會先取得表1的Key值，在拿表1的Key值到表2查詢，這樣等於我要做兩次的SQL查詢(可能有更好的作法吧，我沒有特別去研究...)，所以當我用`Core Data`的時候，覺得這是一個很方便又快速的作法。

接下來我們建立的資料表如下圖。

{% img /images/CoreDataRelationship/1.png %}

<!-- more -->

`UserInfo`只用來記錄名字，`UserDetail`則用來記錄該名字的電話、地址、E-mail，而下面`Relationship`分別為`detail`跟`info`。

**Step 1.編輯`xcdatamodeled`檔案。**

`Entity`、`Attribute`、`Relationship`的新增就跳過了；這邊需注意的是`Relationship`設定，除了`Destination`還須設定`Inverse`，如下圖。

{% img /images/CoreDataRelationship/2.png %}

{% img /images/CoreDataRelationship/3.png %}

**Step 2.新增`Model`檔案。**

點選`UserInfo`，在對著專案`右鍵`/`New File`/`iOS`/`Core Data`/`NSManagedObject subclass`/`Next`，接著自動就會產生UserInfo.h與.m的檔案；`UserDetail`一樣的操作，完成後應該會有四個檔案，分別為UserInfo與UserDetail的.h與.m檔案。

{% img /images/CoreDataRelationship/5.png %}

**Step 3.編輯`MainStoryboard.storyboard`檔案。**

完成如下圖，新增的時候要輸入姓名、email、電話、地址；顯示則是顯示這四個值。

{% img /images/CoreDataRelationship/4.png %}

**Step 4.修改`MasterViewController.m`。**

記得要把`viewDidLoad`方法內的執行內容清除，僅需保留`[super viewDidLoad];`。

找到下列三段程式碼，分別修改成對應的`Entity`名稱與`Key`值。

{% codeblock MasterViewController.m lang:objc %}
//1
NSEntityDescription *entity = [NSEntityDescription entityForName:@"UserInfo" inManagedObjectContext:self.managedObjectContext];

//2
NSSortDescriptor *sortDescriptor = [[NSSortDescriptor alloc] initWithKey:@"name" ascending:NO];

//3
cell.textLabel.text = [[object valueForKey:@"name"] description];
{% endcodeblock %}

**Step 5.新增`AddViewController`檔案。**

新增一個`UIViewController`的檔案，class名稱為`AddViewController`。
{% img /images/CoreDataRelationship/6.png %}

指定該檔案為新增資料頁面的`Class`。
{% img /images/CoreDataRelationship/7.png %}

**Step 6.編輯`AddViewController.h`。**

編輯程式如下
{% codeblock AddViewController.h lang:objc %}
#import <UIKit/UIKit.h>
#import "AppDelegate.h"
#import "UserInfo.h"
#import "UserDetail.h"

@interface AddViewController : UIViewController
@property (strong, nonatomic) IBOutlet UITextField *name;
@property (strong, nonatomic) IBOutlet UITextField *phone;
@property (strong, nonatomic) IBOutlet UITextField *email;
@property (strong, nonatomic) IBOutlet UITextField *address;
- (IBAction)save:(id)sender;

@property (strong, nonatomic) AppDelegate *app;
@end
{% endcodeblock %}

分別定義四個`UITextField`的`property`，一個`IBAtcion`的save:方法；`AppDelegate`如同之前說的是要使用`AppDelegate`內的`managedObjectContext`。

**Step 7.編輯AddViewController.m。**

編輯程式如下
{% codeblock AddViewController.m lang:objc %}
@synthesize name, email, phone, address, app;

- (IBAction)save:(id)sender {
  app = [[UIApplication sharedApplication] delegate];
  NSManagedObjectContext *context = [app managedObjectContext];

  UserInfo *info = [NSEntityDescription insertNewObjectForEntityForName:@"UserInfo" inManagedObjectContext:context];
  info.name = name.text;

  UserDetail *detail = [NSEntityDescription insertNewObjectForEntityForName:@"UserDetail" inManagedObjectContext:context];
  detail.address = address.text;
  detail.email = email.text;
  detail.phone = phone.text;
  info.detail = detail; //關連

  NSError *error;
  if (![context save:&error]) {
    NSLog(@"Save Error : %@", [error localizedDescription]);
  } else {
    [self.navigationController popViewControllerAnimated:YES];
  }
}
{% endcodeblock %}

補上`synthesize`！。這邊可以看到定義一個`UserInfo`與`UserDetail`的類別變數，分別將TextField的值指定給個別對應的`Attribute`，這邊可以注意到的是第14行，是透過這樣方式指定兩個表格的對應關連！

**Step 8.修改`MasterViewController.m`。**

找到`prepareForSegue`方法，修改的程式如下
{% codeblock MasterViewController.m lang:objc %}
- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender
{
    if ([[segue identifier] isEqualToString:@"showDetail"]) {
        NSIndexPath *indexPath = [self.tableView indexPathForSelectedRow];
        
        //修改成UserInfo
        UserInfo *object = [[self fetchedResultsController] objectAtIndexPath:indexPath];
        [[segue destinationViewController] setDetailItem:object];
    }
}
{% endcodeblock %}

**Step 9.修改`DetailViewController.h`。**

編輯程式碼如下

{% codeblock DetailViewController.h lang:objc %}
#import <UIKit/UIKit.h>
#import "UserInfo.h"

@interface DetailViewController : UIViewController

//修改detailItem的形態為UserInfo
@property (strong, nonatomic) UserInfo *detailItem;

@property (weak, nonatomic) IBOutlet UILabel *detailDescriptionLabel;

//多定義3個UILabel元件
@property (strong, nonatomic) IBOutlet UILabel *detailEmail;
@property (strong, nonatomic) IBOutlet UILabel *detailPhone;
@property (strong, nonatomic) IBOutlet UILabel *detailAddress;

@end
{% endcodeblock %}

**Step 10.修改`DetailViewController.m`。**

找到`configureView`方法，修改程式碼如下

{% codeblock DetailViewController.m lang:objc %}
- (void)configureView
{
    // Update the user interface for the detail item.

  if (self.detailItem) {
    self.detailDescriptionLabel.text = [[self.detailItem valueForKey:@"name"] description];
    self.detailEmail.text = self.detailItem.detail.email;
    self.detailPhone.text = self.detailItem.detail.phone;
    self.detailAddress.text = self.detailItem.detail.address;
  }
}
{% endcodeblock %}

如果上述的程式碼有問題的話，不妨檢查一下`UserInfo.h`的檔案，這邊我發現到自動產生的檔案，他產生的形態並沒有如預期的型態，`detail`的形態應該是`UserDeatail`，程式碼如下

{% codeblock UserInfo.h lang:objc %}
#import <Foundation/Foundation.h>
#import <CoreData/CoreData.h>
#import "UserDetail.h"

@interface UserInfo : NSManagedObject

@property (nonatomic, retain) NSString * name;
@property (nonatomic, retain) UserDetail *detail;

@end
{% endcodeblock %}

**Step 11.編譯專案。**

最後編譯專案時，記得檢查一下IBOutlet的關聯是否都建立了，如果沒問題，就可以開始新增資料與檢視資料了。

{% img /images/CoreDataRelationship/8.png 216px 390px %}
{% img /images/CoreDataRelationship/9.png 216px 390px %}
{% img /images/CoreDataRelationship/10.png 216px 390px %}