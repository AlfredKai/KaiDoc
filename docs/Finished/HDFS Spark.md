# HDFS Spark

## Spark RDD

「請問binary tree中文是什麼？」
面試者一臉懵逼
「請問binary tree複雜度是多少？」
現在不只面試者懵逼，一旁協助面試的我也懵逼，這題連我也不會

我今天終於知道公司是如何被外界視為獨角獸企業的

## Superset

hdfs->hive->presto->superset

## 運算、資料與儲存格式

- 甚麼是data locality?例如matrix如何存在memory
- Data locality會影響你的程式效能，真的要tune的話甚至會考慮到一個cache line的大小來寫程式
- 平行在這裡主要考慮怎麼切分data
- 可對data切，也可對loop切
- 大小很重要，通常越小越好，可是太小會有communication和data locality問題
- 最重要的兩個問題：
  - Even workload distribution
  - Proper granularity(Communication Reduction)
- Process互相溝通可以靠Serialize/Deserialize
- Apache Arrow => zero copy，統一各程式語言的memory格式，沒有複製的overhead，直接讀memory就好
- partition:切資料，只要我需要的data，不用一次取全部

## Hadoop

使用 general purposes 的機器來做到這件事情

- 就是個file system
- 可以用便宜的機器來做儲存機器
- Scalability: 可以 horizontally 的 scale up 資料的儲存空間與 throughput
- 分散到不同機器的好處:可以有 high throughput,且善用每顆硬碟各自的 IOPS
- Fault tolerance:簡單將資料 replica 成 3 份 (default value)
  1. 一臺機器 / 一顆硬碟掛的時候恢復的gal度會比 raid 快 (raid 5 會很慢)
  2. 即使某幾個掛了 service 依然還可以使用 (raid 則要下線修復)

重點是和 Spark 的相容性好

- Spark 內建就和 HDFS 相容 (其他 issue 都有得再討論,但這點是主要的選擇)
- 因爲 general purposes 的關系,儲存資料的機器也可以拿來做運算
- Data locality 佳,做運算的機器很大機會可以直接拿到本機的資料而不用依靠網路的傳輸

## Spark

- RDD的主要特色是fault tolerance，當某台executer掛了可以再去叫其他executer跑
- Spark可以把他想像成管理分散式資料的程式，讓你在使用的時候沒有分散式的感覺
- dataframe就是有schema的RDD，更高階
- PySpark主要是操作RDD跟dataframe，dataframe能做的事就盡量使用dataframe
- UDF在RDD比較好寫，dataframe不好寫
- `df.rdd`有performance issue
