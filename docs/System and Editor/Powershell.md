# PowerShell Notes

## 常用cmdlet

確認版本

```powershell
Get-Host
```

取得時間

```powershell
Get-Date -Format yyyyMMdd

# 定義來源與目的資料夾的絕對路徑
$srcfolder = 'C:\Program Files (x86)\Bridgewell'    #最新檔案的資料夾
$folderName = 'update' + (Get-Date -Format yyyyMMdd)
$tarfolder = 'C:\Users\Administrator\Desktop\' + $folderName   #目的資料夾
```

指令執行路徑

```powershell
Split-Path $MyInvocation.MyCommand.Path
```

script路徑

```powershell
$PSScriptRoot
```

## 常用指令

## 清除畫面(變數還會存在)

```powershell
cls
```

## 開啟指令目錄

```powershell
start .
```

## Prompt呼叫script:Invoke-Expression

範例

```powershell
Invoke-Expression c:\scripts\test.ps1
& c:\scripts\test.ps1
```

## 傳遞Parameter

```powershell
$args[0]
$args[1]
$args[2]
```

## 遠端PS

```powershell
Enter-PSSession Server01
Exit-PSSession
```

```powershell
$secpasswd = ConvertTo-SecureString "password" -AsPlainText -Force
$mycreds = New-Object System.Management.Automation.PSCredential ("user", $secpasswd)
Enter-PSSession -ComputerName n196 -Credential $mycreds
```

## 無資料夾則建立

```powershell
IF (!(Test-Path $FolderToCreate -PathType Container)) {
    New-Item -ItemType Directory -Force -Path $FolderToCreate
}
```

## WindowsService

```powershell
net start BWECDataUpdateWindowService
net stop BWECDataUpdateWindowService
```

## 字串Slpit, Trim

```powershell
$parts = $content -split '\n'
$parts[0]
$parts[1]
if ($parts[0].Trim() -eq '@Q@')
{
    'WTF'
}
```

## 讀檔

```powershell
$content = [IO.File]::ReadAllText(".\test.txt")
```

## Get Assembly Version

```powershell
function get-assembly-version() {
    param([string] $file)

    $version = [System.Reflection.AssemblyName]::GetAssemblyName($file).Version;

    #format the version and output it...
    $version
}
```

## 暫停

```powershell
pause
```

## 顯示通訊協定統計資料

```powershell
nbtstat
```

## DNS

```powershell
nslookup
```

## How to debug ps1

`Write-Host` == `printf`

Join-Path
Select-String

## 確認客戶給的帳密是否正確

```powershell
runas /u:yourdomain\a_test_user notepad.exe
```

## Exit code

```powershell
Exit 0
```

## Environment Variables in PowerShell

```powershell
$env:<variable_name>
```
