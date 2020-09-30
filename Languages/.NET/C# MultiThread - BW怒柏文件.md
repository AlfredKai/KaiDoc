# C# MultiThread - BW怒柏文件

## Problems

1. 問題很難重現, 超難 debug, 所以建議大家盡量別踩這種雷, 不過, 如果是個小 exe 檔案, 絕對沒差.
2. 雖然很難重現, 但在 EC 系統的 20kqps 狀況下, 重現機率提高不少!

## GuideLines

1. 沒有特別說明 ThreadSafe 的 functions or objects 建議想成 no threadsafe.
   - MD5 object functions is not threadsafe, but MD5.create is threadsafe
   - random object is not threadsafe.
   - 有特別需求可以參考下面手法來處理

```c#
public static class ThreadSafeRandom
{
    private static Random _global = new Random();

    [ThreadStatic]
    private static Lazy<Random> _localrandom = new Lazy<Random>(()=> new Random());

    public static Random localRandom
    {
        get
        {
            if (_localrandom == null)
            {
                int seed;
                lock (_global)
                    seed = _global.Next();
                _localrandom = new Random(seed);
            }
            return _localrandom;
        }
    }

    public static double NextDouble()
    {
        return localRandom.NextDouble();
    }

    public static int Next(int maxValue)
    {
        return localRandom.Value.Next(maxValue);
    }

    public static int Next()
    {
        return localRandom.Next();
    }
}
```

1. .NET 有許多 Concurrent Library 可以使用, such as ConcurrentDictionary , 建議使用這些而不是 靠 Lock Dictionary , Concurrent Library 效能當然會略遜於一般 Library without Lock, 但是會遠快過 Lock 機制 , 但真的不行就 lock 吧! 至少不會出包…
2. 可以利用 IBM 的 no lock pattern [ref](https://www.ibm.com/developerworks/library/j-dcl/index.html) 來避免使用 Concurrent Library , 因為 .NET runtime 當中的每一個 object assignment 基本上是 atomic operation (雖然有提供 interlocked.exchange) , 所以可以使用 Dictionary object 給 multiple thread “READ ONLY” , 要更改的時候則是先生成一個新的 Dictionary Object, 然後把內容都填好後, 直接取代原本的 Dictionary Object.  更改 example:

```c#
var newobj = new Dictionary<key,value>(shared_obj)
// do newobj.add
// do newobj.remove
shared_obj = newobj; // replace shared_obj
```

不過要注意若有讀取的 Thread 寫法是

```c#
if (dict.ContainsKey(x)) { var y = dict[x];}
```

這樣的話會有問題, 也就是 dict.ContainsKey 跟 dict[x] 可能會是兩個不同的 dict , 解決方法是每次呼叫 dict 的 function 當下只能代表該 dict 而已, 也就是利用 dict.TryGetValue 來查詢並取得, 另一個方法是先把共用的 object 取回到 local thread 使用的 object, 類似這樣手法:

```c#
var local_obj=shared_obj;
if (local_obj.ContainsKey(x)) {
 var y = local_obj[x]; // 當然此時也只支援 READ ONLY
}
```

1. Task 比 Thread 好, 但唯一缺點是不好中斷. Thread 由於是 OS 管理, 可以 abort(), 因而 raise threadabort exception. Task 只能靠 Cancellation Token 來中斷.
2. Task 需要更多的知識.
   - 一般而言是用系統唯一的一個 ThreadPool 中的 Thread 來跑 Task , 所以若是 Task 跑太久, 很容易造成 ThreadPool 乾枯… 雖然 ThreadPool 會因為用完而生出新的 Thread, 但這需要時間, 偶而會高達 5 seconds or 1 mins, 對於某些情境使用下面, 會讓人崩潰 (也就是, 你的 Task 當中的工作有可能會先被 delay 5 seconds or 1 mins later 才開始執行)

因為系統只有一個 ThreadPool (這個在新的 .NET Runtime 當中可能已被解決)
所以榨乾 ThreadPool 可能會有許多詭異現象發生 (比如原本運作順暢的系統 functions 突然會卡卡)

所以建議

- 預計會超過 50 ~ 100 ms 執行期間的工作, 選用 TaskCreateOptions.LongRunning , 這是因為一個 LongRunning 其實就是跑一個新的 Thread , 而不是選用 ThreadPool 來執行, 差異為生成/回收一個 Thread 需要耗費系統資源, 通常為 10 ~ 20 ms, 如果為了一個 2 ms 的工作而跑 LongRunning, 會變成總共耗時 12 ~ 22 ms 之類.
- 所有執行中的 Task 依舊是 Thread! (若排隊等待開始的就不算) 除非使用 async/await 才有辦法歸還出 Thread 給其他 Task 用, 所以, 系統若同時跑 10000 個 Task (running 跟 new 出來不一樣歐!), 沒有使用 async/await 的話, 就會有 10000 個 Thread, 此時 ContextSwitch 對於系統效能來說耗費很大. 要避免這樣的狀況已知有幾種手法: 1. 使用 async/await , 2. 使用少數 Task 來跑這些 10000 個 Task.
- Task 當中使用 Thread.Sleep 其實就是霸占該 Thread 不給其他人用, 建議可以利用 CancellationToken.WaitHandler.WaitOne(TimeSpan) 來取代, 雖然依舊是霸占 Thread, 但至少外面的人可以利用 Cancellation Token 來中斷它, 要避免霸佔 Thread 只能靠 async/await
- 盡量思考 CancellationToken 在你 Task 執行中所展現的意義.
- IIS 當中的 Task 狀況可能會不如想像中的停止, 一般而言 Task 都是 Background Thread, 也就是 Process 中斷時, 隨之結束. 但是 IIS 會用一個 process (w3wp) 跑多個 Application Domain , 而在 Application Domain shutdown 時, 可能因為 Task 無法中斷而阻止回收, 所以建議 IIS 當中的 Task 都要能在 application_end 的時候想辦法中斷停止. 不然就只能等到 w3wp 換新的才有辦法停止了.
- 考慮使用 ThreadRecyclingScheduler , 算是生 Thread 比較快的 Task Scheduler

```c#
public class ThreadRecyclingScheduler : TaskScheduler
   {
       public static NLog.Logger logger = NLog.LogManager.GetCurrentClassLogger();
       public static readonly ThreadRecyclingScheduler instance = new ThreadRecyclingScheduler();

       public static new TaskScheduler Default
       {
           get { return instance; }
       }

       public class RecyclingThreadObj
       {
           public Thread thread { get; set; }

           /// <summary>
           /// when task is null means idling.
           /// </summary>
           public Task task { get; set; }

           /// <summary>
           /// when idling, thread will wait this singal
           /// </summary>
           public EventWaitHandle eventwait { get; set; }

           public int runnedTasks = 0;
       }

       public int MaxIdleThreadCount = Environment.ProcessorCount - 2;

       protected ConcurrentBag<RecyclingThreadObj> IdlingThreads;

       public int PeakIdlingThreadCount = 0;

       public List<RecyclingThreadObj> GetIldingThreads()
       {
           return IdlingThreads.ToList();
       }

       public ThreadRecyclingScheduler(int maxIdleThreadCount = -1)
       {
           if (maxIdleThreadCount != -1)
               MaxIdleThreadCount = maxIdleThreadCount;

           IdlingThreads = new ConcurrentBag<RecyclingThreadObj>();

           // monitor too many idling threads.
           var t = new Thread(() =>
           {
               while (true)
               {
                   //logger.Info("peak idling thread is " + PeakIdlingThreadCount);
                   //var idlingTasks = ThreadRecyclingScheduler.instance.GetIldingThreads();
                   //logger.Info("current idling threads :" + idlingTasks.Count);
                   //logger.Info("runnedtasks :" + idlingTasks.Sum(tt => tt.runnedTasks));

                   while (IdlingThreads.Count > MaxIdleThreadCount)
                   {
                       if (PeakIdlingThreadCount < IdlingThreads.Count)
                           PeakIdlingThreadCount = IdlingThreads.Count;

                       RecyclingThreadObj obj;
                       if (IdlingThreads.TryTake(out obj))
                       {
                           obj.thread.Abort();
                           obj.thread.Join();
                           obj.eventwait.Dispose();
                       }
                   }
                   Thread.Sleep(TimeSpan.FromSeconds(10));
               }
           });
           t.IsBackground = true;
           t.Start();
       }

       protected override IEnumerable<Task> GetScheduledTasks()
       {
           // never queue tasks.
           yield break;
       }

       protected override void QueueTask(Task task)
       {
           RecyclingThreadObj obj;
           if (IdlingThreads.TryTake(out obj))
           {
               obj.task = task;
               obj.eventwait.Set();
               return;
           }

           obj = new RecyclingThreadObj();
           obj.task = task;
           obj.eventwait = new EventWaitHandle(true, EventResetMode.AutoReset);
           obj.thread = new Thread(() =>
           {
               while (true)
               {
                   try
                   {
                       if (obj.task != null)
                       {
                           TryExecuteTask(obj.task);
                           obj.runnedTasks++;
                           // we are idling.
                           obj.task = null;
                           IdlingThreads.Add(obj);
                       }
                       obj.eventwait.WaitOne();
                   }
                   catch (ThreadAbortException)
                   { return; }
                   catch (Exception) { }
               }
           });
           obj.thread.IsBackground = true;
           obj.thread.Start();
       }

       protected override bool TryExecuteTaskInline(Task task, bool taskWasPreviouslyQueued)
       {
           return TryExecuteTask(task);
       }
   }
```

## Interlock.Increment 問題

EventToken 原本的寫法

```c#
private static int ClockSeq = new Random().Next(65535);

/// <summary>
/// get 2 bytes clock seq and variant for the host
/// variant is
///    Msb0  Msb1  Msb2  Description
///     0     x     x    Reserved, NCS backward compatibility.
///     1     0     x    The variant specified in this document.
///     1     1     0    Reserved, Microsoft Corporation backward compatibility
///     1     1     1    Reserved for future definition.
/// so we use 10x as the variant,
/// and clock seq is a random part when host booted, we use static instead.
/// </summary>
/// <returns>Incremental sequence number in bytes</returns>
public static byte[] GetClockSeqAndVariant()
{
    Interlocked.Increment(ref ClockSeq);
    byte[] r = BitConverter.GetBytes(ClockSeq).Take(2).ToArray();
    r[1] = (byte)((r[1] & 0x3f) | 0x80);
    return r;
}
```

結果還是有發生 Duplicate Token 的問題,  (上述的作法只是確保了 ClockSeq counter 的數字是對的)  
原來 Interlocked.Increment 雖然是確保了 atomic operation, 但之後取用 ClockSeq 當然還是會有重複的問題啊~ 也就是說, 可能會有兩個 thread 同時呼叫 GetClockSeqAndVariant() , 而同時執行了

```c#
byte[] r = BitConverter.GetBytes(ClockSeq).Take(2).ToArray();
```

這行程式, 導致取用到同一個 ClockSeq …  
所以應該改為

```c#
 private static int ClockSeq = new Random().Next(65535);

   /// <summary>
   /// get 2 bytes clock seq and variant for the host
   /// variant is
   ///    Msb0  Msb1  Msb2  Description
   ///     0     x     x    Reserved, NCS backward compatibility.
   ///     1     0     x    The variant specified in this document.
   ///     1     1     0    Reserved, Microsoft Corporation backward compatibility
   ///     1     1     1    Reserved for future definition.
   /// so we use 10x as the variant,
   /// and clock seq is a random part when host booted, we use static instead.
   /// </summary>
   /// <returns>Incremental sequence number in bytes</returns>
   public static byte[] GetClockSeqAndVariant()
   {
       // atomic operation 需要取用回傳值而不是 clock seq
       var id = Interlocked.Increment(ref ClockSeq);
       byte[] r = BitConverter.GetBytes(id).Take(2).ToArray();
       r[1] = (byte)((r[1] & 0x3f) | 0x80);
       return r;
   }
```

才能解決問題.

### ConcurrentDictionary GetOrAdd

ConcurrentDictionary 有兩個 GetOrAdd 的 overload

1. GetOrAdd(TKey key, Func valueFactory);
2. GetOrAdd(TKey key, TValue value);

選項2. 在 read 的時候就會去拿 lock，因此效能較差
選項1. 會先 call lockfree 的 TryGetValue，如果沒拿到 key 才會去拿 lock ，call valueFactory 將 value 加到 dictionary

因此在大量 concurrent 的狀況下，使用 選項2. 是不建議的!!

which is KafkeHelper 現在的寫法 ...在寫每筆 log 的時候都會去拿一次 lock<http://gitlab.local.bridgewell.com/shutong/kafkahelper/blob/master/KafkaHelper/KafkaProducer.cs#L147>

refs : <https://basildoncoder.com/blog/concurrentdictionary-getoradd-vs.html> (edited)
Basildon Coder
