# VisualStudio

## Package Dependency

會以最上層的程式使用的版本為準
底層的元件跟上層的package版本不符會跳error
就算是裡面沒有用到該package
在最外層專案能看所有底下程式使用的package版本

## Specflow lose track

1. Exit Visual Studio.
2. Open Windows Explorer.
3. In the address bar, type %TEMP% and hit Enter to go to your temp folder.
4. Find the files whose names start with "specflow-stepmap-YourProjectName" with a .cache extension.
5. Delete those files.
6. Start Visual Studio again.

## 將錯誤訊息轉成英文

```c#
System.Threading.Thread.CurrentThread.CurrentCulture = System.Globalization.CultureInfo.CreateSpecificCulture("en-US");
System.Threading.Thread.CurrentThread.CurrentUICulture = new System.Globalization.CultureInfo("en-US");
```
