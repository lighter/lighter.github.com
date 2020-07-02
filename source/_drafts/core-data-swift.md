title: core data(swift)
date: 2015/8/9 22:07:00
tags:
- swift
- core data
---

建立`Single View Application`。

{% img /images/core_data_swift/core_data_swift_1.jpg %}

記得勾選`Use Core Data`

{% img /images/core_data_swift/core_data_swift_2.jpg %}

<!-- more -->

開啟`Main.storyboard`，修改一下ViewController呈現的尺寸，其實這樣的修改只是在開發上看起來跟實際上的iPhone尺寸是比較接近的。

{% img /images/core_data_swift/core_data_swift_3.jpg %}

加入`Navigation Controller`

{% img /images/core_data_swift/core_data_swift_4.jpg %}

完成後可以看到原本的ViewController前面多了一個`Navigation Controller`。

{% img /images/core_data_swift/core_data_swift_5.jpg %}

在`ViewController`加入`Table View`。

{% img /images/core_data_swift/core_data_swift_6.jpg %}

加入`Bar Button Item`，並將名稱修改為`新增`。

{% img /images/core_data_swift/core_data_swift_7.jpg %}

點選`Table View`，`control + 拖拉滑鼠`建立`Outlet`，名稱為`tableView`。

{% img /images/core_data_swift/core_data_swift_8.jpg %}

按鈕的方式也是一樣，只是這邊是新增`Action`，方法名稱為`add`

{% img /images/core_data_swift/core_data_swift_9.jpg %}

加入`Table View`的`dataSource`為`View Controller`。

{% img /images/core_data_swift/core_data_swift_10.jpg %}

上述完成後的code

```
import UIKit

class ViewController: UIViewController {

    @IBOutlet weak var tableView: UITableView!

    @IBAction func add(sender: AnyObject) {

    }

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
}
```

1. 加入`lists`陣列變數用來儲存所以有的list資料

```
@IBOutlet weak var tableView: UITableView!
var lists = [String]() // 1
```

1. 設定Title名稱
2. 註冊一個`UITableViewCell`

```
override func viewDidLoad() {
    super.viewDidLoad()

    title = "第一次CoreData" // 1
    tableView.registerClass(UITableViewCell.self, forCellReuseIdentifier: "cell") // 2
}
```
加入`UITableViewDataSource` protocol

```
class ViewController: UIViewController,  UITableViewDataSource {
```

加入下列code。

```
  // table view rows的數量
  func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
      return lists.count
  }

  // 各個row的文字
  func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
      let cell = tableView.dequeueReusableCellWithIdentifier("cell") as! UITableViewCell
      cell.textLabel!.text = lists[indexPath.row]
      return cell
  }
  ```
  
現在試著編譯，在模擬器跑跑看，是否正常。現在資料是存放在陣列裡面，當App整個結束，下次再打開，就看不到你上次的資料啦。這時就是需要`Core Data`來記錄我們的資料，讓我下次開啟時可以看到上次輸入的資料。

----------

### Continue

一開始在建立專案時因為有勾選了`Use core data`，在專案中也已經幫你建立好`.xcdatamodeld`檔案。點選他，可以看到不一樣的設定介面。

{% img /images/core_data_swift/core_data_swift_11.jpg %}

點選`Add Entity`，名稱改為`Somthing`(ps: 第一個字要大寫)。

{% img /images/core_data_swift/core_data_swift_12.jpg %}

新增`Attribute`，名稱為`detail`，型態為`String`。

{% img /images/core_data_swift/core_data_swift_13.jpg %}

編輯`ViewController.swift`，加入`CoreData`

```
import UIKit
import CoreData // import core data
```

將`lists`的型態修改為`NSManagedObject`

```
//var lists = [String]()
var lists = [NSManagedObject]()
```

由於`lists`陣列儲存的型態變了，需要回頭修改`tableView:cellForRowAtIndexPath:`。

```
//cell.textLabel!.text = lists[indexPath.row]

let list = lists[indexPath.row]
cell.textLabel!.text = list.valueForKey("detail") as? String
```

先前將新增的資料放進陣列，現在要換個做法了。修改`@IBAction func add`，`saveList`是我們建立的新function。

```
//self.lists.append(textField.text)
self.saveList(textField.text)
```

`saveList`如下

```
func saveList(list: String) {
    let appDelegate = UIApplication.sharedApplication().delegate as! AppDelegate

    let managedContext = appDelegate.managedObjectContext!

    let entity = NSEntityDescription.entityForName("Somthing", inManagedObjectContext: managedContext)

    let somthing = NSManagedObject(entity: entity!, insertIntoManagedObjectContext: managedContext)

    somthing.setValue(list, forKey: "detail")

    var error: NSError?

    if !managedContext.save(&error) {
        println("失敗囉~ \(error), \(error?.userInfo)")
    }

    lists.append(somthing)
}
```

接著是撈出資料

```
override func viewWillAppear(animated: Bool) {
    super.viewWillAppear(animated)

    let appDelegate = UIApplication.sharedApplication().delegate as! AppDelegate

    let managedContext = appDelegate.managedObjectContext

    let fetchRequest = NSFetchRequest(entityName: "Somthing")

    var error: NSError?

    let fetchedResult = managedContext?.executeFetchRequest(fetchRequest, error: &error) as? [NSManagedObject]


    if let results = fetchedResult {
        lists = results
    }
    else {
        println("Fetch 失敗囉~ \(error), \(error!.userInfo)")
    }
}
```
----------------

完整專案：[Github](https://github.com/lighter/core-data-swift-1)
