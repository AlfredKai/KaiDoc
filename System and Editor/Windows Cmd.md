# Windows Cmd

執行完命令後不關閉窗口：

```bat
cmd /k dir
```

可用來combo在捷徑的目標內創造出預設好路徑的捷徑：

```bat
%windir%\system32\cmd.exe /k "C:\Program Files (x86)\NUnit.org\nunit-console\nunit_console.bat"
```

看各port開啟了哪些服務

```bat
netstat -a
netstat -ant | find ":80"
```

在中文版Windows中，Command Prompt預設使用BIG5編碼，因此檢視UTF-8編碼檔案時會出現亂碼。使用chcp切換語系成UTF-8

```bat
chcp 65001
```
