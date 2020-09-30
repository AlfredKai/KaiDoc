# WCF

## 強大快速入門教學

<https://msdn.microsoft.com/zh-tw/library/ms734712(v=vs.110).aspx>

其中”HOW TO：裝載和執行基本 Windows Communication Foundation 服務”
六個步驟，如果使用Visual Studio自動生成WCF服務  
Step1, Step3, Step4可省略剩下

```c#
ServiceHost selfHost = new ServiceHost(typeof(YourHostType));
selfHost.Open();
selfHost.Close();
```

非常無腦清爽

## 快速建立用戶端

在server端的config裡確認wcf service的address，即可如同webservice一樣用Add Service Reference的方式增加(不知道有沒有更方便的方式)

## 測試WCF Host

你需要的工具就只有

    System.Console.ReadLine();

幫你暫停程式，讓另外一隻程式確認是否可以連線  
小地雷：

![pic](https://d2mxuefqeaa7sj.cloudfront.net/s_C743CEBBEF6BC60994BB10D5ED77ABA796E5B5C10F5737164CF3A75354EF505E_1513566558426_image.png)

如果Output type是**Windows Application**，Console.ReadLine()會無效

## 在Server取得host物件

如果想在server端取得host物件，最好方法大概就是把該物件丟進去初始化，ServiceHost有兩種初始化方式，一種使用類別型別(typeof(Class))另外一種就是把object丟進去，但使用object初始化必須將InstanceContextMode設成Single，這個在該物件的class使用attribute設定

```c#
[ServiceBehavior(InstanceContextMode = InstanceContextMode.Single)]
class LogCheckerWCFHost : ILogChecker
{
}
```

<https://msdn.microsoft.com/zh-tw/library/system.servicemodel.servicebehaviorattribute.instancecontextmode(v=vs.110).aspx>

## 傳遞自訂型別

使用protobuffer

<http://blog.darkthread.net/post-2015-09-09-wcf-survey-8.aspx>

注意如果enum套上

```c#
[DataContract, Serializable, ProtoContract]
```

會變成string…

## 其他

如果在WCF host type本身open host會導致這個錯誤

```c#
{"為了使用其中一個接受服務例項的 ServiceHost 建構函式，服務的 InstanceContextMode 必須設為 InstanceContextMode.Single。這可經由 ServiceBehaviorAttribute 來設定。否則，請考慮使用接受 Type 引數的 ServiceHost 建構函式。"}
```
