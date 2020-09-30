# IIS

## Simple HTML5 Server

1. `站台右鍵->新增應用程式->設定實體路徑`
2. 確定web.config的權限，權限設定完記得重啟iis
3. 預設文件把目標文件往上移成為預設目錄，可以在`站台->管理應用程式->瀏覽`直接開啟預設目錄
4. 如果你發現所有東西都是空的，可能是沒開啟靜態內容：`In Control Panel --> Programs --> Programs And Features --> Turn Windows features on or off -> Internet Information Services -> World Wide Web Services -> Common HTTP Features -> Static Content` Also make sure .NET Extensibility 3.5 and .NET Extensibility 4.5 are checked.
