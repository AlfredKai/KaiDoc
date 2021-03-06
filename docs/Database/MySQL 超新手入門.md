# MySQL 超新手入門

[MySQL 超新手入門](http://www.codedata.com.tw/database/mysql-tutorial-getting-started)

## SELECT 基礎查詢

- 查詢子句順序
  - SELECT  
  - FROM  
  - WHERE  
  - GROUP BY  
  - HAVING  
  - ORDER BY  
  - LIMIT
- 指定使用中資料庫:`USE`，如果使用UI通常用不到這指令
- 不用`USE`可以用`DatabaseName.TableName`
- `SELECT`單獨使用可以當print使用
- 可以使用別名在SELECT之後，使用`AS`或`空格`接上想要取的名稱
- 一些特殊實用的的條件運算子：`BETWEEN...AND...`,`IN(...)`,`IS`
- `BETWEEN...AND...`是包含(>=,<=)
- NULL要用`IS`或`<=>`判斷不能直接使用`=`，不等於使用`IS NOT`
- `LIKE`專門處理字串，後面接
  - %：0到多個任何字元
  - _ ：一個任何字元
- `LIMIT`除了回傳限制外可以給兩個數字第一個代表跳過幾筆

## 運算式與函式

### 值與運算式

- 數值：可以用來執行算數運算的數值，包含整數與小數，分為精確值與近似值兩種
- 字串：使用單引號或雙引號包圍的文字
- 日期/時間：使用單引號或雙引號包圍的日期或時間
- 空值：使用「NULL」表示的值
- 布林值：「TRUE」或「1」表示「真」，「FALSE」或「0」表示「假」

- 如果你拿字串來執行算數運算的話，MySQL會先把字串中的內容轉換為數字，然後再執行算數運算
- 日期可以用關鍵字`INTERVAL`運算
- NULL值與其它任何值都不一樣，包含NULL自己
- 看到`()`他就是函式
  - 字串處理：大小寫、串接、TRIM、REPLACE...
  - 數學：ROUND、CEIL、FLOOR...
  - 日期時間：目前日期、年月日、加減乘除...
  - 流程控制：
    - `IF(條件, 運算式1, 運算式2)`跟三元運算子有87%像
    - `CASE WHEN THEN`一個條件不用夠用的時候...(然後這跟switch有87%像)
  - 其他：IFNULL()、ISNULL

### 群組查詢

群組函式：

- MAX(運算式)：最大值
- MIN(運算式)：最小值
- SUM(運算式)：合計
- AVG(運算式)：平均
- COUNT([DISTINCT]\*|運算式)：使用「DISTINCT」時，重複的資料不會計算；使用[*]時，計算表格紀錄的數量：使用[運算式]時，計算的數量不會包含「NULL」值

- `COUNT()`裡面塞欄位名稱的話，NULL不會被計算
- `GROUP_CONCAT`是用來串接字串資料的函式

```sql
GROUP_CONCAT( [DISTINCT] 運算式 [排序設定] [SEPARATOR 運算式] )
```

#### GROUP BY與HAVING子句

在上列使用群組函式的所有範例中，都是將「FROM」子句中指定的表格當成是一整個「群組」，群組函式所處理的資料是表格中所有的紀錄。如果希望依照指定的資料來計算分組統計與分析資訊，使用`GROUP BY`

```sql
GROUP BY {欄位|運算式|位置編號} [ASC|DESC] [WITH ROLLUP] [,...]
```

```sql
HAVING 分組條件
```

- 可以GROUP BY多個欄位，意思是照順序分組
- 使用「GROUP BY」指定群組的設定以後，回傳的群組查詢資料都會依照指定的群組排序，預設定排序方式是遞增排序，使用「DESC」關鍵字可以指定排序的方式為遞減排序
- 使用「GROUP BY」子句的時候可以搭配「WITH ROLLUP」，效果會作用在查詢中的每一個群組函式，功用式統計所有數量
- 在「GROUP BY」子句中有多個群組設定的時候，你可以在最後面加入「WITH ROLLUP」
- 包含群組函式的條件設定就一定要放在「HAVING」子句中
- SELECT了沒有出現在GROUP BY的欄位行為會很奇怪，但MySQL不會報錯，有設定可以設`ONLY_FULL_GROUP_BY`強制沒有使用群組函式的欄位一定要全部出現在GROUP BY中，不然會報錯

## JOIN 與 UNION 查詢

### Inner Join

```sql
SELECT  表個名稱.欄位名稱[,表個名稱.欄位名稱...]
FROM    表個名稱 [AS] [表格別名][,表個名稱 [AS] [表格別名]...]
WHERE   結合條件
```

懶人寫法

```sql
SELECT  表個名稱.欄位名稱[,表個名稱.欄位名稱...]
FROM    表個名稱 [INNER] JOIN 表個名稱 ON 結合條件
```

當不同Table而欄位名稱一樣時可以更懶

```sql
SELECT 表個名稱.欄位名稱[,表個名稱.欄位名稱...]
FROM   表個名稱 [INNER] JOIN 表個名稱 USING (結合欄位...)
```

### Outer Join

```sql
SELECT  表格名稱.欄位名稱[,表格名稱.欄位名稱...]
FROM    表格名稱 {LEFT | RIGHT} [OUTER] JOIN 表格名稱 {ON 結合條件 | USING (結合欄位...)}
```
- right or left join就是看哪邊需要`另一邊`沒值得時候也出現

### Union

- join是一個查詢使用多個table，而union就是把一個以上的查詢合併，聽起來就不是個好主意
- 欄位名稱和數量要一樣(的樣子?)

## CRUD 與資料維護

- `DESCRIBE`或`DESC`是專屬於MySQL的指令，取得Table結構資訊
- 欄位是有順序的，`DESC`玩由上至下就是他的欄位順序

### 新增

#### 基礎新增敘述

按欄位順序在表格新增一筆資料

```sql
INSERT [INTO] 表格名稱
VALUES ( 運算式 | DEFAULT,...)
```

當你忘記順序的時候可以用，此種方式沒有寫出的欄位會使用default

```sql
INSERT [INTO] 表格名稱 [(欄位名稱,...)]
VALUES ( 運算式 | DEFAULT,...)
```

全欄位default

```sql
INSERT [INTO] 表格名稱 () VALUES ()
```

另一種寫法，感覺比較好讀

```sql
INSERT [INTO] 表格名稱 
SET 欄位名稱 = 運算式|DEFAULT [,...]
```

一次新增多筆，簡單來說就是`VALUES`後面加上,()

```sql
INSERT [INTO] 表格名稱 [(欄位名稱,...)]
VALUES ( 運算式 | DEFAULT,...)[,...]
```

#### Primary Key

- 在設計表格的時候，通常會視需要指定表格中的某一個欄位為「主索引」欄位
- 你可以在使用`INSERT`敘述的時候，加入`IGNORE`關鍵字，它可以在執行一個違反主索引規定的新增敘述時，自動忽略新增的動作，這樣就不會產生錯誤訊息

#### ON DUPLICATE KEY UPDATE

使用`INSERT`敘述新增紀錄的時候，還可以視需要在最後搭配一串關鍵字`ON DUPLICATE KEY UPDATE`，它可以用來指定在違反重複索引值的規定時要執行的修改

```sql
INSERT ...
ON DUPLICATE KEY UPDATE 欄位=運算式[,...] 
```

### 「REPLACE」敘述

除了使用「INSERT」敘述新增紀錄外，「REPLACE」敘述同樣可以新增紀錄，它們的語法幾乎相同：

```sql
REPLACE [INTO] 表格名稱 [(欄位名稱,...)]
VALUES ( 運算式 | DEFAULT,...)
```

```sql
REPLACE [INTO] 表格名稱 
SET 欄位名稱 = 運算式|DEFAULT [,...]
```

就是當Key一樣時直接取代

### 修改

```sql
UPDATE [IGNORE] 表格名稱
SET 欄位名稱 = 運算式|DEFAULT [,...]
[WHERE 條件]
[ORDER BY 排序]
[LIMIT 限制]
```

- 一定要注意沒下where就會變成全欄位...
- 一樣可下`IGNORE`忽略索引鍵錯誤
- 如果`IGNORE`忽略的是轉型錯誤會有意想不到的驚喜(雷爆)
- 搭配`ORDER BY`與`LIMIT`範例：幫薪水最低的三個月公加薪

### 刪除

```sql
DELETE [IGNORE] FROM 表格名稱
[WHERE 條件]
[ORDER BY 排序]
[LIMIT 限制]
```

「TRUNCATE」敘述在執行刪除紀錄的時候，會比使用「DELETE」敘述的效率好一些，尤其是表格中的紀錄非常多的時候會更明顯。

```sql
TRUNCATE [TABLE] 表格名稱
```

「TRUNCATE」敘述在執行刪除紀錄的時候，會比使用「DELETE」敘述的效率好一些，尤其是表格中的紀錄非常多的時候會更明顯。

## 字元集與資料庫

### Character Set與Collation

- Character Set(字元集)就是編碼
- Collation(or binary collation)是字元大小排序規則
  
可以看資料庫支援的字元集

```sql
SHOW CHARACTER SET
```

每種字元集可以搭配不同的Collation，同樣：

```sql
SHOW COLLATION
```

### 資料庫

每個資料庫都會把資料存在`資料庫資料夾`中

- 雖然MySQL對於資料庫的數量並沒有限制，可是你要注意MySQL資料庫伺服器軟體所安裝的作業系統，它對於資料夾與檔案大小的限制。
- MySQL使用資料庫名稱作為資料庫資料夾的名稱，所以你要特別注意大小寫的問題。在資料夾名稱不分大小寫的作業系統(例如Windows)，資料庫名稱「MyDB」和「mydb」是一樣的；可是在資料夾名稱會區分大小寫的作業系統(例如Linux)，資料庫名稱「MyDB」和「mydb」就不一樣了。
- 每一個資料庫資料夾中都有一個特別的檔案，檔案名稱是「db.opt」，這個檔案的內容是資料庫的字元集與collation設定。

註：MySQL把「DATABASE」與「SCHEMA」當成是一樣的，所有你在後續使用的指令，都可以把「DATABASE」換成「SCHEMA」。

- 建立可以用`IF NOT EXISTS`檢查，刪除可以用`IF EXISTS`檢查
- 建立資料庫時可以同時指定字元集與collation，也可以都不指定或只指定一個，沒被指定的會使用default。
- 建立了之後還是可以修改字元集或是collation，但不會影響已經存在的table
- 刪除會直接掰掰，不會再問你要不要刪除，檔案也會掰

取得所有資料庫名稱：

```sql
SHOW DATABASE
```

或

```sql
SHOW SCHEMAS
```

取得建立資料庫的sql：

```sql
SHOW CREATE DATABASE mydb
```

MySQL資料庫伺服器有一個很重要的資料庫，名稱為「information_schema」，這個資料庫通常會把它稱為「系統資料庫」，資料庫中儲存伺服器所有重要的資訊。跟資料庫相關的資訊儲存在「SCHEMATA」表格中，所以你可以使用查詢敘述取得所有資料庫的相關資訊：

```sql
SELECT * FROM information_schema.SCHEMATA
```

## 儲存引擎與資料型態

### 表格與儲存引擎

- 資料庫資料夾裡面會在依照table分資料夾儲存
- 「Storage engine、儲存引擎」是MySQL用來儲存資料的技術，為了資料庫多樣化的應用，你可以在建立表格的時候，依照自己的需求指定一種儲存引擎
  - MyISAM：MySQL預設的儲存引擎，雖然它支援的功能並沒有像一般的資料庫那麼多(例如交易、transaction)；不過也因為它比較簡單，所以運作的效率相對也比較好
  - InnoDB：這種儲存引擎所提供的功能已經跟大型的商用資料庫軟體一樣了，像是交易(transaction)、紀錄鎖定(row-level locking) 與自動回復(auto-recovery)。
  - MEMORY：這是一個比較特殊的儲存引擎，它把資料儲存在紀憶體中，所以運作的效率是最快的；不過只要MySQL伺服器關閉後，儲存的資料就全部不見了。

MyISAM vs InnoDB

- MyISAM可以用複製檔案的方式搬移table
- 相較於MyISAM每個table有各自的儲存空間，InnoDB共用一個空間儲存所有表格

### 欄位資料型態

#### 數值與位元

- 整數可以設定長度限制，我不知道幹啥用
- 浮點數可以設定長度跟小數長度限制，小數太長會四捨五入，整數部分超過會錯誤
- 把float塞進int不會出錯，會幫你四捨五入
- MySQL的數值型態，包含整數與浮點數都可以設定為「只能儲存正數」
- 「ZEROFILL」的設定表示在查詢這些欄位的時候，回傳的資料會在左側根據長度的設定填滿「0」
- 儲存bit型態可以用整數也可以用二進位表示(b'xxx')

#### 字串

MySQL把字串型態分為兩大類：「非二進位制、non-binary」與「二進位制、binary」。非二進位制就是儲存一般文字的字串，會有特定的字元集與collation；二進位制使用位元組儲存資料，不包含字元集與collation，所以大多用來儲存圖片或音樂這類資料。

- 只有char是固定長度字串，長度不夠會用空白補滿
- 固定長度與變動長度的兩種字串型態都可以儲存字串，差異在儲存的文字個數小於型態指定的長度時，變動長度實際儲存的空間會小一些
- 每種字元集佔用空間長度不一，可以查詢MySQL資料庫支援的字元集特性，「MAXLEN」欄位是關於儲存空間的資訊
- 使用在「LENGTH」函式來查詢儲存在這個表格中的字串資料，就可以很明顯的看出不同的字元集，在儲存字元時使用的儲存空間
- 「LENGTH」函式會傳回字串資料實際的儲存長度(byte)；如果你要查詢字串的字元數量的話，就要使用「CHAR_LENGTH」函式
- Collation除了影響排序外，其中的「ci」(case insensitive)還會影響比較
  
#### 列舉與集合

- ENUM就是會幫你做檢查的VARCHAR拉
- 列舉(ENUM)型態欄位除了可以直接使用字串值來新增與更新資料外，還可以使用數值資料的編號來代替，任何一個列舉型態中的成員，MySQL都會幫它們編一個號碼
- 雖然在查詢列舉型態欄位資料的時候，所得到的結果都是成員的字串值；不過真正儲存在資料庫中的資料卻是成員的編號，所以指定列舉型態欄位為排序欄位的時候，資料庫會使用編號來排序，而不是以成員的字串值
- 在指定列舉型態欄位的查詢條件時，可以使用成員的字串值或編號
- 集合(SET)型態同樣可以設定一組成員，不過它可以儲存多個成員資料，一樣具有驗證功能
- 集合的成員編號是用bit flag方式儲存(0,2,4,8,16...)
- 列舉與集合型態都可以設定需要的字元集與collation，collation的大小寫會影響驗證

#### 日期與時間

- 日期(DATE)型態欄位可以儲存年、月、日的資料，範圍從「1000-01-01」到「9999-12-31」，你的日期資料不可以超過「9999-12-31」，可是你可以儲存「1000-01-01」以前的日期，不過MySQL建議你最好不要這麼作，不然可能會造成一些奇怪的問題。
- 時間(TIME)型態可以儲存時、分、秒的資料，範圍從「-838:59:59」到「838:59:59」,MySQL的時間型態欄位可以讓你儲存類似「經過的時間」這樣的資料
- 在指定一個時間資料的時候，你可以省略秒或分，省略的部份，MySQL都會幫你設定為「0」
- 日期與時間(DATETIME)型態可以儲存完整的年、月、日與時、分、秒資料，範圍從「1000-01-01 00:00:00」到「9999-12-31 23:59:59」。
- 如果只需要儲存年份資料的話，你可以使用西元年(YEAR)型態，這樣會節省很多儲存空間。
- 「TIMESTAMP」型態的格式與「DATETIME」一樣，都包含完整的年、月、日與時、分、秒資料，不過它使用的儲存空間只有4bytes，是「DATETIME」型態的一半。
- 「TIMESTAMP」也是MySQL日期與時間型態中具有「時區」特性的型態。它可以儲存從「1970-01-01 00:00:00」到目前經過的秒數。這個起始日期與時間使用「Coordinated Universal Time、UTC」世界標準時間為儲存資料的依據，它與「Greenwich Mean Time、GMT」格林威治標準時間是一樣的。
- MySQL資料庫採用與作業系統同樣的時區設定，所以在儲存「TIMESTAMP」型態欄位的資料時，過程中會有一些計算的動作

## 表格與索引

### 建立表格

```sql
CREATE TABLE [IF NOT EXISTS] 表格名稱
(欄位定義,...)
[{ENGINE | TYPE} [=] 儲存引擎名稱]
[CHARACTER SET [=] 字元集名稱]
[COLLATE [=] collation名稱]
```

欄位定義

```sql
欄位名稱 欄位型態 [欄位屬性],...
```

查詢引擎

```sql
SHOW ENGINES
```

修改設定檔：MySQL資料庫伺服器在啟動時會讀取一個名稱為「my.ini」的設定檔，檔案中有許多啟動資料庫伺服器時需要的資訊。其中就包含預設的儲存引擎設定，你可以修改這個設定後再重新啟動資料庫伺服器，讓新的設定生效：

```ini
default-storage-engine=InnoDB
```

設定儲存引擎：你也可以使用「SET」敘述設定預設的儲存引擎：

```sql
SET GLOBAL storage_engine = 儲存引擎
```

單次生效：

```sql
SET SESSION storage_engine = 儲存引擎
SET storage_engine = 儲存引擎
```

- 可以只針對單一欄位指定字元集與collation，沒指定的就使用default
- 數值型態欄位專用的屬性設定有「UNSIGNED」、「ZEROFILL」與「AUTO_INCREMENT」
- 除了字串與數值兩種欄位專用的欄位屬性設定外，還有「NULL」、「NOT NULL」
- 如果你沒有為欄位使用「DEFAULT」關鍵字設定預設值，而且也沒有設定為「NOT NULL」，MySQL會自動為你加入預設值的設定「DEFAULT NULL」
  
#### TIMESTAMP

- 在表格中使用「TIMESTAMP」型態的欄位時，如果你沒有設定它們的欄位屬性，MySQL會自動幫你在第一個「TIMESTAMP」欄位加入「NOT NULL」、「DEFAULT」和「ON UPDATE」三個欄位屬性的設定。其它沒有設定欄位屬性的「TIMESTAMP」欄位，MySQL會幫你加入「NOT NULL」與「DEFAULT」兩個欄位屬性。
  - 「NOT NULL」不允許你儲存「NULL」值
  - 「DEFAULT CURRENT_TIMESTAMP」設定預設值為目前的日期時間。在所有欄位型態中，只有「TIMESTAMP」可以使用「CURRENT_TIMESTAMP」指定預設值；其它的欄位型態，在指定預設值只能是「一個明確的值」
  - 「ON UPDATE」可以指定在修改紀錄的時候，MySQL自動幫你填入的資料
- 在一個表格中，MySQL限制「CURRENT_TIMESTAMP」只能在一個欄位出現。
- 儲存建立紀錄的時間欄位使用「0」代替「CURRENT_TIMESTAMP」，當欄位的值為「NULL」的時候，MySQL會自動為你填入目前的日期與時間。(超奇怪...)

#### 用其他表格建新表格

- 可以使用CREATE TABLE搭配查詢語法直接把查詢結果建成表格
  - MySQL使用查詢結果的欄位名稱與型態來建立新的表格
  - 如果沒有指定儲存引擎、字元集或collation的話，建立的新表格使用資料庫預設的儲存引擎、字元集與collation
  - 查詢表格中，欄位的索引與「AUTO_INCREMENT」設定都會被忽略

只建Schema

```sql
CREATE TABLE [IF NOT EXISTS] 表格名稱 { LIKE 表格名稱 | (LIKE 表格名稱)}
```

使用這種語法建立的新表格，並不會新增紀錄到新表格中，可是包含索引與「AUTO_INCREMENT」設定都會套用在新表格，除了下列兩個例外：

- 使用「MyISAM」儲存引擎時，你可以在建立表格的時候使用「DATA DIRECTORY」與「INDEX DIRECTORY」指定資料與索引檔案的資料夾位置；建立的新表格會忽略這些設定，而使用資料庫預設的資料夾
- 欄位的「FOREIGN KEY」與表格的「REFERENCES」屬性設定都會被忽略

上列討論的建立表格方式，都可以在建立表格的時候，依照需要加入「TEMPORARY」關鍵字，指定這個新建立的表格為「用戶端暫時存在」的表格：

#### 建立暫存表格

```sql
CREATE [TEMPORARY] TABLE [IF NOT EXISTS] 表格名稱
```

「TEMPORARY」表格有下列重點：

- 「TEMPORARY」表格是每一個用戶端專屬的表格，用戶端離線後，MySQL就會自動刪除這些表格
- 因為「TEMPORARY」表格是用戶端專屬的表格，其它用戶端不能使用，所以不同的用戶端，使用同樣名稱建立「TEMPORARY」表格也沒有關係
- 「TEMPORARY」表格名稱可以跟資料庫中的表格名稱一樣，不過在「TEMPORARY」表格存在的時候，資料庫中的表格會被隱藏起來
- 可以使用「ALTER TABLE」修改「TEMPORARY」表格名稱，不可以使用「RENAME TABLE」修改「TEMPORARY」表格名稱

### 修改表格

```sql
ALTER TABLE 表格名稱 修改定義[,...]
```

新增

```sql
ADD [COLUMN] 欄位定義 [FIRST | AFTER 欄位名稱]  #增加欄位指定位置
ADD [COLUMN] (欄位定義[,...])                   #增加多個新欄位
```

修改

```sql
CHANGE [COLUMN] 舊欄位名稱 新的欄位定義 [FIRST | AFTER 欄位名稱]    #改名稱、定義與位置
MODIFY [COLUMN] 欄位定義 [FIRST | AFTER 欄位名稱]                   #改定義與位置
```

刪除

```sql
DROP [COLUMN] 欄位名稱
```

修改表格名稱

```sql
ALTER TABLE 舊表格名稱 RENAME [TO] 新表格名稱 
RENAME TABLE 舊表格名稱 TO 新表格名稱[,...]     #可以一次改多個
```

刪除表格

```sql
DROP TABLE [IF EXISTS] 表格名稱[,...]
```

`MySQL不會跟你確認`

### 索引介紹

索引分為主索引鍵(primary key)、唯一索引(unique index)與非唯一索引(non-unique index)三種。

主索引鍵的應用很常見，而且一個表格通常會有一個，而且只能有一個。在一個表格中，設定為主索引鍵的欄位值不可以重複，而且不可以儲存「NULL」值。因為這樣的限制，所以很適合使用在類似編碼、代號或身份證字號這類欄位。

唯一索引也稱為「不可重複索引」，在一個表格中，設定為唯一索引的欄位值不可以重複，但是可以儲存「NULL」值。這種索引適合用在類似員工資料表格中儲存電子郵件帳號的欄位，因為員工不一定有電子郵件帳號，所以允許儲存「NULL」值，可以每一個員工的電子郵件帳號都不可以重複。

上列兩種索引都可以預防儲存的資料發生重複的問題，也可以增加查詢與維護資料的效率。非唯一索引就只是用來增加查詢與維護資料效率的索引。設定為非唯一索引的欄位值可以重複，也可以儲存「NULL」值。

```sql
CREATE TABLE 表格名稱(
    欄位定義 [UNIQUE] [KEY] | [PRIMARY [KEY]]
)
```

預設的「HASH」演算法適合用在主索引鍵和唯一索引，這種演算法在搜尋不能重複的資料時，效率會比較好；而「BTREE」演算法適合用在可以允許重複資料的一般索引，在搜尋上會比「HASH」有更好的效率。

註：「FULLTEXT」索引只能用在「CHAR」、「VARCHAR」與「TEXT」型態的欄位，而且表格使用的儲存引擎必須是「MyISAM」。「SPATIAL」索引是「SPATIAL」型態欄位專用的，而且表格使用的儲存引擎必須是「MyISAM」。這兩種索引不會在這裡討論。

### AUTO_INCREMENT

- 自動遞增
- 只有整數型態才可以使用「AUTO_INCREMENT」欄位屬性，你可以根據編號大小的需求，選擇使用「TINYINT」、「SMALLINT」、「MEDIUMINT」、「INT」或「BIGINT」，而且因為只會使用到正數，所以你可以加入「UNSIGNED」來增加編號的範圍
- NOT NULL
- 一個表格只能有一個「AUTO_INCREMENT」欄位，而且要為它建立一個索引，而且通常是建立主索引鍵或唯一索引，這樣可以防止重複的編號；不過MySQL也允許你建立可重複的索引
- 可使用LAST_INSERT_ID()取的最新一筆，但自己指定AUTO_INCREMENT欄位的話這功能會有問題
- 使用「TRUNCATE TABLE」敘述刪除包含「AUTO_INCREMENT」欄位表格的所有紀錄，編號會重新從頭開始。
- 不要指定值，或是指定「NULL」值給「AUTO_INCREMENT」欄位，都可以讓MySQL為你自動編製一個流水號，並儲存到紀錄中，這兩種也是比較好的方式
- 如果編號已經到欄位型態的最大範圍，例如一個「SMALLINT」型態，而且是指定為「UNSIGNED」的「AUTO_INCREMENT」欄位，編號已經到「65535」了，如果再執行新增的敘述，就會造成「Duplicate entry ’65535′ for key ‘欄位名稱’」的錯誤

### 查詢表格與索引資訊

MySQL資料庫在啟動以後，會有一個很特別的資料庫，名稱是「information_schema」，這個資料庫通常會稱為「系統資訊資料庫」。這個資料庫中有一個表格叫作「TABLES」，它儲存所有MySQL資料庫中的表格相關資訊。

## 子查詢

子查詢(subquery)是一種很常見的應用，不論是查詢、新增、修改或刪除都有可能出現。子查詢是一個放在左右刮號中的「SELECT」敘述，而這個查詢敘述會放在另一個SQL敘述中。在執行一些工作的時候，使用子查詢可以簡化SQL敘述。

```sql
SELECT Code, Population
FROM country
WHERE Population > ( SELECT Population
                     FROM   country
                     WHERE  Code = 'USA' )
```

子查詢大部份使用在提供判斷條件用的資料，在「WHERE」和「HAVING」子句中，都可能出現子查詢

### 比較運算子

```sql
WHERE   運算式  ==  (子查詢)
HAVING          <>
                <
                <=
                >
                >= 
```
注意回傳不可超過一個欄位的資料也不能超過一筆以上的資料

### 「IN」運算子

```sql
WHERE   運算式  [NOT]   IN  (子查詢)
HAVING
```
因為是「IN」可以有多筆(但還是只能一欄位)

### 其他運算子

```sql
WHERE   運算式  ==  ALL (子查詢)
HAVING          <>  Any
                <   SOME
                <=
                >
                >= 
```

- 在MySQL中，「ANY」與「SOME」運算子的效果是一樣的
- 「<> ALL」效果其實跟「NOT IN」是一樣的
- 「= ANY」運算子的效果跟「IN」是一樣的

### 多欄位子查詢

多欄位比較

```sql
SELECT Name, GNP
FROM country
WHERE ( Continent, GovernmentForm ) = ( 'Asia', 'Replublic' )
```

搭配子查詢

```sql
SELECT Name
FROM country
WHERE (Region, GovernmentForm) = ( SELECT Region, GovernmentForm
                                   From country
                                   WHERE Name = 'Iraq')
```

`=`同樣可以改成`IN`的方式查詢

### SELECT子句與子查詢

如果需要的話，子查詢也可以使用在「SELECT」子句中

感覺沒啥屁用?

### FROM子句與子查詢

把子查詢的回傳當表格使用，一定要加`AS`

感覺也沒啥屁用

### 資料維護與子查詢

在使用「INSERT」、「UPDATE」與「DELETE」敘述執行新增、修改與刪除資料時，也可以依照需要使用子查詢來簡化資料維護的敘述。

- INSERT:當成塞入的資料
- UPDATE、DELETE:放在WHERE裡當條件
- MySQL的UPDATE、DELETE修改和子查詢不可以出現相同的表格

### 關聯子查詢

在使用子查詢的的時候，通常不會跟外層查詢有直接的關係，也就是子查詢不會使用外層查詢的資料；不過遇到一些比較特殊的需求時，在「WHERE」或「HAVING」子句中的子查詢，也需要使用外層查詢的資料來執行判斷的工作，這樣的敘述稱為「關聯子查詢、correlated subqueries」，使用別名來達成

在「WHERE」或「HAVING」子句中用來設定條件的子查詢，可以依照需求使用像「IN」、「ANY」這些運算子來判斷條件是否符合。除了上列以經討論的比較運算子外，還有一個「EXISTS」運算子，「EXISTS」運算子判斷條件是否成立的依據比較不一樣，如果子查詢有任何紀錄資料回傳，條件就算成立，所以SELECT後面接甚麼都一樣(*, 1, NULL, 'Hello'...)

```sql
WHERE   運算式  [NOT]   EXISTS  (子查詢)
HAVING
```

### 子查詢與結合查詢

有些時候可以使用結合查詢就好

```sql
SELECT Name
FROM city
WHERE ID NOT IN ( SELECT Captial
                  FROM country
                  WHERE Captial IS NOT NULL)
```

先left join再把not null去掉達成跟上面相同效果

```sql
SELECT city.Name, country,Capital
FROM city LEFT JOIN country ON city.ID = country.Capital
WHERE country.Capital IS NULL
```

## Views

如果在資料庫的應用中，出現很常執行的查詢敘述時，你可以在MySQL資料庫中建立一種「View」元件，View元件用來保存一段你指定的查詢敘述。

也有很多人稱「View」元件是一種「虛擬表格」，因為它不是一個真正儲存紀錄資料的表格，可是它又跟表格的用法類似。

```sql
CREATE [OR REPLACE] VIEW 名稱
```

- 如果需要修改一個已經建立好的View元件，你就要加入「OR REPLACE」的設定
- 如果想要查詢一個View元件中會傳回哪些欄位的資料，可以使用「DESCRIBE」或是比較簡短的「DESC」指令

下列是MySQL關於View元件的規定與限制：

- 在同一個資料庫中，View的名稱不可以重複，也不可以跟表格名稱一樣
- View不可以跟Triggers建立聯結

儲存在View中的查詢敘述也有下列的規定：

- 查詢敘述中只能使用到已存在的表格或View
- 「FROM」子句中不可以使用子查詢
- 不可以使用「TEMPORARY」表格
- 不可以使用自行定義的變數、Procedure與Prepared statement參數

解決欄位名稱一樣的方法除了使用別名外，也可以指定View元件欄位名稱：

```sql
CREATE [OR REPLACE] VIEW 名稱
[(欄位名稱[,...])]
AS
查詢敘述
```

- 使用「ALTER VIEW」敘述，可以讓你修改一個已經建立好的View元件
- 如果以修改View元件的工作來說，使用「ALTER VIEW」或「CREATE OR REPLACE VIEW」敘述的效果是完全一樣的。唯一的差異是要修改View元件如果不存在的話，「CREATE OR REPLACE VIEW」敘述會直接建立新的View元件
- 使用「DROP VIEW」刪除一個不需要的View元件
- 對View做「INSERT」、「UPDATE」和「DELETE」都會直接影響來源資料
- 加入「WITH CHECK OPTION」設定的View元件，在執行資料維護工作時，會先執行檢查的工作，規則是一定要符合「View元件中WHERE設定的條件」
- View元件中的「WITH CHECK OPTION」設定，還有額外的「CASCADE」和「LOCAL」兩個控制檢查範圍的設定，檢查範圍設定為「LOCAL」的View元件，在執行資料維護的時候，只會檢查是否符合自己的條件設定；檢查範圍設定為「CASCADE」的View元件，在執行資料維護的時候，就不能違反所有VIew元件的條件設定
- View元件可以提供更方便的資料查詢與維護方式，在你建立View元件的時候，除了指定的查詢敘述要符合規定，還可以指定資料庫執行View元件時所使用的「演算法、algorithm」
- 可使用檢查表格或View元件的敘述「CHECK TABLE」檢查View元件包含的查詢敘述是否正確
- MySQL資料庫在啟動以後，會有一個很特別的資料庫，名稱是「information_schema」，這個資料庫通常會稱為「系統資訊資料庫」。這個資料庫中有一個表格叫作「VIEWS」，它儲存所有MySQL資料庫中View元件的相關資訊

## Prepared Statement

### 使用者變數

MySQL資料庫伺服器提供一種簡易的儲存資料方式，稱為「使用者變數、user variables」。使用者變數儲存一些簡單的資料，例如數字或字串，它們可以在後續的操作中使用。

```sql
SET @變數名稱 {= | := } 值 [ㄡ]
```

select順便設定變數(只能使用`:=`)：

```sql
SELECT @變數名稱 := 值 [,...]
```

### Prepared Statements的應用

如果有「許多要執行的敘述，可是內容卻相似」的情況，可以使用「Prepared statements」改善資料庫的效率。

一般：

檢查->解析->執行->回傳

```sql
SELECT Code, Name, GNP
FROM country
WHERE Code = 'USA'
```

使用prepared statement

檢查->解析->保存

```sql
PREPARE my_country FROM
`SELECT Code, Name, GNP FROM country WHERE Code = ?'
```

執行->回傳

```sql
SET @my_code = 'USA'
EXECUTE my_country USING @my_code
```

建立

```sql
PREPARE 名稱 FROM '敘述`
```

執行

```sql
EXECUTE 名稱 [USING @變數名稱[,...]]
```

刪除

```sql
{ DEALLOCATE | DROP } PREPARE 名稱
```

prepared statement使用?來代表需要的參數，傳入的時候會依照順序帶入，數量不對會產生錯誤；如果傳入的變數不存在會變成NULL。

所有使用者變數與prepared statements都是某一個用戶端專屬的，如果用戶端離線以後，他所設定的使用者變數與prepared statements都會被清除，所以建立prepared statements時，不可以指定它是屬於哪一個資料庫，否則會有錯誤訊息。

## Stored Routines 入門
## Sotred Routines的變數與流程
## Stored Routines進階

```
Triton Ho

路邊小酸酸的信箱收到的問題：
為什麼銀行都喜歡把SQL寫在預存程序內？
除了比較快、比較安全以及不用佈版，卻也失去了彈性，這樣好嗎

一支預存程序處理好所有事情，還是在AP呼叫不同的預存程序好
前者寫得快但難維護，後者慢一些些但可維護性卻提升了不少

———————————————————————————
我反問一句：

如果是大約１５年前，你是銀行的Tech Lead，你會打算用什麼programming language去寫你的business logic？

１　PHP？別鬧好嗎，weak typed language少了compiler便少了多一份額外的檢查。銀行的帳目錯一下都是被政府抓去抱茶的
２　Java？剛出世，還沒成熟
３　ASP .NET？未發明，886
４　ASP：除了MS MVP之外沒人讚好的語言

那時空下，一堆現代的語言根本還未出來／成熟。所以用那時空下相對已經成熟的stored procedure是一個合理選擇。

而銀行系統，一向都是沒事就不會輕易改動／升級的。
（現實問題：有人碰過的source code便有帶入新bug的風險……）
所以，一堆寫下來的stored procedure到今天還沒被替代掉也是很正常的。

最後一句：除了極少數特殊場合，2019的今天不應該再把business logic放在stored procedure內
```

## Triggers

我覺得這東西也不該存在

## 查詢 information_schema

|表格名稱|	說明|
|-|-|
|CHARACTER_SETS|	MySQL資料庫支援的字元集|
|COLLATIONS|	MySQL資料庫支援的collation|
|COLLATION_CHARACTER_SET_APPLICABILITY|	字元集與collation對應資訊|
|COLUMNS|	欄位資訊|
|COLUMN_PRIVILEGES|	欄位授權資訊|
|KEY_COLUMN_USAGE| 索引欄位的限制資訊|
|ENGINES|	MySQL資料庫支援的儲存引擎|
|GLOBAL_STATUS| MySQL資料庫伺服器狀態資訊|
|GLOBAL_VARIABLES|	MySQL資料庫伺服器變數資訊|
|KEY_COLUMN_USAGE|	索引鍵資訊|
|ROUTINES|	Stored routines資訊|
|SCHEMATA|	資料庫資訊|
|SESSION_STATUS|	用戶端連線狀態資訊|
|SESSION_VARIABLES|	用戶端連線變數資訊|
|STATISTICS|	表格索引資訊|
|TABLES|	表格資訊|
|TABLE_CONSTRAINTS|	表格限制資訊|
|TABLE_PRIVILEGES|	表格授權資訊|
|TRIGGERS|	Triggers資訊|
|USER_PRIVILEGES|	使用者授權資訊|
|VIEWS|	Views資訊|

「information_schema」資料庫稱為「database metadata」，包含資料庫元件與伺服器運作的完整資訊都儲存在這個資料庫中。你不須要自己建立與維護「information_schema」資料庫，它是由MySQL資料庫伺服器負責建立與維護的。你只能夠在需要的時候，使用「SELECT」敘述來查詢儲存在裡面的資料。

### SHOW指令

除了使用查詢敘述直接查詢「information_schema」資料庫中的資訊外，MySQL資料庫伺服器供有許多不同用法的「SHOW」指令，同樣可以查詢資料庫資訊。「SHOW」指令是MySQL資料庫伺服器專用的指令，並不是標準的SQL敘述。

查詢MySQL資料庫伺服器中的資料庫資訊：

```sql
SHOW {DATABASES | SCHEMAS} [LIKE '樣版']
```
查詢MySQL資料庫伺服器中的表格資訊：

```sql
SHOW TABLES [FROM 資料庫名稱] [LIKE '樣版']
```

```sql
SHOW TABLES STATUS [FROM 資料庫名稱] [LIKE '樣版' | WHERE 條件]
```

查詢MySQL資料庫伺服器中的欄位資訊：

```sql
SHOW [FULL] COLUMNS FROM 表格名稱 [FROM 資料庫名稱] [LIKE '樣版' | WHERE 條件]
```

查詢MySQL資料庫伺服器中的索引資訊：

```sql
SHOW INDEX FROM 表格名稱 [FROM 資料庫名稱] [WHERE 條件]
```

查詢MySQL資料庫伺服器中的trigger資訊：

```sql
SHOW TRIGGERS [FROM 資料庫名稱] [LIKE '樣版' | WHERE 條件]
```

查詢MySQL資料庫伺服器中的字元集與collation資訊：

```sql
SHOW CHARACTER SET [LIKE '樣版' | WHERE 條件]
SHOW COLLATION [LIKE '樣版' | WHERE 條件]
```

查詢MySQL資料庫伺服器中支援的儲存引擎資訊：

```sql
SHOW [STORAGE] ENGINE
```

查詢MySQL資料庫伺服器狀態與系統變數資訊：

```sql
SHOW [GLOBAL | SESSION] STATUS [LIKE '樣版 | WHERE 條件]
SHOW [GLOBAL | SESSION] VARIABLES [LIKE '樣版 | WHERE 條件]
```

查詢MySQL資料庫伺服器中與字元集相關的變數資訊：

```sql
SHOW GLOBAL VARIABLE LIKE 'character%`
```

### 建立元件資訊

下列的「SHOW」指令語法可以查詢MySQL資料庫伺服器中建立各種元件的詳細資訊：

|指令|	說明|
|-|-|
|SHOW CREATE DATABASE 資料庫名稱|	查詢建立資料庫的詳細資訊|
|SHOW CREATE TABLE 表格名稱|	查詢建立表格的詳細資訊|
|SHOW CREATE FUNCTION 名稱|	查詢建立Function的詳細資訊|
|SHOW CREATE PROCEDURE 名稱|	查詢建立Procedure的詳細資訊|
|SHOW CREATE VIEW 名稱|	查詢建立View的詳細資訊|

### DESCRIBE指令

「DESCRIBE」是MySQL資料庫伺服器提供的特殊指令，並不是標準的SQL敘述。它可以查詢指定表格的欄位資訊：

```sql
{DESCRIBE | DESC} 表格名稱 [欄位名稱 | '樣版']
```

### mysqlshow

MySQL資料庫伺服器提供一個可以在命令提示字元下執行的工具程式「mysqlshow」：

```sql
mysqlshow -h 資料庫伺服器 -u 帳號 -p密碼
```

密碼跟-p之間不能有空格

## 錯誤處理與查詢

MySQL資料庫環境中，可以使用「sql_mode」系統變數設定資料庫對於檢查錯誤資料的「嚴格」程度，分為「strict」與「non-strict」兩種模式。在strict模式下，資料庫會嚴格的檢查與發現錯誤的資料，而且不會儲存錯誤的資料；在non-strict模式下，資料庫同樣會檢查與發現錯誤的資料，不過它會儘量試著處理這些錯誤的資料，再把資料儲存起來。

你可以依照自己的需求設定「sql_mode」系統變數，下列的指令可以設定為「non-strict」模式：

```sql
SET sql_mode = ''
```

下列的敘述設定為「strict」模式：

```sql
SET sql_mode = 'STRICT_TRANS_TABLES'
SET sql_mode = 'STRICT_ALL_TABLES'
```

## Non-Strict模式

```sql
SET [SESSION | GLOBAL] sql_mode = '[設定[,...]]'
```

如果資料庫發現不符合欄位規定的資料，它會儘量試著處理這些錯誤的資料，再把資料儲存起來，然後使用警告訊息通知你。

在non-strict模式運作時，下列幾種情形都有可能會啟動自動修正資料的功能：

- 執行新增或修改敘述，包含INSERT、REPLACE、UPDATE與LOAD DATA INFILE
- 使用ALTER TABLE修改表格的欄位定義
- 在欄位定義中使用「DEFAULT」指定欄位的預設值

## Strict模式與IGNORE關鍵字

你也可以將資料庫設定為「strict」模式，在這個模式下，只有在儲存字串資料到非字串型態的欄位時，資料庫會嘗試幫你指定的字串轉換為欄位型態；其它任何違反資料型態的問題，資料庫不會儲存錯誤的資料，而且會產生錯誤訊息。

在「strict」模式模式下執行新增與修改時，可以依照需求加入「IGNORE」關鍵字執行non-strict模式：

```sql
INSERT [IGNORE] [INTO] 表格名稱 ...
UPDATE [IGNORE] 表格名稱 ...
```

## 其它設定

「sql_mode」變數設定為「non-strict」或「strict」模式後，還可以依照自己的需求加入額外的設定：

|設定值|	說明|
|-|-|
|ALLOW_INVALID_DATES|	允許錯誤的日期資料|
|NO_ZERO_DATE|	不允許全部是0的日期資料|
|NO_ZERO_IN_DATE|	日期資料中不可以有0|
|ERROR_FOR_DIVISION_BY_ZERO|	除以0時產生錯誤，而不是產生NULL值|

有一些default的sql_mode可以設定，例如`MSSQL`, `ORACLE`等。

## 查詢錯誤與警告

在執行SQL敘述後，如果發生警告或錯誤，你可能需要根據這些訊息來執行一些補救工作。MySQL提供的「SHOW」指令可以查詢這些訊息：

```sql
SHOW WARNINGS [LIMIT [忽略數量,] 數量]
SHOW ERRORS [LIMIT [忽略數量,] 數量]
```

如果是因為執行SQL敘述，導致資料庫產生的警告或錯誤，都可以使用「SHOW WARNINGS」或「SHOW ERRORS」查詢；不過也有可能是因為作業系統發生問題，如果發生這類的錯誤，詳細的錯誤訊息要在命令提示字元下，使用「perror」程式來查詢：

```shell
shell> perror [Errorcode]
```

## 匯入與匯出資料

你可以使用SQL敘述或MySQL提供的用戶端程式，執行匯出與匯入的工作。匯出資料可以使用「SELECT INTO OUTFILE」敘述，或是「mysqldump」用戶端程式，它們都可以將指定的資料儲存為檔案保存起來；匯入資料可以使用「LOAD DATA INFILE」敘述，或是「mysqlimport」用戶端程式，它們都可以將指定檔案中的資料新增到資料庫中。

### 使用SQL敘述匯出資料

```sql
SELECT ...
INTO OUTFILE `檔案名稱`
[FIELDS [TERMINATED BY '字串']
        [[OPTIONALLY] ENCLOSED BY '字元']
        [ESCAPED BY '字元']
]
[LINES [STARTING BY '字串']
       [TERMINATED BY '字串']
]
FROM ...
```

- 使用「FIELDS TERMINATED BY」子句設定新的分隔字元
- 使用「FIELDS ESCAPED BY」子句設定新的跳脫字元符號
- 使用「FIELDS ENCLOSED BY」子句可以設定包圍欄位資料的字元符號
- 匯出的資料如果遇到「NULL」值的時候，MySQL會使用「\N」儲存在檔案中
- 使用「LINES STARTING BY」與「TERMINATED BY」子句可以設定每一列資料開始與結束字串
- 可以用上面這些設定兜出CSV格式

### 使用SQL敘述匯入資料

```sql
LOAD DATA [LOCAL]
INFILE '檔案名稱'
[IGNORE | REPLACE]
INTO TABLE 表格名稱
[FIELDS [TERMINATED BY '字串']
        [[OPTIONALLY] ENCLOSED BY '字元']
        [ESCAPED BY '字元']
]
[LINES [STARTING BY '字串']
       [TERMINATED BY '字串']
]
[IGNORE 數值 LINES]
[({欄位名稱 | 使用者變數}[,...])]
[SET (欄位=運算式[,...])]
```

default匯入格式是tab+\N

在新增、修改或匯入資料到資料庫的時候，都有可能發生索引值重複的錯誤，在使用「LOAD DATA INFILE」匯入資料的時候，如果發生索引值重複的情況，你可以使用「IGNORE」或「REPLACE」來決定資料庫該作什麼處理

在執行匯入資料的敘述以後，你應該會想要知道有多少資料匯入到資料庫中。如果你在「MySQL Query Browser」工具中執行「LOAD DATA INFILE」敘述的話，它會告訴你總共影響了幾筆資料，包含新增與修改；如果你在命令提示字元中執行「LOAD DATA INFILE」敘述的話，除了影響的資料數量以外，還會告訴你比較完整的匯入資訊

### 使用mysqldump程式匯出資料

MySQL提供許多不同應用的工具程式，讓你可以在命令提示字元中執行，這些工具程式都是MySQL才有的，而且它們並不是SQL敘述。你可以使用「mysqldump」工具程式匯出資料。

```shell
mysqldump [選項] 資料庫名稱 [表格名稱...]
```

### 使用mysqlimport程式匯入資料

```shell
mysqlimport [選項] 資料庫名稱 檔案名稱[,...]
```

在指定資料檔案的名稱時，要特別注意下列兩個重點：

- 資料檔案中不可以包含SQL敘述
- 檔案名稱會決定匯入資料庫中的哪個表格，MySQL會使用去除附加檔名後的名稱。例如「dept.dat」為「dept」表格；「dept.txt.dat」同樣為「dept」表格

## 效率

### 索引

主索引鍵的應用很常見，而且一個表格通常會有一個，而且只能有一個。在一個表格中，設定為主索引鍵的欄位值不可以重複，而且不可以儲存「NULL」值。因為這樣的限制，所以很適合使用在類似編碼、代號或身份證字號這類欄位。

唯一索引也稱為「不可重複索引」，在一個表格中，設定為唯一索引的欄位值不可以重複，但是可以儲存「NULL」值。這種索引適合用在類似員工資料表格中儲存電子郵件帳號的欄位，因為員工不一定有電子郵件帳號，所以允許儲存「NULL」值，可以每一個員工的電子郵件帳號都不可以重複。

非唯一索引用來增加查詢與維護資料效率的索引。設定為非唯一索引的欄位值可以重複，也可以儲存「NULL」值。

「FULLTEXT」索引只能用在「CHAR」、「VARCHAR」與「TEXT」型態的欄位，而且表格使用的儲存引擎必須是「MyISAM」，一般會稱為「全文檢索」，可以提高搜尋大量文字的效率。

「SPATIAL」索引是「SPATIAL」型態欄位專用的，而且表格使用的儲存引擎必須是「MyISAM」。「FULLTEXT」與「SPATIAL這兩種索引不會在這裡討論。

索引有兩個主要的用途：

- 主索引鍵與唯一索引可以避免重複的資料
- 主索引鍵、唯一索引與非唯一索引都可以增加資料庫的效率

如果想要為了增加效率而建立索引的話，你應該要考慮下列幾點：

- 最重要的，當然是不要建立沒有必要的索引，例如上列討論的情況
- 索引的欄位儘量不要有「NULL」值
- 雖然某個欄位很常使用在「WHERE」、「ORDER BY」或「GROUP BY」子句中，也不一定要建立索引。例如性別欄位的值只有兩種(使用ENUM(‘M’, ‘F’)型態)，建立索引所增加的效率也不多
- 主索引鍵與唯一索引的效率會比非唯一索引好
  
可以只拿部分內容來建立索引，這時候要注意建立索引的欄位值不應該有太多重複的值，可以用DISTINCT來檢查。

### 判斷條件的設定

如果想要查詢一個表格所有的資料，你就不會使用「WHERE」設定查詢條件，那就只能請資料庫讀取表格中所有的資料後傳回來，有沒有索引就不會有效率上的影響。不過如果使用「WHERE」子句設定查詢條件的話，就要儘量使用索引來增加查詢的效率。

如果索引欄位在WHERE條件的函式裡面也會沒有索引效果。

```sql
SELECT *
FROM test2
WHERE YEAR(birthdate) = 1990
```

下面比上面效率好

```sql
SELECT *
FROM test2
WHERE birthdate >= '1990-1-1' AND birthdate <= '1990-12-31'
```

雖然在算術運算的時候給字串MySQL會自動幫你轉換，但會有額外效能耗損，所以也應該避免。

結合查詢是一種很沒有效率的查詢，因為資料庫要比對兩個表格中，結合條件所設定的欄位值，如果資料數量很多的話，這樣的比對工作就會花很多時間。所以你通常會幫結合條件中的欄位建立索引。

在查詢的條件中，如果跟多個欄位的索引有關的話，MySQL會依照索引欄位的順序來決定是否使用索引。

### EXPLAIN與查詢敘述

MySQL資料庫提供「EXMPLIN」指令，可以讓你分析一個查詢敘述。

```sql
EXPLAIN
SELECT * FROM country WHERE GNP < 10000
```

type如果是`ALL`，表示這個查詢發生「full table scan」。
type如股是`const`，表示只有讀取一筆資料。
「possible_keys」是MySQL用來找到資料所使用的索引，NULL表示這個查詢沒有使用索引。

### 資料維護

當你使用「INSERT」、「UPDATE」或「DELETE」敘述執行資料維護的工作時，也要注意效率上的問題。在執行修改或刪除資料的時候，除了要修改或刪除表格中所有的資料以外，你都會加入條件的設定。在「UPDATE」和「DELETE」敘述中使用「WHERE」子句設定條件時，跟查詢時候該注意的地方都一樣，除了儘量使用索引來增加執行的效率，也要避免不必要的資料轉換。

MySQL提供的「EXPLAIN」敘述，只可以為你分析一個查詢敘述，它不可以使用在「SELECT」以外的敘述。不過你可把它改為查詢敘述(`SELECT *`)再「EXPLAIN」。

### LIMIT子句

在查詢和維護資料的時候，都有可能會使用「LIMIT」子句設定查詢或維護資料的數量。「LIMIT」子句在某些應用上是非常方便的，不過要特別注意在效率上的問題。

### 使用暫時表格

如果在查詢工作中，很常使用一個查詢的結果，再加上不同的條件或結合，你就可以考慮使用暫時表格。

### 儲存引擎

MySQL資料庫是一種允許多個用戶端同時使用的資料庫管理系統，在多用戶端的的運作環境下，資料庫就使用「鎖定、Locking」來避免資料的混亂。

MySQL提供的「MyISAM」和「InnoDB」兩種儲存引擎，使用不同的鎖定方式來處理上列的情況。MyISAM使用的是「table-level」的鎖定方式：

- MyISAM儲存引擎使用的「table-level」鎖定方式，適合使用在查詢工作非常多，資料維護比較少的資料庫，這樣的資料庫運作起來的效率會比較好。
- InnoDB儲存引擎使用的是「row-level」的鎖定方式，InnoDB儲存引擎使用的「row-level」鎖定方式，適合使用在查詢與資料維護工作都差不多的資料庫，這樣的資料庫運作起來的效率會比較好。