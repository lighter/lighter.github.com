---
layout: post
title: "將XCODE的程式發佈到iOS裝置"
date: 2012-07-21 15:52
comments: true
tags: 
- iPhone
---

以下是我使用的作業環境

* Mac Lion 10.7.4
* XCODE 4.3.3
* iOS5.1 裝置為 iPhone 4

<!-- more -->

**Step 1.製作假驗證**

首先在Finder的地方，點選前往 -> 工具程式。

{% img /images/xcodeToiPhone/xcodeToiPhone1.png %}

開啟鑰匙圈存取。

{% img /images/xcodeToiPhone/xcodeToiPhone2.png %}

選取鑰匙圈存取 -> 憑證輔助程式 -> 製作憑證。

{% img /images/xcodeToiPhone/xcodeToiPhone3.png %}

* 名稱：fake
* 識別身分類型：自簽根
* 憑證類型：代碼簽名
*覆蓋預設值(勾選)

繼續之後會跳出一個對話視窗一樣選取繼續。

{% img /images/xcodeToiPhone/xcodeToiPhone4.png %}

* 序號：121212
* 有效時間：999

接著一直繼續到最後。

{% img /images/xcodeToiPhone/xcodeToiPhone5.png %}

Step 2.修改XCODE讓他可以抓取到我們的製作的假驗證。

開啟應用程式資料夾，找到XCODE，右鍵 -> 顯示套件內容。

{% img /images/xcodeToiPhone/xcodeToiPhone6.png %}

依照Contents/Developer/Platforms/iPhoneOS.platform的路徑尋找到<code>Info.plist</code>。

{% img /images/xcodeToiPhone/xcodeToiPhone7.png %}

1. 將<code>Info.plist</code>複製到桌面上開啟，使用<code>cmd+f</code>尋找<code>CODE_SIGN_CONTEST_CLASS</code>，將原本的預設值改為<code>XCCodeSignContext</code>(共有兩個地方要修改)。
2. <code>RuntimeRequirements -> Classes -> item 0</code>的預設值也改為<code>XCCodeSignContext</code>。

**ps.這樣總共有三個地方要修改為<code>XCCodeSignContext</code>。**

修改完後將該檔案貼回原本的地方並且取代掉。

Step 3.修改專案。

接著在你的<code>XCODE</code>專案，點選專案的設定檔<code>PROJECT</code> -> <code>Build Settings</code> -> <code>Code Signing</code> -> <code>Code Signing Identity</code>。

{% img /images/xcodeToiPhone/xcodeToiPhone8.png %}

將裡面的憑證選取您剛新增的金鑰名稱，這裡使用之前已經建立好的<code>fake code sign</code>，所以會跟前面有所不同！！

{% img /images/xcodeToiPhone/xcodeToiPhone9.png %}

建立完之後在編譯專案的右邊，選取我們要發佈的裝置，記得裝置必須先與電腦連接。接著就可以編譯發佈到實體裝置上了。

{% img /images/xcodeToiPhone/xcodeToiPhone12.png %}

發佈的時候如果點選App後馬上閃退的話，這時修改一下<code>Scheme</code>。

{% img /images/xcodeToiPhone/xcodeToiPhone10.png %}

選擇第二個<code>Run XXX.app</code> -> <code>Info</code> -> <code>Debugger</code> -> 選取<code>GDB</code>，最後按下OK，再次重新編譯專案發佈到實體裝置，應該就可以解決閃退的問題了。

{% img /images/xcodeToiPhone/xcodeToiPhone11.png %}

### 參考資料

1. [如何將Xcode的程式發布到iOS裝置(免年費)](http://tw.myblog.yahoo.com/pcman-128/article?mid=850&prev=-2&next=-2&page=1&sc=1#yartcmt)
2. [iOS學習_Error launching remote program: failed to get the task for process](http://wangshifuola.blogspot.tw/2012/04/ioserror-launching-remote-program.html)

