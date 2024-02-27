# Ubuntu

## 套件管理

Debian 派系 →Ubuntu→Apt  
Redhat 派系 →CentOS→Yum

## Terminal

### call terminal

```hotkey
ctrl + alt + t
```

### clear

```hotkey
ctrl + l
```

## Bash

[鳥哥認識與學習BASH](http://linux.vbird.org/linux_basic/0320bash.php#set)

| cmd                          | descriptions        |
| ---------------------------- | ------------------- |
| pwd                          | 列出目前完整路徑    |
| top                          |                     |
| cp                           | 複製檔案            |
| tar                          | 打包                |
| cat                          | 把檔案輸出到 stdout |
| touch                        | 建立檔案            |
| touch "kai-\$(date +%Y%m%d)" | 以日期建檔          |
| usermod -aG [group][user]    | add user to group   |
| export VAR=value             | 設定環境變數        |

### 串聯指令

| symbol | descriptions                       |
| ------ | ---------------------------------- |
| `;`    | 連續下達指令                        |
| `&&`   | 當指令回傳 1(成功)則執行下一個指令   |
| `||`   | 當前指令回傳 0(失敗)執行下一指令     |
| `>`    | stdoutput                          |
| `|`    | pipe                               |

example

```bash
cd /home/kai/Desktop/kaitest && touch "kai-$(date +%d%s)"
```

### Disc Usage

```bash
du -h
```

### Add an Existing User Account to a Group

usermod -aG examplegroup exampleusername

### 遠端執行 make 指令

```bash
make -n > xxx.sh
ssh user@host < xxx.sh
```

遠端全套

```bash
sshpass -p $HOST_PWD ssh -o StrictHostKeyChecking=no $WORKER_NAME@$DEPLOY_MACHINE < makerun.sh
```

不檢查 key，自動化好用

```bash
-o StrictHostKeyChecking=no
```

## env, set export

- set: 目前shell的環境變數
- env: 除了是目前shell的環境變數以外，能把這些變數一起帶到目前shell發起的子程式
- export: 設定env變數

## export env from file

```bash
export $(cat env/YOUR_ENV_FILE |xargs)
```

## Makefile

<https://hackmd.io/s/SySTMXPvl>

- 特別字元
  - **@** 不要顯示執行的指令
    - 因執行 make 指令後會在終端機印出正在執行的指令
  - **\-** 表示即使該行指令出錯，也不會中斷執行，而 make 只要遇到任何錯誤就會中斷執行。但像是在進行 clean 時，也許根本沒有任何檔案可以 clean，因而 rm 會傳回錯誤值，因而導致 make 中斷執行。我們可以利用\-來關閉錯誤中斷功能，讓 make 不會因而中斷。

## SSH

### Add ssh private key

```bash
ssh-add [~/.ssh/id_rsa]
```

如果出現

```bash
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@ WARNING: UNPROTECTED PRIVATE KEY FILE! @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
```

表示權限開太大

```bash
chmod 0600 [private key file]
```

### sshpass

sshpass:不用互動遠端使用 ssh 下 command 的工具

```bash
sshpass -p 'mypassword' ssh myaccount@my.linux.host
```

## Cron tab

|         |                                             |
| ------- | ------------------------------------------- |
| cron -e | 編輯該使用者的 crontab 指令                 |
| cron -l | 列出該使用者擁有的 crontab 指令             |
| cron -r | 將使用者的 crontab 全部清除！（ 小心使用 ） |

※ 下達指令請用 絕對路徑 避免錯誤

```bash
*/5 * * * * /home/ubuntu/test.sh：每五分鐘執行一次測試 shell script
```

## Set static IP

用 UI 點

## Resources

[GNU/Linux 常用指令](https://wiki.ubuntu-tw.org/index.php?title=GNU/Linux_%E5%B8%B8%E7%94%A8%E6%8C%87%E4%BB%A4)

https://en.wikipedia.org/wiki/Sed