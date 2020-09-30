# Refactoring

## Chapter 1 Refactoring: A First Example

>When you have to add a feature to a program but the code is not structured in a convenient way, first refactor the program to make it easy to add the feature, then add the feature.

注意一切都是因為"改動"的需求造成需要重構，如果程式碼運作正常且不需要改動，也沒有人需要看，那麼"It's perfectly fine to leave it alone."

>Before you start refactoring, make sure you have a solid suite of tests. These tests must be self-checking.

測試必須要self-checking，這樣才不會托慢速度，才會頻繁執行。

重構長函式通常先找尋可以分離行為的地方。

每次的小更動結束都跑一次測試，這也代表一旦發現錯誤你尋找他的範圍就很小，這就是重構的根本：小改動而每次改動完跑測試。

>Refactoring changes the programs in small steps, so if you make a mistake, it is easy to find where the bug is.

使用版控，每次的重構都使用private commit，最後push之前再squash changes.

>Any fool can write code that a computer can understand. Good programmers write code that humans can understand.

大多時候重構不太會造成效能問題，而且聰明的編譯器或現代快取機制也會影響我們對程式碼實際的效能判斷，通常軟體的效能只取決於一部分的程式碼。但總有例外的時候，如果重夠確實造成了效能問題，那就重構之後再開始調教效能，儘管有可能回復到重構之前的狀態，但大多時候因為重構的關係你可以把效能調教得更好，最後得到效能好又簡潔的程式碼。  
所以整體而言重構對效能的建議是：忽略大部分的情況，如果真的遇上了就先重構再調效能。

>I prefer to treat data as immutable as much as I can-mutable state quickly becomes something rotten.

跟重構前的程式相比多了很多行，但這讓邏輯更佳清楚且新增功能更方便

>Brevity is the soul of wit, but clarity is the soul of evolvable software.

我總是要在我能做的所有重構和添加新功能之間取得平衡。目前，大多數人都沒有優先考慮重構 - 但仍然存在平衡

>When programming, follow the camping rule: Always leave the code base healthier than when you found it.

很多人喜歡討論好的程式碼應該長甚麼樣子，但真正該關心的是如何改善而不是個人口味

>The true test of good code is how easy it is to change it.

### Javascript小技巧

1. 使用extract function可以利用nested function，這樣就不用傳遞參數
2. 在動態型別的語言，把default parameter名稱加上型別名稱
3. 利用複製物件新增資訊達到盡量使資料immutable

複製物件

```javascript
Object.assign({}, data)
```
