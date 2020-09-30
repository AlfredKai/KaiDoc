# C Sharp

## MySql in Sharp

## 要先裝MySql.Data

![mysql](/_assets/ccharp_mysql.png)

## 取table names

```c#
private static List<string> GetTables(string dataBase, MySqlConnection conn)
{
    List<string> tableNames = new List<String>();
    string query = "show tables from " + dataBase;
    MySqlCommand command = new MySqlCommand(query, conn);
    using (MySqlDataReader reader = command.ExecuteReader())
    {
        while (reader.Read())
        {
            tableNames.Add(reader.GetString(0));
        }
    }
    //foreach (var t in tableNames)
    //    Console.WriteLine(t);

    return tableNames;
}
```

## Dapper

有一個DB長這樣

![db](/_assets/dapper_sample.png)

只要先這樣

```c#
using Dapper;
```

再這樣

```c#
public class hostlist
{
    public int? MallId { get; set; }
    public string Host { get; set; }
    public string MallName { get; set; }
}
```

然後這樣

```c#
using Dapper;
using MySql.Data.MySqlClient;
```

```c#
string constring = "server=n126;user=root;pwd=scupio;database=bwse_cloud;";
List<hostlist> hlist = new List<hostlist>();
using (MySqlConnection conn = new MySqlConnection(constring))
{
    conn.Open();
    hlist = conn.Query<hostlist>(@"SELECT * FROM hostlist").ToList();
}
```

就能讀取了，94這麼簡單。  
注意資料class的member需要是property且query內不能加上’會出錯(ex: " SELECT * FROM ‘hostlist’ ”)

不預先定義class而使用`dynamic`取得資料

```c#
dynamic data;
using (var connectionhost = new MySqlConnection(mysqlconnection))
{
    connectionhost.Open();
    data = connectionhost.Query<dynamic>(@"SELECT createTime, actType, versionid FROM versionhistory WHERE createtime = (SELECT MAX(createtime) FROM versionhistory)").First();
}

lastindextime = data.createTime;
isRebuild = data.actType;
```

注意reflaction使用的是你query string的字串，所以兩者要對得上，例如`SELECT createTime,...`結果後面變成`lastindextime = data.createtime;`就會錯誤。

## 檔案讀取

### 執行路徑

```c#
AppDomain.CurrentDomain.BaseDirectory
```

or

```c#
Environment.CurrentDirectory
```

### 路徑不存在則建立

```c#
string dir = Path.Combine(AppDomain.CurrentDomain.BaseDirectory, "HotTermsList");
if (Directory.Exists(dir) == false)
    Directory.CreateDirectory(dir);
```

### 取特定檔名檔案

```c#
FileInfo[] files = new DirectoryInfo(System.Environment.CurrentDirectory).GetFiles();
foreach (FileInfo fi in files)
{
    Console.WriteLine("N: " + fi.Name);

    if (Regex.IsMatch(fi.Name, "NewBehaviorResult"))
    {
        Console.WriteLine("Match!" + fi.FullName);
    }
}
```

## 網路芳鄰

```c#
Minimod.Impersonator.Impersonator

private class FileAccessDir
{
    public string FullPath;
    public string loginUser;
    public string loginPassword;
    public string loginDomain;

    public FileAccessDirImp DoImpersonate()
    {
        var r = new FileAccessDirImp();

        if (string.IsNullOrEmpty(loginPassword) == false)
            r.imp = new Minimod.Impersonator.Impersonator(loginUser, loginDomain, loginPassword);

        return r;
    }
}

public class FileAccessDirImp : IDisposable
{
    public Minimod.Impersonator.Impersonator imp = null;

    public void Dispose()
    {
        if (imp != null)
        {
            imp.Dispose();
            imp = null;
        }
    }
}
```

## Nlog

安裝

- Nlog
- Nlog.Config

要同時輸出至Console，在config加上target與rule：

```xml
<target xsi:type="Console" name="console" layout="${message}"  />
<logger name="*" minlevel="Debug" writeTo="console" />
```

加入顯示timestamp的layout:

```xml
<variable name="msgd" value="${longdate} - ${message}"/>
```

記得layout要改成下變數

```xml
<target xsi:type="Console" name="console" layout="${msgd}"  />
```

## System.ServiceModel Configuration

```xml
<configuration>  
    <system.serviceModel>  
        <bindings>  
        </bindings>  
        <services>  
        </services>  
        <behaviors>  
        </behaviors>  
        <client>
        </client>
    </system.serviceModel>  
</configuration>  
```

- services和client可以設定各種endpoint
- Endpoints provide the clients access to the functionality that a WCF service offers
  - address:indicates where to find the endpoint
  - binding:specifies how a client can communicate with the endpoint
  - contract:identifies the methods available(其實重點只有這個，contract就是對應的程式端，出現mutiple endpoint error先檢查這個是不是重複)
- binding用來設定傳輸(HTTP, TCP, pipes, Message Queuing)和協定(Security, Reliability, Transaction flows)，分為自訂的跟預設的，預設參考：<https://docs.microsoft.com/en-us/dotnet/framework/wcf/system-provided-bindings>
- bindingconfiguration對應binding裡面的name

## Configs

### User vs Application scope

偷換conifg value：

```c#
BWStorage.MasterSettings.Default.PropertyValues["DB_ConnectionString"].PropertyValue = DBManager.Read_DBConnectionStringWithDataBase("bwstorageoption");
```
