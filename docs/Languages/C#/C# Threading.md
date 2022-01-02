
# C# 學習筆記

[Huanlin學習筆記](https://www.huanlintalk.com/)
[C#多執行緒相關學習資源](https://blog.miniasp.com/post/2009/03/08/Multi-Threading-in-CSharp-Learning-Resources)

- 執行緒就是用來切割 CPU 執行時間的基本單位
- 代價包括空間（記憶體損耗）與時間（執行效能）。Windows 每建立一條執行緒，須為它配置大約 1MB 左右的記憶體，其中包含執行緒核心物件、環境區塊（Thread Environment Block）、使用者模式堆疊、核心模式堆疊等等。這是記憶體空間的額外負擔
- 執行效能的負擔則主要來自於兩個地方：與 unmanaged DLL 的互動，以及 context switch
- 可以在工作管理員看執行續數量

## 分道揚鑣

相較於執行緒集區這種從一個共用的池子裡面取用執行緒的作法，「自行建立執行緒」則意味著建立新的執行緒來專門負責處理特定工作，所以有時候我們也說這種作法是「建立專屬的執行緒（dedicated thread）」。

當你碰到以下幾種特殊場合，才應該自行建立專屬的執行緒來處理特定的運算工作：

- 你希望某些執行緒擁有特殊優先權。在預設情況下，執行緒集區裡面的執行緒都是「正常」優先權。如果想要讓某執行緒擁有特權，可以個別建立執行緒並修改其優先權。但一般不建議這麼做就是了。
- 你希望某些執行緒以前景執行緒的方式運作，以避免工作還沒完成，應用程式就被使用者或其他程序關閉。執行緒集區裡面的執行緒永遠都是背景執行緒，它們有可能還沒完成任務就被 CLR 結束掉。
- 執行緒所負責的工作需要大量運算，而且需要花很長的時間才能執行完畢。碰到這種情況，自行建立執行緒可能會比使用執行緒集區來的有效率，因為這樣可以省去執行緒集區的一些處理邏輯，例如何時該建立額外的執行緒。
- 執行緒開始工作後，你可能需要在某些情況下提前終止執行緒（透過呼叫 Thread 類別的 Abort 方法）。

建立專屬的執行緒：

```c#
public delegate void ThreadStart();
public delegate void ParameterizedThreadStart(Object obj);
```

除此之外你應該直接使用執行緒集區

- .NET CLR 實作了集區（pool）的概念，讓應用程式可以將完成任務的執行緒丟進集區裡面待命，等到有其他工作需要非同步執行，便可直接從集區取出執行緒，並將工作派給它執行。
- CLR 管理的執行緒集區有兩種： 工作執行緒集區（worker thread pool）和輸入／輸出執行緒集區（I/O thread pool）。兩種集區裡面的執行緒是同樣東西；之所以分成兩個集區，主要是希望應用程式依實際用途來選擇適當的集區，以免經常發生執行緒耗盡（thread starvation）的情形。
- 使用Join來等待thread結束，就不用一直判斷IsAlive
- 執行緒之間可以共享變數，有時候也的確需要這麼做。多條執行緒之間共享同一個變數時，如果都只是讀取變數值，並不會有問題。但如果執行緒會去修改共享變數的值，那就得使用lock
- 依行為來區分，執行緒可分為兩種：前景執行緒和背景執行緒。兩者的主要區別是：當所有的前景執行緒停止時，應用程式就會結束，並且停止所有背景執行緒。

## 優先順序

- Windows 作業系統把執行緒的優先順序分成 32 個等級，編號從最低的 0 至最高的 31，優先權愈高，愈能分到更多 CPU 時間
- 無法精確指定或得知某執行緒究竟何時分配到 CPU，以及分配到多久的時間──這些完全由 Windows 作業系統來控制。我們能控制的，是藉由調整執行緒的優先等級來提高（或降低）執行緒獲得 CPU 資源的機會
- 處理序的優先順序類別有以下六種：
  - 即時（RealTime）
  - 高（High）
  - 高於標準（Above Normal）
  - 標準（Normal）
  - 低於標準（Below Normal）
  - 閒置（Idle）
- 預設的處理序優先順序是「標準」
- 可以利用 Windows 工作管理員來手動調整特定處理序的優先順序
- 決定為應用程式指定哪一種優先順序類別之後，接著要考慮的是應用程式中的執行緒。Windows 提供七種執行緒優先順序：閒置（Idle）、最低（Lowest）、低於正常（Below Normal）、正常（Normal）、高於正常（Above Normal）、最高（Highest）、時間緊迫（Time-Critical）
- 六種處理序優先順序類別搭配七種執行緒優先順序，便能決定執行緒最終的優先等級

## 執行緒集區與 Execution Context

- 當一個 CLR 初始化的時候，它的執行緒集區是空的，裡面沒有任何執行緒。執行緒集區本身有一個工作請求佇列，每當應用程式需要非同步操作時，便可呼叫一些方法來將工作請求送進這個佇列。CLR 會從佇列中逐一取出請求（先到先服務），並查看集區裡面有沒有閒置的執行緒。剛開始當然沒有，於是 CLR 會建立一條新的執行緒來負責執行任務。等到任務執行完畢，CLR 並不摧毀那個執行緒，而是將它放回執行緒集區，等待下一次任務指派。如此一來，就如前面所說，執行緒能夠重複使用，從而減少了反覆建立和摧毀執行緒所產生的效能損耗。
- 另一方面，放回執行緒集區中待命的執行緒在閒置一段時間之後若未再接到新任務，就會自動摧毀，以便將記憶體資源釋放出來。除了摧毀閒置的執行緒，CLR 還有一套演算法（爬山演算法hill-climbing algorithm），會根據系統目前擁有的運算資源（CPU 核心的數量）和應用程式的負載等狀況來決定是否要建立更多執行緒來處理應用程式提出的工作請求。

```c#
static Boolean QueueUserWorkItem(WaitCallback callBack);
static Boolean QueueUserWorkItem(WaitCallback callBack, Object state);
```

- 從 .NET Framework 4 開始，我們還可以利用 Task.Run 方法來使用集區中的執行緒
- 執行緒在執行任務時會需要記住一些相關資訊，我們用一個專有名詞來統稱這些資訊，叫做執行環境（execution context）
- 而用來放置環境資訊的稱作 TLS（Thread Local Storage）
- 下游執行續可以可以得到上游執行續的相關資訊這是因為CLR預設會幫我門把上游執行續的資訊往下傳
- 如果你發現下游根本不需要上游的那些環境資訊，此時就可以利用 ExecutionContext 類別的 SuppressFlow 方法來關閉此預設機制，以減輕效能損耗(不是很重要)

## 工作の取消和逾時

- 主要是透過 System.Threading 命名空間裡的兩個類別來達成：CancellationTokenSource 和 CancellationToken
- 任何工作執行緒只要能夠拿到上游建立的 CancellationTokenSource 物件，就能夠呼叫其 Cancel 方法來取消工作。
- 和 CancellationTokenSource 類別比起來，CancellationToken 較輕盈、功能也較少（無法取消工作）；而且從型別名稱也可以看得出來，其目的就是單純用來當作取消工作的旗號。
- CancellationToken 有一個靜態屬性叫作 None，它會傳回一個 CancellationToken 結構的執行個體，而且已經預先設定成無法取消，所以適合用在不需要取消工作的場合。
- CancellationToken 的 Register 方法提供了訂閱「工作取消」的窗口。也就是說，你可以利用此方法註冊一個或多個回呼函式，以便在工作取消時收到通知。
- 預設情況下，如果 Register 方法所註冊的回呼函式拋出異常（exception），此異常會先被暫存起來，等到其他回呼函式都全部執行完畢，之後才一口氣把先前捕捉到的異常拋出來。(但此行為可以更改)
- CancellationTokenSource 還有提供逾時自動取消工作的功能，讓我們可以預先設定一個逾時時間，若工作執行緒在這個限定時間內還沒跑完，就自動取消工作。

## Task Parallel Library(TPL)

```c#
static void Main(string[] args)
{
    // 寫法 1 - .NET 2 開始提供
    ThreadPool.QueueUserWorkItem(state => MyTask());

    // 寫法 2 - .NET 4 開始提供 Task 類別。
    var t = new Task(MyTask);   // 等同於 new Task(new Action(MyTask));
    t.Start();

    // 寫法 3 - 也可以用靜態方法直接建立並開始執行工作。
    Task.Factory.StartNew(MyTask);

    // 寫法 4 - .NET 4.5 的 Task 類別新增了靜態方法 Run。
    Task.Run(() => MyTask());

    Console.ReadLine();
}

static void MyTask()
{
    Console.WriteLine("工作執行緒 #{0}", Thread.CurrentThread.ManagedThreadId);
}
```

- QueueUserWorkItem的另外一種做法，比較簡潔
- StartNew有16種多載，Run是新版本只有八種比較簡單
- StartNew的Action<Object>是逆變型（contravariant）參數，一定只能是Object
- 如果 Task 物件並未開始執行（未曾呼叫其 Start 方法）就呼叫了 Wait 方法，視情況而定，有可能會執行非同步工作並立即返回，也有可能產生奇怪的狀況。

## 你的非同步程式設計觀念正確嗎

使用專屬執行緒的時機

當你碰到以下幾種特殊場合，才應該考慮使用 new Thread() 這種建立專屬執行緒的方式來處理非同步工作：
欲執行的工作需要花很長的時間才能執行完畢（例如一個小時以上）。碰到這種情況，自行建立專屬的執行緒通常會比使用執行緒集區來得有效率。
你希望某些執行緒擁有特殊優先權。預設情況下，執行緒的優先權是「正常」等級。如果想要讓某執行緒擁有特權，則可以個別建立執行緒並修改其優先權。但一般不建議這麼做就是了。
你希望某些執行緒以前景執行緒的方式運作，以避免工作還沒完成，應用程式就被使用者或其他程序關閉。執行緒集區裡面的執行緒永遠都是背景執行緒，它們有可能還沒完成任務就被 CLR 結束掉。
執行緒開始工作後，你可能需要在某些情況下提前終止執行緒（透過呼叫 Thread 類別的 Abort 方法）。

小結

大致可歸納出這樣的建議：優先考慮 Task-base Asynchronous Pattern（TAP）寫法（Task、async、await），然後第二順位是建立專屬執行緒（new 一個 System.Threading.Thread）抑或使用ThreadPool。至於 APM 和 EAP 等舊式寫法，則儘量少用。

## async 與 await

1. 在宣告方法時，前面加上關鍵字 async，表示這是一個非同步方法，裡面會用到 await 關鍵字。
2. 以 async 關鍵字宣告的方法若有回傳值，回傳的型別須以泛型 Task 表示。原先同步版本的方法是傳回 string，故此處改為 Task。非同步方法若不需要傳回值，則回傳型別應寫成 Task，而不要寫 void（原因後述）。
3. 一般而言，非同步方法的名稱會以「Async」結尾，所以方法名稱現在改為 MyDownloadPageAsync。
4. 用 Task 的 Result 屬性來取得非同步工作的執行結果，而不是寫成 await task。
5. 用到 await 的函式都必須在宣告時加上 async 關鍵字，否則無法通過編譯。

- 使用了關鍵字 await 的地方會暫且記住 await 敘述所在的程式碼位置，並且令程式控制流程立刻返回呼叫端；等到 await 所等待的那個非同步工作執行完畢，控制流才又會切回來繼續執行剛才保留而未執行的程式碼。
- 「讀取 Task 物件的 Result 屬性」則是阻斷式（blocking）操作，也就是說，它會令當前的執行緒暫停，直到欲等待的工作執行完畢並傳回結果之後，才繼續往下執行。
- 在宣告方法時加上關鍵字 async，即表示它是個非同步方法。其實 async 的作用也真的就只有一個，那就是告訴編譯器：「這是個非同步方法，裡面可以、而且應該要使用關鍵字 await 來等待非同步工作的結果。」

>錯誤觀念：程式執行時，一旦進入以 async 關鍵字修飾的方法，就會執行在另一條工作執行緒上面。

請注意，程式的控制流一開始進入非同步方法時，仍是以同步的方式執行，而且是執行於呼叫端所在的執行緒；直到碰到 await 敘述，控制流才會一分為二。，await 之前的程式碼是一個同步執行的程式區塊，而 await 敘述之後的程式碼則為另一個同步執行的程式區塊；兩者分屬不同的控制流。前者即為本章開頭提到的先導工作，後者則是延續的工作——它會在 await 所等待的工作完成之後接著執行。

>錯誤觀念：await 會阻斷當前所在的程式碼，直到欲等待的工作完成時才會繼續執行。

一個以`async`關鍵字修飾的非同步方法裡面可以有一個或多個`await`敘述。按照先前的講法，若非同步方法中有兩個`await`敘述，即可以理解為該方法被切成三個控制流（三個各自同步執行的程式區塊）。若非同步方法中三個`await`敘述，則表示該方法被切成四個控制流。依此類推。

碰到 await 時，控制流會立刻返回呼叫端，而 await 之後的程式碼則會暫時保留，直到 await 所等待的非同步工作完成後，才會從剛才暫時保留的地方恢復，繼續執行後面的程式碼（即被 await 切開的後半部分）。

在回傳值的部分，非同步方法的回傳型別可以是下列三種寫法：

1. Task： 適用於非同步工作沒有傳回值的場合。
2. Task<T> ：適用於非同步工作有傳回值的場合。回傳值的型別帶入泛型參數 T，例如 Task<String>。
3. void：雖然能夠通過編譯，但此寫法應該只用於事件處理常式的場合，而不該用來宣告沒有回傳值的非同步工作。

無回傳值的非同步方法不應宣告為 async void 的一個重要原因，是這種寫法會令呼叫端捕捉不到非同步方法所拋出的異常。

以 async 關鍵字修飾的方法，其傳入參數有個規則：不可使用 ref 或 out 修飾詞。

## Resources

[C# 學習筆記：多執行緒 (1) - 從零開始](https://www.huanlintalk.com/2013/04/csharp-notes-multithreading-1.html)
[https://www.huanlintalk.com/2015/01/things-you-know-about-asynchronous.html](https://www.huanlintalk.com/2015/01/things-you-know-about-asynchronous.html)
[以 C# 撰寫多執行緒 (Multi-threading) 相關學習資源整理](https://blog.miniasp.com/post/2009/03/08/Multi-Threading-in-CSharp-Learning-Resources.aspx)
