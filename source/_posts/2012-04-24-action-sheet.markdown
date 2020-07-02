---
layout: post
title: "Action Sheet"
date: 2012-04-24 10:29
comments: true
tags: 
- iPhone
---
UIActionSheet是一個由下往上出現的視窗，通常彈出一些按鈕選項讓使用者選擇要執行的方式為何！使用一個簡單的範例來介紹它的使用方法。

在iPhone畫面中加入一個Round Rect Button，當Button按下時，ActionSheet則會彈出。

{% img center /images/iPhone/actionSheets1.jpeg %}

**Step 1.**建立一個<code>SingleView</code>的專案，專案名稱為Action Sheet

**Step 2.**開啟MainStoryboard.storyboard，加入一個<code>Round Rect Button</code>的元件，並且設定該元件的Title為Show Action Sheet

<!-- more -->

{% img center /images/iPhone/actionSheets2.jpeg %}

**Step 3.**編輯ViewController.h

1.加入<code>UIActionSheetDelegate</code>的協定。

2.定義showActionSheet為Button的觸發事件。

{% codeblock lang:objc %}
@interface ViewController : UIViewController<UIActionSheetDelegate>
-(IBAction)showActionSheet:(id)sender;
{% endcodeblock %}

**Step 4.**編輯ViewController.m

1.在showActionSheet:內定義一個<code>UIActionSheet</code>的actoinSheet變數，當中的參數如以下設定；特別的是當你有其它選項想讓使用者選擇時，<code>otherButtonTitles:</code>這個參數設定後面連續使用<code>@""</code>連接。

2.<code>actionSheetStyle</code>設定ActionSheet的Style。

3.記得設定<code>showInVie</code>將ActionSheet顯示在畫面上。

{% codeblock lang:objc %}

-(IBAction)showActionSheet:(id)sender
{
    UIActionSheet *actionSheet = [[UIActionSheet alloc] initWithTitle:@"Share Save"
    			delegate:self
				cancelButtonTitle:@"Cancel" 
			    destructiveButtonTitle:@"Save" 
			    otherButtonTitles:@"Email", @"Facebook", @"Twitter", @"Plurk", nil];

    actionSheet.actionSheetStyle = UIActionSheetStyleDefault;
    [actionSheet showInView:self.view];
}
{% endcodeblock %}

**Step 5.**判斷按下了哪個按鈕！！

可以透過<code>actionSheet:clickedButtonAtIndex:</code>這個方法來協助得知按下了是哪一個按鈕。

在ViewController.m檔加入這個方法，並使用簡單的<code>switch</code>來判斷是哪一個按鈕被按下了，這邊使用一個簡單的<code>NSLog</code>訊息來告知你按下了哪一個按鈕。
{% codeblock lang:objc %}
-(void)actionSheet:(UIActionSheet *)actionSheet clickedButtonAtIndex:(NSInteger)buttonIndex
{
    switch (buttonIndex) {
        case 0:
            NSLog(@"Save");
            break;
        case 1:
            NSLog(@"Email");
            break;
        case 2:
            NSLog(@"Facebook");
            break;
        case 3:
            NSLog(@"Twitter");
            break;
        case 4:
            NSLog(@"Plurk");
            break;
        default:
            break;
    }
}
{% endcodeblock %}

**Step 6.**建立關連

建立<code>showActionSheet</code>與<code>Round Rect Button</code>的關聯

{% img center /images/iPhone/actionSheets3.jpeg %}