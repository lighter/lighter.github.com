---
layout: post
title: "Core Data-Part3"
date: 2012-11-01 11:48
comments: true
tags: 
- iPhone
- core data
---

延續[Core Data-Part2](http://lighter.tw/blog/2012/06/02/core-data-part2/)的文章，現在要加入的是搜尋，當Table View中有越來越多的資料後，要尋找某比資料的話，用眼睛尋找是一件很累人的事情！所以可以透過輸入關鍵字來搜尋，接下來就來完成這個範例。

{% img /images/CoreDataWithSearch/1.png 216px 390px %}
{% img /images/CoreDataWithSearch/2.png 216px 390px %}

<!-- more -->

**Step 1.加入<code>Search Bar and Search Display Controller</code>**

加入<code>Search Bar and Search Display Controller</code>到<code>Table View</code>上方。

{%img /images/CoreDataWithSearch/3.png %}

**Step 2.編輯MasterViewController.h檔案**

{% codeblock MasterViewController.h lang:objc %}
#import <UIKit/UIKit.h>
#import <CoreData/CoreData.h>

//1
#import "Phone.h"

//2
@interface MasterViewController : UITableViewController 
<NSFetchedResultsControllerDelegate, UISearchDisplayDelegate, UISearchBarDelegate>
{
  //3
  BOOL savedSearchTerm;
}

@property (strong, nonatomic) NSFetchedResultsController *fetchedResultsController;
@property (strong, nonatomic) NSManagedObjectContext *managedObjectContext;

//4
@property (nonatomic, retain) NSMutableArray *searchResults;

//5
@property (nonatomic, retain) NSIndexPath *indexP;

@end
{% endcodeblock %}

1.	引入<code>Phone.h</code>。
2.	加入<code>UISearchDisplayDelegate</code>、<code>UISearchBarDelegate</code>兩個協定。
3.	<code>saveSearchTerm</code>用來判斷是否為搜尋狀態。
4.	<code>searchResults</code>陣列是用來儲存搜尋的結果。
5.	<code>indexP</code>是用來取得搜尋結果，點選cell的索引值。

**Step 3.編輯MasterViewController.m**

{% codeblock MasterViewController.m lang:objc %}
@synthesize indexP;
{% endcodeblock %}

補上<code>synthesize</code>。

{% codeblock MasterViewController.m lang:objc %}
//1
- (void)viewDidLoad
{
    [super viewDidLoad];
    self.searchResults = [NSMutableArray arrayWithCapacity:[[self.fetchedResultsController fetchedObjects] count]];
    [self.tableView reloadData];
}

//2
- (void)viewDidUnload
{
    [super viewDidUnload];
    self.searchResults = nil;
}
{% endcodeblock %}

1.	在`viewDidLoad`方法內對`searchResults`初始化，並且重新整理`Table View`。
2.	`viewDidUnload`方法內，將`searchResults`釋放掉。

{% codeblock MasterViewController.m lang:objc %}
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
  if (tableView == self.searchDisplayController.searchResultsTableView) {
    return [self.searchResults count];
  }
  else {
    id <NSFetchedResultsSectionInfo> sectionInfo = [[self.fetchedResultsController sections] objectAtIndex:section];
    return [sectionInfo numberOfObjects];
  }
}
{% endcodeblock %}

`Table View`的`cell`個數，在搜尋的時候會不一樣，因此`tableView:numberOfRowsInSection:`方法需要做修改。 

如上當`if`判斷式條件成立時，會回傳`searchResults`陣列的個數，否則就回傳原本`fetchedResultsController`的個數。

{% codeblock MasterViewController.m lang:objc %}
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"Cell"];
	
    //1
    if (cell == nil) {
      cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"Cell"];
      cell.accessoryType = UITableViewCellAccessoryDisclosureIndicator;
    }
	
    //2
    Phone *phone = nil;
    if (tableView == self.searchDisplayController.searchResultsTableView)
    {
      phone = [self.searchResults objectAtIndex:indexPath.row];
    }
    else
    {
      phone = [self.fetchedResultsController objectAtIndexPath:indexPath];
    }

    //3
    cell.textLabel.text = phone.name;
    return cell;
}
{% endcodeblock %}

1.	這邊需要額外使用程式碼的方式定義一個`cell`，並且初始化，因為在搜尋的時候並不知道`Storyboard`中的`cell`。
2.	當`tableView`是在搜尋的狀態時，指定`searchResults`搜尋結果給`phone`，反之將`fetchedResultsController`指定給`phone`。
3.	這邊改成直接指定`cell`的文字為`phone.name`。

{% codeblock MasterViewController.m lang:objc %}
#pragma mark -
#pragma mark UISearchDisplayController Delegate Methods

- (BOOL)searchDisplayController:(UISearchDisplayController *)controller shouldReloadTableForSearchString:(NSString *)searchString
{
  [self filterContentForSearchText:searchString scope:@"All"];
  savedSearchTerm = TRUE;
  return YES;
}

- (BOOL)searchDisplayController:(UISearchDisplayController *)controller shouldReloadTableForSearchScope:(NSInteger)searchOption
{
  [self filterContentForSearchText:[self.searchDisplayController.searchBar text] scope:@"All"];
  savedSearchTerm = TRUE;
  return YES;
}
-(void) searchDisplayControllerWillEndSearch:(UISearchDisplayController *)controller{
  savedSearchTerm = TRUE;
}
{% endcodeblock %}

加入上述三個`UISearchDisplayController`的方法，並且指定`savedSearchTerm`為`TRUE`，其中可以看到`filterContentForSearchText:scope:`這個方法，該方法是用來根據您輸入的文字做搜尋的。

{% codeblock MasterViewController.m lang:objc %}
#pragma mark -
#pragma mark Content Filtering

- (void)filterContentForSearchText:(NSString*)searchText scope:(NSString*)scope
{
	[self.searchResults removeAllObjects];
  NSLog(@"search text: %@", searchText);
	for (Phone *phone in [self.fetchedResultsController fetchedObjects])
	{
		if ([scope isEqualToString:@"All"] || [phone.name isEqualToString:scope])
		{
			NSComparisonResult result = [phone.name compare:searchText
                                             options:(NSCaseInsensitiveSearch|NSDiacriticInsensitiveSearch)
                                               range:NSMakeRange(0, [searchText length])];
      if (result == NSOrderedSame)
			{
				[self.searchResults addObject:phone];
      }
		}
	}
}
{% endcodeblock %}

該方法會根據您輸入的字串，尋找到符合的結果！

{% codeblock MasterViewController.m lang:objc %}
//1
-(void) tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
{
  if (savedSearchTerm) {
    indexP = indexPath;
    [self performSegueWithIdentifier:@"showDetail" sender:self];
  }
}

//2
- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender
{
    if ([[segue identifier] isEqualToString:@"showDetail"]) {
      if (savedSearchTerm) {
        NSManagedObject *object = [[self fetchedResultsController] objectAtIndexPath:indexP];
        [[segue destinationViewController] setDetailItem:object];
      } else {
        NSIndexPath *indexPath = [self.tableView indexPathForSelectedRow];
        NSManagedObject *object = [[self fetchedResultsController] objectAtIndexPath:indexPath];
        [[segue destinationViewController] setDetailItem:object];
      }
    }
}
{% endcodeblock %}

1.	當你搜尋完畢後要可以點選`cell`，然後看到該`cell`的細部資訊，所以透過`tableView:didSelectRowAtIndexPath:`方法可以取得你點選`cell`的`indexPath`索引值，將該值指定給`indexP`，在呼叫到`perforSegueWithIdentifier:sender:`方法。
2.	該方法就是要用來傳遞參數到`DetailViewController`的，一樣需要判斷是否在搜尋的狀態，然後再傳遞對應的索引值(`indexPath`或`indexP`)。