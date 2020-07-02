---
layout: post
title: "iOS With Dropbox"
date: 2012-10-28 00:38
comments: true
tags: 
- iPhone
---
{% img /images/iPhoneWithDropbox/1.png %}

iPhone上已經有iCloud可以用來同步及備份資料了，但iCloud有空間的限制，而有在使用Dropbox的人，如果有參加一些特殊的活動或者是學生的身份，都可以多取得到一些額外的空間容量(或者是有邀請許多朋友加入也都可以額外增加空間)。雖然Dropbox官方的說明已經很清楚了，但是我還是記錄一下，免得之後要用忘記....

<!-- more -->

**Step 1.建立Single View Application專案**

**Step 2.編輯MainStroyboard.storyboard檔案**

這邊我使用三個<code>Round Rect Button</code>，分別用來建立檔案、檢驗檔案是否存在與上傳檔案

{% img /images/iPhoneWithDropbox/2.png %}


**Step 3.建置Dropbox App**

到Dropbox開發者[網站](https://www.dropbox.com/developers)，建立一個新的App。建立完成後可以看到Dropbox根據這個App會給你一個<code>App key</code>與<code>App secret</code>，到時會需要這兩個值。

下載Dropbox的Framework，並且加入到Xcode專案；還有要加入<code>Security.framework</code>與<code>QuartzCore.framework</code>到專案中。

{% img /images/iPhoneWithDropbox/3.png %}

**Step 4.編輯AppDelegate.m**

在<code>AppDelegate.m</code>中加入下列程式碼

{% codeblock AppDelegate.m lang:objc %}
#import <DropboxSDK/DropboxSDK.h>
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
	DBSession* dbSession =
		[[DBSession alloc]
			initWithAppKey:@"APP_KEY"
			appSecret:@"APP_SECRET"
			root:ACCESS_TYPE]; // either kDBRootAppFolder or kDBRootDropbox
	[DBSession setSharedSession:dbSession];
	return YES;
}

- (BOOL)application:(UIApplication *)application handleOpenURL:(NSURL *)url {
  if ([[DBSession sharedSession] handleOpenURL:url]) {
    if ([[DBSession sharedSession] isLinked]) {
      NSLog(@"App linked successfully!");
      // At this point you can start making API calls
    }
    return YES;
  }
  // Add whatever other url handling code your app requires here
  return NO;
}
{% endcodeblock %}

**Step 5.編輯<code>專案名稱-Info.plist</code>檔案

使用<code>open source</code>開啟<code>專案名稱-Info.plist</code>，找到第一個<code><dict></code>在下方加入這段：

{% codeblock 專案名稱-Info.plist lang:objc %}
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>db-APP_KEY</string>
        </array>
    </dict>
</array>
{% endcodeblock %}

**Step 6.編輯ViewController

首先在<code>ViewController.h</code>程式碼如下：

{% codeblock ViewController.h lang:objc %}
#import <UIKit/UIKit.h>
#import <DropboxSDK/DropboxSDK.h>

@interface ViewController : UIViewController<DBRestClientDelegate>
{
  DBRestClient *restClient;
}
- (IBAction)loginDropbox:(id)sender;
- (IBAction)upload:(id)sender;
- (IBAction)creatFile:(id)sender;

@end
{% endcodeblock %}

接著在<code>ViewController.m</code>撰寫三個方法的實作內容，並且撰寫一個<code>restClient</code>的getter方法。

{% codeblock ViewController.m lang:objc %}
- (DBRestClient *)restClient {
  if (!restClient) {
    restClient =
    [[DBRestClient alloc] initWithSession:[DBSession sharedSession]];
    restClient.delegate = self;
  }
  return restClient;
}

- (IBAction)loginDropbox:(id)sender {

  if (![[DBSession sharedSession] isLinked]) {
    [[DBSession sharedSession] linkFromController:self];
  } else {
    NSLog(@"linked");
  }
}

- (IBAction)upload:(id)sender {

  NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
  NSString *path = [[paths objectAtIndex:0]stringByAppendingPathComponent:@"file1.txt"];;
  NSString *pathString = @"/";
  [self.restClient uploadFile:@"file1.txt" toPath:pathString fromPath:path];

}

- (IBAction)creatFile:(id)sender {
  NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
  NSString *path = [[paths objectAtIndex:0]stringByAppendingPathComponent:@"file1.txt"];;

  // String to write
  NSString *str = @"iPhone Developer Tips\nhttp://iPhoneDevelopTips,com";
  NSError *error;
  // Write the file
  [str writeToFile:path atomically:YES encoding:NSUTF8StringEncoding error:&error];

  NSFileManager *filemgr;

  filemgr = [NSFileManager defaultManager];

  if ([filemgr fileExistsAtPath: path ] == YES)
    NSLog (@"File exists");
  else
    NSLog (@"File not found");

}
{% endcodeblock %}

**Step 7.編譯專案**

點選<code>link dropbox</code>按鈕，跟Dropbox做連接；在點選<code>create & check</code>按鈕建立檔案與確認檔案是否存在；最後才點選<code>upload</code>按鈕，將建立的檔案上傳到Dropbox。