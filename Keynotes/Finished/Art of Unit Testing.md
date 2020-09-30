# Art of Unit Testing

## About this book

為什麼寫這本書

>To truly learn something, teach it.

永遠的詛咒

>The curse is that the more experience you have, the more stupid you feel.

第二版對Unit test的新體悟

>There are parts of the first edition that today I do not agree with—for example, that a *unit* refers to a method. That’s not true at all. A unit is a unit of work, as I discuss in chapter 1 of this second edition. It can be as small as a method, or as big as several classes (possibly assemblies)

### What’s new in the second edition

>I no longer use RhinoMocks. Stay away from it. It is dead. At least for now. I use NSubstitute for examples of Isolation Framework Basics, and I also recommend FakeItEasy. I am still not crazy about MOQ, for reasons detailed in chapter 6.

不要再使用property setter來做DI

>There are plenty of design changes in the code I show in the book. Mostly I stopped using property setters and am mostly using constructor injection.

### Who should read this book

>The book is for anyone who writes code and is interested in learning best practices for unit testing. All the examples are written in C# using Visual Studio, so .NET developers will find the examples particularly useful. But the lessons I teach apply equally to most, if not all, object-oriented and statically typed languages (VB.NET, Java, and C++, to name a few). If you’re an architect, developer, team lead, QA engineer (who writes code), or novice programmer, this book should suit you well.

### Roadmap

如果要開分享會可以回頭看

## Chapter 1: The basics of unit testing

unit test定義進化論

### Defining unit testing, step by step

Wiki上對於unit test的經典定義：

>**DEFINITION 1.0** A *unit test* is a piece of a code (usually a method) that invokes another piece of code and checks the correctness of some assumptions afterward. If the assumptions turn out to be wrong, the unit test has failed. A *unit* is a method or function.

作者原本認為就技術上來說這個定義是正確的

>Yes, feel. There is no science in this book. Just art.

但是過了幾年之後作者變心了

>To me, a unit stands for “unit of work” or a “use case” inside the system.

一個單元的工作(?

>**Definition**  
A *unit of work* is the sum of actions that take place between the invocation of a public method in the system and a single noticeable end result by a test of that system. A noticeable end result can be observed without looking at the internal state of the system and only through its public APIs and behavior. An end result is any of the following:

>- The invoked public method returns a value (a function that’s not void).
>- There’s a noticeable change to the state or behavior of the system before and after invocation that can be determined without interrogating private state. (Examples: the system can log in a previously nonexistent user, or the system’s properties change if the system is a state machine.)
>- There’s a callout to a third-party system over which the test has no control, and that third-party system doesn’t return any value, or any return value from that system is ignored. (Example: calling a third-party logging system that was not written by you and you don’t have the source to.)

單元測試中的"單元"不再局限於一個class裡的一個method

>This idea of a unit of work means, to me, that a unit can span as little as a single method and up to multiple classes and functions to achieve its purpose.

你不該刻意最小化你的測試，如果你的測試大一點卻更有可讀性那你的測試就更好維護。

>If you try to minimize the size of a unit of work, you end up faking things down the line that aren’t really end results to the user of a public API but instead are just train stops on the way to the main station.

更新定義

>**UPDATED DEFINITION 1.1** A *unit test* is a piece of code that invokes a unit of work and checks one specific end result of that unit of work. If the assumptions on the end result turn out to be wrong, the unit test has failed. A unit test’s scope can span as little as a method or as much as multiple classes.

UI上的一顆按鈕，它也很接近典型的*單元測試*定義，但它並不是個*好的*單元測試

>No matter what programming language you’re using, one of the most difficult aspects
of defining a unit test is defining what’s meant by a “good” one.

如果你不知道怎麼寫一個好的單元測試那麼不如不寫，不要浪費你的時間因為你很難維護它

談測試的重要性

>How do you make sure that the code works today?

### Properties of a good unit test

A unit test should have the following properties:

- It should be automated and repeatable.
- It should be easy to implement.
- It should be relevant tomorrow.
- Anyone should be able to run it at the push of a button.
- It should run quickly.
- It should be consistent in its results (it always returns the same result if you don’t change anything between runs).
- It should have full control of the unit under test.
- It should be fully isolated (runs independently of other tests).
- When it fails, it should be easy to detect what was expected and determine how to pinpoint the problem.

如果以下任何問題你的回答是不，那你很有可能是在做整合測試

- Can I run and get results from a unit test I wrote two weeks or months or years ago?
- Can any member of my team run and get results from unit tests I wrote two months ago?
- Can I run all the unit tests I’ve written in no more than a few minutes?
- Can I run all the unit tests I’ve written at the push of a button?
- Can I write a basic test in no more than a few minutes?

整合測試和單元測試一樣重要，但是有缺點

### Integration tests

不是"好"的單元測試都歸類給整合測試就對惹

> I consider integration tests as any tests that aren’t fast and consistent and that use one or more real dependencies of the units under test. For example, if the test uses the real system time, the real filesystem, or a real database, it has stepped into the realm of integration testing.

```c#
DateTime.Now
```

這是個經典的例子，因為你每次跑內容都不一樣所以這個測試並不*consistent*

整合測試不是不好，他與單元測試同等重要，只是兩者應該分開

整合測試的一些缺點：

1. 慢
2. 外部依賴讓你比較難消除測試殘留的垃圾(ex:database)
3. 一次測太多東西，壞掉了你很難找是哪邊出錯

>**DEFINITION** *Integration testing* is testing a unit of work without having full control
over all of it and using one or more of its real dependencies, such as time,
network, database, threads, random number generators, and so on.

不自動化整合測試的缺點(阿不就手動測試...)：

- 慢
- 缺少*regression test*，不敢改動*legacy code*
- 可能需要先設定機器環境(configuration)才能跑，就會*不想跑*
- 測試的範圍太大導致很多小地方沒測到
- 沒辦法很快寫出來，有可能相依太多，很難寫成自動測試；如果不寫成自動你又會失去自動化的優點

不過對於*很快*生出單元測試有以下警告

>Small warning: even experienced unit testers can find that it may take 30 minutes or more to figure out how to write the very first unit test against an object model they’ve never unit tested before. This is part of the work, and is expected. The second and subsequent tests on that object model should be very easy to accomplish.

定義regression：

>**DEFINITION** A *regression* is one or more units of work that once worked and now don’t.

定義legacy code：

>**DEFINITION** *Legacy code* is defined by Wikipedia as “source code that relates to a no-longer supported or manufactured operating system or other computer technology,” but many shops refer to any older version of the application currently under maintenance as legacy code. It often refers to code that’s hard to work with, hard to test, and usually even hard to read.

本書使用遺~~留代碼之父~~的定義

>Many people like to define legacy code as “code that has no tests.” *Working Effectively with Legacy Code* by Michael Feathers (Prentice Hall, 2004) uses this as an official definition of legacy code, and it’s a definition to be considered while reading this book.

### What makes unit tests good

好長一段故事終於講完了終於要做最後的定義了(once and for all)。

>**UPDATED AND FINAL DEFINITION 1.2** A *unit test* is an automated piece of code that invokes the unit of work being tested, and then checks some assumptions about a single end result of that unit. A unit test is almost always written using a unit testing framework. It can be written easily and runs quickly. It’s trustworthy, readable, and maintainable. It’s consistent in its results as long as production code hasn’t changed.

懶人包：

- 94要自動
- 只測一個結果
- 好寫、跑得快
- 只要產品沒動，測試結果應該要一致

測試不一定只針對*Control flow code*，例如*Property*，通常他不包含邏輯但是他有可能是*unit of work*的一環。

>**DEFINITION** *Control flow code* is any piece of code that has some sort of logic in it, small as it may be. It has one or more of the following: an if statement, a loop, switch, or case statement, calculations, or any other type of decisionmaking code.

### A simple unit test example

教你古早沒有framework的時候自動測試可能會長啥樣。

### Test-driven development

TDD種類很多，這裡講的是Test Driven Development，不包含Design。
>**Test Driven Development:** the idea of writing your code in a test first manner. You may already have an existing design in place.

這本書不會教你TDD，TDD有許多好處，但是有代價的(要學)。

>Test-driven development— a bird’s-eye view. Notice the spiral nature of the process: write test, write code, refactor, write next test. It shows the incremental nature of TDD: small steps lead to a quality end result.

TDD跟寫一個好測試是沒關係的，使用TDD不能保證你的測試好讀好維護。

TDD步驟：

1. Write a failing test to prove code or functionality is missing from the end product.
2. Make the test pass by writing production code that meets the expectations of your test.
3. Refactor your code.

重構可以發生在一堆測試之後或是寫一個馬上重構，它的目的是讓你的程式更好讀更好維護。

>**DEFINITION** *Refactoring* means changing a piece of code without changing its functionality. If you’ve ever renamed a method, you’ve done refactoring. If you’ve ever split a large method into multiple smaller method calls, you’ve refactored your code. The code still does the same thing, but it becomes easier to maintain, read, debug, and change.

TDD用的好，程式品質上升、bug減少、對code更有信心、找bug的時間變少、code的設計更好和讓manager開薰；反之用得不好，會讓時程delay、讓你更沒動力、程式品質下降，是一把雙面刃。

TDD最大的好處之一：測試測試本身，因為你親眼看過他紅燈再綠燈，讓你對測試本身更有自信，所以你為了"以防萬一"而做的測試會變少。

### The three core skills of successful TDD

~~阿不是說不教還一直講~~

>To be successful in test-driven development you need three different skill sets: knowing how to write good tests, writing them test-first, and designing them well.

以下開始傳教

*Just because you write your tests first doesn’t mean they’re maintainable, readable, or trustworthy.*

所以才有這本書

*Just because you write readable, maintainable tests doesn’t mean you get the same benefitsas when writing them test-first.*

推薦Kent Beck’s *Test-Driven Development: by Example* (Addison-Wesley Professional, 2002).

*Just because you write your tests first, and they’re readable and maintainable, doesn’tmean you’ll end up with a well-designed system.*

推薦*Growing Object-Oriented Software, Guided by Tests* by Steve Freeman and Nat Pryce (Addison-Wesley Professional, 2009) and *Clean Code* by Robert C. Martin (Prentice Hall, 2008)

簡單來說你要會好的TDD你~~他媽~~還要再看兩本書

**建議一次專注學習一個技能**忽略其他技能就好，不然你會很容易放棄(the wall is too high to climb.)

### Summary

好測試懶人包：

- It’s an automated piece of code that invokes a different method and then checks some assumptions on the logical behavior of that method or class.
- It’s written using a unit testing framework.
- It can be written easily.
- It runs quickly.
- It can be executed repeatedly by anyone on the development team.

你了解了整合測試與單元測試的區別，當你面對問題的時候需要做抉擇是有哪種測試才能解決你的問題。

你了解TDD的一些好處，但在這之前你要先會寫好的測試。

## Chapter 2: A first unit test

Nunit登場

### Frameworks for unit testing

>Manual tests suck.

沒有這東西你平常是怎麼測你的code的?

|Unit testing practice|How the framework helps|
|-|-|
|Write tests easily and in a structured manner.|Framework supplies the developer with a class libarary that contains </br> - Base classes or interfaces to inherit </br> - Attributes to place in your code to note which of your methods are tests </br> - Assertion classes that have special assertion methods you invoke to verify your code|
|Execute one or all of the unit tests.|Framework provides a test runner (a console or GUI tool) that </br> - Identifies tests in your code </br> - Runs tests automatically </br> - Indicates status while running </br> - Can be automated by the command line|
|Review the results of the test runs.|The test runners will usually provide information such as </br> - How many tests ran </br> - How many tests didn’t run </br> - How many tests failed </br> - Which tests failed </br> - The reason tests failed </br> - The ASSERT message you wrote </br> - The code location that failed </br> - Possibly a full stack trace of any exceptions that caused the test to fail, and will let you go to the various method calls inside the call stack|

>**NOTE** Using a unit testing framework doesn’t ensure that the tests you write are *readable, maintainable, or trustworthy* or that they cover all the logic you’d like to test.

### Introducing the LogAn project

從簡單開始

>Here’s the scenario. Your company has many internal products it uses to monitor its applications at customer sites. All these products write log files and place them in a special directory. The log files are written in a proprietary format that your company has come up with that can’t be parsed by any existing third-party tools. You’re tasked with building a product, LogAn, that can analyze these log files and find special cases and events in them. When it finds these cases and events, it should alert the appropriate parties.

### First steps with NUnit

先安裝R幹~

NUnit3不知道還有沒有GUI

```c#
public class LogAnalyzer
{
    public bool IsValidLogFileName(string fileName)
    {
        if(fileName.EndsWith(".SLF"))
        {
            return false;
        }
            return true;
    }
}
```

1. Add a new class library project to the solution, which will contain your test classes. Name it LogAn.UnitTests (assuming the original project name is LogAn.csproj).
2. To that library, add a new class that will hold your test methods. Name it LogAnalyzerTests (assuming that your class under test is named LogAnalyzer.cs).
3. Add a new method to the preceding test case named IsValidLogFileName_BadExtension_ReturnsFalse().
4. Add a reference to the project under test for the new testing project.

Basic rules for placing and naming tests

|Object to be tested|Object to create on the testing side|
|-|-|
|Project|Create a test project named `[ProjectUnderTest].UnitTests`.|
|Class|For a class located in ProjectUnderTest, create a class with the name `[ClassName]` Tests.|
|Unit of work (a method, or a logical grouping of several methods, or several classes)|For each unit of work, create a test method with the following name: `[UnitOfWorkName]_[ScenarioUnderTest]_[ExpectedBehavior]`. The unit of work name could be as simple as a method name (if that’s the whole unit of work) or more abstract if it’s a use case that encompasses multiple methods or classes such as UserLogin or RemoveUser or Startup. You might feel more comfortable starting with method names and moving to more abstract names later. Just make sure that if these are method names, those methods are public, or they don’t really represent the start of a unit of work.|

反正重點你也知道：好找

你的測試應該直接寫在production code project裡還是另開test專用project呢？分開可以讓你的production code乾淨一點好讀一點，但寫在production code旁邊可以在deployment之後做health test，You *can* actually have your cake and eat.

來人，上Attribute

```c#
[TestFixture]
public class LogAnalyzerTests
{
    [Test]
    public void IsValidFileName_BadExtension_ReturnsFalse()
    {
    }
}
```

The NUnit runner needs at least two attributes to know what to run:

- The `[TestFixture]` attribute that denotes a class that holds automated NUnit tests. (If you replace the word Fixture with Class, it makes much more sense, but only as a mental exercise. It won’t compile if you literally change the code that way.) Put this attribute on top of your new LogAnalyzerTests class.
- The `[Test]` attribute that can be put on a method to denote it as an automated test to be invoked. Put this attribute on your new test method.

### Writing your first test

>How do you test your code? A unit test usually comprises three main actions:
>1. Arrange objects, creating and setting them up as necessary.
>2. Act on an object.
>3. Assert that something is as expected.

```c#
[Test]
public void IsValidFileName_BadExtension_ReturnsFalse()
{
    LogAnalyzer analyzer = new LogAnalyzer();

    bool result = analyzer.IsValidLogFileName("filewithbadextension.foo");

    Assert.False(result);
}
```

#### The Assert class

```c#
Assert.AreEqual(expectedObject, actualObject, message);
Assert.AreEqual(2, 1+1, "Math is broken");
Assert.AreSame(expectedObject, actualObject, message);
Assert.AreSame(int.Parse("1"),int.Parse("1"), "this test should fail").
```

叫你不要用message辣：

Also note that all the assert methods take a last parameter of type “string,” which gets displayed in addition to the framework output, in case of a test failure. Please, never, *ever*, use this parameter (it’s always optional to use). Just make sure your test name explains what’s supposed to happen. Often, people write the trivially obvious things like “test failed” or “expected x instead of y,” which the framework already provides. Much like comments in code, if you have to use this parameter, your method name should be clearer.

開跑囉

There are at least four ways you can run this test:

- Using the NUnit GUI
- Using Visual Studio 2012 Test Runner with an NUnit Runner Extension, called the NUnit Test Adapter in the NUget Gallery
- Using the ReSharper test runner (a well-known commercial plug-in for VS)
- Using the TestDriven.NET test runner (another well-known commercial plug-in for VS)

好了你看到紅燈了，修成綠燈吧

```c#
if(!fileName.EndsWith(".SLF"))
{
    return false;
}
```

>You’ve seen that bad extensions are flagged as such, but who’s to say that good ones do get approved by this little method? If you were doing this in a test-driven way, a missing test here would have been obvious, but because you’re writing the tests after the code, you have to come up with good test ideas that will cover all the paths.

```c#
[Test]
public void IsValidLogFileName_GoodExtensionLowercase_ReturnsTrue()
{
    LogAnalyzer analyzer = new LogAnalyzer();
    bool result = analyzer
    .IsValidLogFileName("filewithgoodextension.slf");
    Assert.True(result);
}
[Test]
public void IsValidLogFileName_GoodExtensionUppercase_ReturnsTrue()
{
    LogAnalyzer analyzer = new LogAnalyzer();
    bool result =
    analyzer
    .IsValidLogFileName("filewithgoodextension.SLF");
    Assert.True(result);
}
```

修正

```c#
public bool IsValidLogFileName(string fileName)
{
    if (!fileName.EndsWith(".SLF",
    StringComparison.CurrentCultureIgnoreCase))
    {
        return false;
    }
    return true;
}
```

TDD神教箴言`Red-Green-Refacto`

>The red/green concept is prevalent throughout the unit testing world and especially in test-driven development. Its mantra is “Red-Green-Refactor,” meaning that you start with a failing test, then pass it, and then make your code readable and more maintainable.

大多的framework在處理任何未預期的例外也會使測試停止並失敗亮紅燈

>Tests can also fail if an unexpected exception suddenly gets thrown. A test that stops because of an unexpected exception will be considered a failed test for most test frameworks, if not all. It’s part of the point—sometimes you have bugs in the form of an exception you didn’t expect.

關於`Test code styling`

- 測試名稱可以超長，可讀性比較重要
- 空行區分3A
- 盡量區分act和assert

老話一句：
>Readability is one of the most important aspects when writing a test.

### Refactoring to parameterized tests

1. Replace the `[Test]` attribute with the `[TestCase]` attribute.
2. Extract all the hardcoded values the test is using into parameters for the test method.
3. Move the values you had before into the braces of the [TestCase(param1, param2,..)] attribute.
4. Rename this test method to a more generic name.
5. Add a `[TestCase(..)]` attribute on this same test method for each of the tests you want to merge into this test method, using the other test’s values.
6. Remove the other tests so you’re left with just one test method that has multiple `[TestCase]` attributes.

```c#
[TestCase("filewithgoodextension.SLF")]
[TestCase("filewithgoodextension.slf")]
public void IsValidLogFileName_ValidExtensions_ReturnsTrue(string file)
{
    LogAnalyzer analyzer = new LogAnalyzer();
    bool result = analyzer.IsValidLogFileName(file);
    Assert.True(result);
}
```

如果要加上會失敗的case呢?

```c#
[TestCase("filewithgoodextension.SLF",true)]
[TestCase("filewithgoodextension.slf",true)]
[TestCase("filewithbadextension.foo",false)]
public void
IsValidLogFileName_VariousExtensions_ChecksThem(string file, bool expected)
{
    LogAnalyzer analyzer = new LogAnalyzer();
    bool result = analyzer.IsValidLogFileName(file);
    Assert.AreEqual(expected,result);
}
```

缺點大概心知肚明

>I’ll warn that doing this will likely create a less-readable test method because the name will have to become even more *generic*. Consider this a demo of the syntax, and know that this is possibly taking this technique too far in the right direction, because it makes the tests less understandable without going deeply through the code.

### More NUnit attributes

#### Setup and teardown

記得把東西清乾淨，如果你的測試有dependency問題要解是很花時間的。

- `[SetUp]`—This attribute can be put on a method, just like a [Test] attribute, and it causes NUnit to run that setup method each time it runs any of the tests in your class.
- `[TearDown]`—This attribute denotes a method to be executed once after each test in your class has executed.

一樣，謹記你用越多這種東西你的可讀性越低

>I tell my students, “Imagine that the readers of your test have never met you and never will. They arrive and read your tests two years after you’ve left the company. Every little thing you do to help them understand the code without needing to ask any questions is a big help. They probably have nobody around who can answer those questions, so you’re their only hope.”

(可是呢，做這種事只有技術人員知道呢)

```c#
using NUnit.Framework;
[TestFixture] public class LogAnalyzerTests
{
    private LogAnalyzer m_analyzer=null;
    [SetUp]
    public void Setup()
    {
        m_analyzer = new LogAnalyzer();
    }
    [Test]
    public void IsValidFileName_validFileLowerCased_ReturnsTrue()
    {
        bool result = m_analyzer
        .IsValidLogFileName("whatever.slf");
        Assert.IsTrue(result, "filename should be valid!");
    }
    [Test]
    public void IsValidFileName_validFileUpperCased_ReturnsTrue()
    {
        bool result = m_analyzer
        .IsValidLogFileName("whatever.SLF");
        Assert.IsTrue(result, "filename should be valid!");
    }
    [TearDown]
    public void TearDown()
    {
        //the line below is included to show an anti pattern.
        //This isn’t really needed. Don’t do it in real life.
        m_analyzer = null;
    }
}
```

>Think of the setup and teardown methods as constructors and destructors for the tests in your class.

換句話說(或我自己腦補)，不要再用constructor，不過作者建議連setup都不要用，直接使用工廠方法(factory method)

>In real life I do *not* use setup methods to initialize my instances. I show it here for you to know that it exists and to avoid it. It may seem like a good idea, but soon it makes the tests below the setup method harder to read. Instead, I use factory methods to initialize my instances under test.

另外也有`[TestFixtureSetUp]`和`[TestFixtureTearDown]`這種attribute，在設置或清除需要較長時間的測試時很好用，但請千萬謹慎使用，因為你正在測試間分享狀態。

不要在`單元`測試使用TearDown和TestFixture

>You almost *never, ever* use **TearDown** or **TestFixture** methods in unit test projects. If you do, you’re very likely writing an integration test, where you’re touching the filesystem or a database, and you need to clean up the disk or the DB after the tests. The only time it makes sense to use a **TearDown** method in unit tests, I’ve found, is when you need to “reset” the state of a static variable or singleton in memory between tests. Any other time, you’re likely doing integration tests. That’s not a bad thing to be doing, but you should be doing it in a separate project that’s dedicated to integration tests.

#### Checking for expected exceptions

```c#
public class LogAnalyzer
{
    public bool IsValidLogFileName(string fileName)
    {
        …
        if (string.IsNullOrEmpty(fileName))
        {
            throw new ArgumentException(
            "filename has to be provided");
        }
        …
    }
}
```

過去的唯一處理這種狀況但你不該使用的API(介紹爽的)：

```c#
[Test]
[ExpectedException(typeof(ArgumentException), ExpectedMessage ="filename has to be provided")]
public void IsValidFileName_EmptyFileName_ThrowsException()
{
    m_analyzer.IsValidLogFileName(string.Empty);
}
private LogAnalyzer MakeAnalyzer()
{
    return new LogAnalyzer();
}
```

順便展現了工廠方法，好處是當constructor有異動你只需要改一個地方而不是所有測試都要修

為什麼不要使用?因為他將整個方try-catch起來，首先你不知道哪一行出錯，另外你有可能發生一個exception(例如建構子出錯)可是你的測試通過了(因為只catch特定exception)

這個是你該用的

```c#
[Test]
public void IsValidFileName_EmptyFileName_Throws()
{
    LogAnalyzer la = MakeAnalyzer();
    var ex =
        Assert.Catch<Exception>(() => la.IsValidLogFileName(""));
    StringAssert.Contains("filename has to be provided",
                                            ex.Message);
}
```

使用**Assert.Catch**塞lambda expression

StringAssert.Contains是另一個Nunit的功能，使用Contains而不是直接equal還有濾除奇怪東西的功能(嘴)

#### Ignoring tests

你知道的，就像ignore Flake8一樣的好用

```c#
[Test]
[Ignore("there is a problem with this test")]
public void IsValidFileName_ValidFile_ReturnsTrue()
{
/// ...
}
```

據說會亮黃燈

#### NUnit’s fluent syntax

AssertThat94潮  
[nunit-fluent-assertions](https://lukewickstead.wordpress.com/2013/02/09/howto-nunit-fluent-assertions/)

```c#
Assert.That(2+2, Is.EqualTo(4));
```

#### Setting test categories

```c#
[Test]
[Category("Fast Tests")]
public void IsValidFileName_ValidFile_ReturnsTrue()
{
/// ...
}
```

### Testing results that are system state changes instead of return values

>Checking that the system’s behavior is different after performing an action on the system under test.

檢查狀態改變

>**DEFINITION** *State-based testing* (also called *state verification*) determines whether the exercised method worked correctly by examining the changed behavior of the system under test and its collaborators (dependencies) after the method is exercised.

以下談到隨便把state露出只為了測試不好，可是好像沒講到解法

>If you’ve read other definitions of state-based testing elsewhere, you’ll notice that I define it differently. That is because I view this in a slightly different light—that of test maintainability. Simply testing direct state (sometimes externalizing it to make it testable) is something I wouldn’t usually endorse, because it leads to less-maintainable and less-readable code.

```c#
public class LogAnalyzer
{
    public bool WasLastFileNameValid { get; set; }
    public bool IsValidLogFileName(string fileName)
    {
        WasLastFileNameValid = false;
        if (string.IsNullOrEmpty(fileName))
        {
            throw new ArgumentException("filename has to be provided");
        }
        if (!fileName.EndsWith(".SLF",
            StringComparison.CurrentCultureIgnoreCase))
        {
            return false;
        }
        WasLastFileNameValid = true;
        return true;
    }
}
```

因為`WasLastFileNameValid`依賴別的method修改他，所以無法直接測

因為作者為我大TDD神教虔誠教徒，又再強調我在這裡把code先賞你是因為我沒打算教你TDD，如果用TDD *could*寫出更好的測試，不過你要*先學會寫測試*

```c#
[Test]
public void
IsValidFileName_WhenCalled_ChangesWasLastFileNameValid()
{
    LogAnalyzer la = MakeAnalyzer();
    la.IsValidLogFileName("badname.foo");
    Assert.False(la.WasLastFileNameValid);
}
```

還是使用method name當測試名稱，`WasLastFileNameValid`只是*unit of work*的一環

refactor新增反向：

```C#
[TestCase("badfile.foo", false)]
[TestCase("goodfile.slf", true)]
public void IsValidFileName_WhenCalled_ChangesWasLastFileNameValid(string file, bool expected)
{
    LogAnalyzer la = MakeAnalyzer();
    la.IsValidLogFileName(file);
    Assert.AreEqual(expected, la.WasLastFileNameValid);
}
```

另外一個口袋計算機例子：

```c#
public class MemCalculator
{
    private int sum=0;
    public void Add(int number)
    {
        sum+=number;
    }
    public int Sum()
    {
        int temp = sum;
        sum = 0;
        return temp;
    }
}
```

該怎麼測試呢？You should always consider the simplest
test to begin with. 就從Sum()的default為0開始：

```c#
[Test]
public void Sum_ByDefault_ReturnsZero()
{
    MemCalculator calc = new MemCalculator();
    int lastSum = calc.Sum();
    Assert.AreEqual(0,lastSum);
}
```

再次強調測試名稱的重要性，以這個為例就像句子一般

>Here’s a simple list of naming conventions of scenarios I like to use in such cases:
>- **ByDefault** can be used when there’s an expected return value with no prior action, as shown in the previous example.
>- **WhenCalled** or **Always** can be used in the second or third kind of unit of work results (change state or call a third party) when the state change is done with no prior configuration or when the third-party call is done with no prior configuration; for example, **Sum_WhenCalled_CallsTheLogger** or **Sum_Always_CallsTheLogger**.

新增Add()的測試：

```c#
[Test]
public void Sum_ByDefault_ReturnsZero()
{
    MemCalculator calc = MakeCalc();
    int lastSum = calc.Sum();
    Assert.AreEqual(0, lastSum);
}
[Test]
public void Add_WhenCalled_ChangesSum()
{
    MemCalculator calc = MakeCalc();
    calc.Add(1);
    int sum = calc.Sum();
    Assert.AreEqual(1, sum);
}
private static MemCalculator MakeCalc()
{
    return new MemCalculator();
}
```

>Notice that this time you use a factory method to initialize **MemCalculator**. This is a good idea, because it saves time writing the tests, makes the code inside each test smaller and a little more readable, and makes sure **MemCalculator** is always initialized the same way. It’s also better for test maintainability, because if the constructor for **MemCalculator** changes, you only need to change the initialization in one place instead of going through each test and changing the **new** call.

`always initialized the same way`我懂，可是`saves time writing the tests`有嗎= =?

>So far, so good. But what happens when the method you’re testing depends on an
external resource, such as the filesystem, a database, a web service, or anything else that’s hard for you to control? And how do you test the third type of result for a unit of work—a call to a third party? That’s when you start creating test stubs, fake objects, and mock objects, which are discussed in the next few chapters.

### Summary

>In this chapter, we looked at using NUnit to write simple tests against simple code.

- You used the `[TestCase]`,`[SetUp]`, and `[TearDown]` attributes to make sure your tests always use new and untouched state.
- You used factory methods to make this more maintainable.
- You used `[Ignore]` to skip tests that need to be fixed.
- Test categories can help you group tests in a logical way rather than by class and namespace
- `Assert.Catch()` helps you make sure your code throws exceptions when it should.
- Testing results that are system state changes instead of return values

>Finally, keep the following points in mind:
>- It’s common practice to have one test class per tested class, one unit test project per tested project (aside from an integration tests project for that tested project), and at least one test method per unit of work (which can be as small as a method or as large as multiple classes).
>- Name your tests clearly using the following model: `[UnitOfWork]_[Scenario]_[ExpectedBehavior]`.
>- Use factory methods to reuse code in your tests, such as code for creating and initializing objects all your tests use.
>- Don’t use `[SetUp]` and `[TearDown]` if you can avoid them. They make tests less understandable.

## Chapter 3: Using stubs to break dependencies

當你的測試目標依賴不受控的東西時(ex: webservice, time of day, threading...)怎辦？stubs登場。

### Introducing stubs

What is external dependency?

>**DEFINITION** An *external dependency* is an object in your system that your code under test interacts with and over which you have no control. (Common examples are filesystems, threads, memory, time, and so on.)

利用stub來解決external dependency的問題

>**DEFINITION** A *stub* is a controllable replacement for an existing dependency (or collaborator) in the system. By using a stub, you can test your code without dealing with the dependency directly.

mock與stub的差別：
>mocks versus stubs is that mocks are just like stubs, but you assert against the mock object, whereas you do not assert against a stub.

題外話：
*xUnit Test Patterns: Refactoring Test Code* by Gerard Meszaros (Addison-Wesley, 2007)這是一本很經典的單元測試pattern書，書中把fake分成了五種，不過本書作者只分成三種認為這樣比較好懂。

### Identifying a filesystem dependency in LogAn

假設`LogAnalyzer`把有效的extensions名稱存在一份檔案裡面當作config，那麼`IsValidLogFileName`可能會長這樣：

```c#
public bool IsValidLogFileName(string fileName)
{
//read through the configuration file
//return true if configuration says extension is supported.
}
```

如果這樣設計就會直接相依於檔案系統，變成在寫整合測試，你就會有以下缺點：

- 很慢
- 需要configuration
- 一次測試太多東西

>This is the essence of *test-inhibiting* design: the code has some dependency on an external resource, which might break the test even though the code’s logic is perfectly valid.

### Determining how to easily test LogAnalyzer

>“There is no object-oriented problem that cannot be solved by adding a layer of indirection, except, of course, too many layers of indirection.” I like this quote (from <http://en.wikipedia.org/wiki/Abstraction_layer>) because a lot of the “art” in the art of unit testing is about finding the right place to add or use a layer of indirection to test the code base.

有東西不能測?加一層把他包起來然後弄個假的東西假裝他，或是讓他可以被替代。這中間的藝術就是你要知道是否哪曾已經存在或許你不需要新增，又或是系統已經太複雜你不該再加一層。

Pattern for breaking the dependency:

1. Find the *interface* that the start of the unit of work under test works against. (In this case, “interface” isn’t used in the pure object-oriented sense; it refers to the defined method or class being collaborated with.) In our LogAn project, this is the filesystem configuration file.
2. If the interface is *directly connected* to your unit of work under test (as in this case—you’re calling directly into the filesystem), make the code testable by adding a level of indirection hiding the interface. In our example, moving the direct call to the filesystem to a separate class (such as `FileExtensionManager`) would be one way to add a level of indirection. We’ll also look at others. (Figure 3.3 shows how the design might look after this step.)
3. Replace the *underlying implementation* of that interactive interface with something that you have control over. In this case, you’ll replace the instance of the class that your method calls (`FileExtensionManager`) with a stub class that you can control (`StubExtensionManager`), giving your test code control over external dependencies.

### Refactoring your design to be more testable

>**DEFINITION** *Refactoring* is the act of changing code without changing the code’s functionality. That is, it does exactly the same job as it did before. No more and no less. It just looks different. A refactoring example might be renaming a method and breaking a long method into several smaller methods.

You can refactor code by introducing a new seam into it without changing the original functionality of the code, which is exactly what I’ve done by introducing the new `IExtensionManager` interface.

>**DEFINITION** *Seams* are places in your code where you can plug in different functionality, such as stub classes, adding a constructor parameter, adding a public settable property, making a method virtual so it can be overridden, or externalizing a delegate as a parameter or property so that it can be set from outside a class. Seams are what you get by implementing the Open-Closed Principle, where a class’s functionality is open for extenuation, but its source code is closed for direct modification. (See *Working Effectively with Legacy Code* by Michael Feathers, for more about seams, or *Clean Code* by Robert Martin about the Open-Closed Principle.)

幹有沒有這麼嚴重

>I’ll remind you that refactoring your code without having any sort of automated tests against it (integration or otherwise) can lead you down a career-ending rabbit hole if you’re not careful.

Type A and Type B refactorings:

- *Type* A—Abstracting concrete objects into *interfaces* or *delegates*
- *Type* B—Refactoring to allow injection of *fake implementations* of those delegates or interfaces

其他例子：

- *Type* A—Extract an interface to allow replacing underlying implementation.
- *Type* B—Inject stub implementation into a class under test.
- *Type* B—Inject a fake at the constructor level.
- *Type* B—Inject a fake as a property get or set.
- *Type* B—Inject a fake just before a method call.

`Extracting a class that touches the filesystem and calling it`

```c#
public bool IsValidLogFileName(string fileName)
{
    FileExtensionManager mgr =
    new FileExtensionManager();
    return mgr.IsValid(fileName);
}
class FileExtensionManager
{
    public bool IsValid(string fileName)
    {
    //read some file here
    }
}
```

`Extracting an interface from a known class`

```c#
public class FileExtensionManager : IExtensionManager
{
    public bool IsValid(string fileName)
    {
    ...
    }
}
public interface IExtensionManager
{
    bool IsValid (string fileName);
}
//the unit of work under test:
public bool IsValidLogFileName(string fileName)
{
    IExtensionManager mgr =
    new FileExtensionManager();
    return mgr.IsValid(fileName);
}
```

`Simple stub code that always returns true`

```c#
public class AlwaysValidFakeExtensionManager : IExtensionManager
{
    public bool IsValid(string fileName)
    {
        return true;
    }
}
```

Dependency Injection

- Receive an interface at the constructor level and save it in a field for later use.
- Receive an interface as a property get or set and save it in a field for later use.
- Receive an interface just before the call in the method under test using one of the following:
  - A parameter to the method (parameter injection)
  - A factory class
  - A local factory method
  - Variations on the preceding techniques

`Injecting your stub using constructor injection`

```c#
public class LogAnalyzer
{
    private IExtensionManager manager;
    public LogAnalyzer(IExtensionManager mgr)
    {
        manager = mgr;
    }
    public bool IsValidLogFileName(string fileName)
    {
        return manager.IsValid(fileName);
    }
}
public interface IExtensionManager
{
    bool IsValid(string fileName);
}
[TestFixture]
public class LogAnalyzerTests
{
    [Test]
    public void
    IsValidFileName_NameSupportedExtension_ReturnsTrue()
    {
        FakeExtensionManager myFakeManager =
        new FakeExtensionManager();
        myFakeManager.WillBeValid = true;
        LogAnalyzer log =
        new LogAnalyzer (myFakeManager);
        bool result = log.IsValidLogFileName("short.ext");
        Assert.True(result);
    }
}
internal class FakeExtensionManager : IExtensionManager
{
    public bool WillBeValid = false;
    public bool IsValid(string fileName)
    {
        return WillBeValid;
    }
}
```

>**NOTE** The fake extension manager is located in the same file as the test code
because currently the fake is used only from within this test class. It’s far easier to locate, read, and maintain a handwritten fake in the same file than in a different one. If, later on, you have an additional class that needs to use this
fake, you can move it to another file easily with a tool like ReSharper (which I
highly recommend).

如果使用建構子注入有可能會因為要注入的東西太多導致參數很長，這時候可以用下列兩種方式解決：

- parameter object refactoring.
- inversion of control (IoC) containers.

>I seldom use containers in my real code. I find that most of the time they complicate the design and readability of things. It might be that if you need a container, your design needs changing. What do you think?

太好了可以少學一個東西

使用建構子注入雖然會讓測試變笨重，但他是首選，因為可讀性最好，不過這還跟設計有關，相依物件放在建構子的意思同時代表這些東西是不可選擇的。

`Simulating exceptions from fakes`
(示範而已，這應該不是個好設計，且要在IsValidLogFileName裡面寫try-catch)

```c#
[Test]
public void IsValidFileName_ExtManagerThrowsException_ReturnsFalse()
{
    FakeExtensionManager myFakeManager =
    new FakeExtensionManager();
    myFakeManager.WillThrow = new Exception(“this is fake”);
    LogAnalyzer log =
    new LogAnalyzer (myFakeManager);
    bool result = log.IsValidLogFileName("anything.anyextension");
    Assert.False(result);
}
}
internal class FakeExtensionManager : IExtensionManager {
    public bool WillBeValid = false;;
    public Exception WillThrow = null ;
    public bool IsValid(string fileName)
    {
    if(WillThrow !=null)
        { throw WillThrow;}
        return WillBeValid;
    }
}
```

`Injecting a fake by adding property setters to the class under test`

```c#
public class LogAnalyzer
{
    private IExtensionManager manager;
    public LogAnalyzer ()
    {
        manager = new FileExtensionManager();
    }
public IExtensionManager ExtensionManager
{
    get { return manager; }
    set { manager = value; }
}
public bool IsValidLogFileName(string fileName)
{
    return manager.IsValid(fileName);
}
}
[Test]
Public void
IsValidFileName_SupportedExtension_ReturnsTrue()
{
    //set up the stub to use, make sure it returns true
    ...
    //create analyzer and inject stub
    LogAnalyzer log =
    new LogAnalyzer ();
    log.ExtensionManager=someFakeManagerCreatedEarlier;
    //Assert logic assuming extension is supported
    ...
}
}
```

使用property設定會影響設計概念：可以不需要這個東西來操作這個類別，感覺沒甚麼使用時機。

`Setting a factory class to return a stub when the test is running`

```c#
public class LogAnalyzer
{
    private IExtensionManager manager;
    public LogAnalyzer ()
    {
        manager = ExtensionManagerFactory.Create();
    }
    public bool IsValidLogFileName(string fileName)
    {
        return manager.IsValid(fileName)
        && Path.GetFileNameWithoutExtension(fileName).Length > 5;
    }
}
[Test]
public void
IsValidFileName_SupportedExtension_ReturnsTrue()
{
    //set up the stub to use, make sure it returns true
    ...
    ExtensionManagerFactory
    .SetManager(myFakeManager);
    //create analyzer and inject stub
    LogAnalyzer log =
    new LogAnalyzer ();
    //Assert logic assuming extension is supported
    ...
}
class ExtensionManagerFactory
{
    private IExtensionManager customManager = null;
    public IExtensionManager Create()
    {
        If(customManager!=null)
        return customManager;
        Return new FileExtensionManager();
    }
    public void SetManager(IExtensionManager mgr)
    {
        customManager = mgr;
    }
}
```

Here are the steps for using a factory method in your tests:

- In the class under test,
  - Add a virtual factory method that returns the real instance.
  - Use the factory method in your code, as usual.
- In your test project,
  - Create a new class.
  - Set the new class to inherit from the class under test.
  - Create a public field (no need for property get or set) of the interface type you want to replace (`IExtensionManager`).
  - Override the virtual factory method.
  - Return the public field.
- In your test code,
  - Create an instance of a stub class that implements the required interface (`IExtensionManager`).
  - Create an instance of the newly derived class, not of the class under test.
  - Configure the new instance’s public field (which you created earlier) and setit to the stub you’ve instantiated in your test.

`Faking a factory method`

```c#
public class LogAnalyzerUsingFactoryMethod
{
    public bool IsValidLogFileName(string fileName)
    {
        return GetManager().IsValid(fileName);
    }
    protected virtual IExtensionManager GetManager()
    {
        return new FileExtensionManager();
    }
}
[TestFixture]
public class LogAnalyzerTests
{
    [Test]
    public void overrideTest()
    {
        FakeExtensionManager stub = new FakeExtensionManager();
        stub.WillBeValid = true;
        TestableLogAnalyzer logan =
        new TestableLogAnalyzer(stub);
        bool result = logan.IsValidLogFileName("file.ext");
        Assert.True(result);
    }
}
class TestableLogAnalyzer : LogAnalyzerUsingFactoryMethod
{
    public TestableLogAnalyzer(IExtensionManager mgr)
    {
        Manager = mgr;
    }
    public IExtensionManager Manager;
    protected override IExtensionManager GetManager()
    {
        return Manager;
    }
}
internal class FakeExtensionManager : IExtensionManager
{
    //no change from the previous samples
    ...
}
```

這個叫做*Extract and Override*，對付Legacy Code的好招，使用時機：

- 需要模擬*return value*
- 需要模擬整個interface
- 需要驗證互動不適合(必須多寫code才有可能達成)
- 如果沒有一個已經看起來欠inject的interface的時候

`Returning a result rather than a stub object from an extracted method`

```c#
public class LogAnalyzerUsingFactoryMethod
{
    public bool IsValidLogFileName(string fileName)
    {
        return this.IsValid(fileName);
    }
    protected virtual bool IsValid(string fileName)
    {
        FileExtensionManager mgr = new FileExtensionManager();
        return mgr.IsValid(fileName);
    }
}
[Test]
public void overrideTestWithoutStub()
{
    TestableLogAnalyzer logan = new TestableLogAnalyzer();
    logan.IsSupported = true;
    bool result = logan.IsValidLogFileName("file.ext");
    Assert.True(result,"...");
}
class TestableLogAnalyzer : LogAnalyzerUsingFactoryMethod
{
    public bool IsSupported;
    protected override bool IsValid(string fileName)
    {
        return IsSupported;
    }
}
```

### Overcoming the encapsulation problem

“Don’t be silly.”

OO的封裝API使得物件模型不會被end user誤用，test也是一種end user，他與原本的end user一樣重要只是目標不同，不過如果有潔癖還是覺得這樣破壞封裝，有以下三種方式可用：

- Using internal and [InternalsVisibleTo]
- Using the [Conditional] attribute
- Using #if and #endif with conditional compilation

### Summary

- 花式injected
- 甚麼是fake
- 進入越深層你的測試越難懂,但越上層你能控制的東西越少
- The Extract and Override method
- TOOD?

## Chapter 4: Interaction testing using mock objects

第三種測試類型：互動，mock登場

### Value-based vs. state-based vs. interaction testing

到目前為止的三種測試：

1. Value-base testing
2. State-base testing
3. Interaction testing

>**DEFINITION** *Interaction testing* is testing how an object sends messages (calls methods) to other objects. You use interaction testing when calling another object is the end result of a specific unit of work.

You can also think of interaction testing as being action-driven testing. *Action-driven* testing means that you test a particular action an object takes (such as sending a message to another object).

interaction testing永遠是你的最後選項，因為它最複雜。(不過並不是所有人都這樣認為，we'll see)，但就是偶爾會有機會使用interaction testing比其他兩種的複雜度來的低。

>**DEFINITION** A *mock object* is a fake object in the system that decides whether the unit test has passed or failed. It does so by verifying whether the object under test called the fake object as expected. There’s usually no more than one mock per test.

既然stub與mock都定義完了就可以定義fake了

>**DEFINITION** A *fake* is a generic term that can be used to describe either a stub or a mock object (handwritten or otherwise), because they both look like the real object. Whether a fake is a stub or a mock depends on how it’s used in the current test. If it’s used to check an interaction (asserted against), it’s a mock object. Otherwise, it’s a stub.

### The difference between mocks and stubs

定義這些東西是方便使用framework時可以快速辨認它的角色，尤其當你review其他人的測試發現有超過一個mock時。

>The easiest way to tell you’re dealing with a stub is to notice that the stub can never fail the test. The asserts that the test uses are always against the class under test.

換句話說，測試程式會對mock做assert。

### A simple handwritten mock example

>Creating and using a mock object is much like using a stub, except that a mock will do a little more than a stub: it will save the history of communication, which will later be verified in the form of *expectations*.

假設今天LogAnalyzer需要與webservice溝通，當檔名太長會發出exception。

第一步：純手工mock

```c#
public interface IWebService
{
    void LogError(string message);
}

public class FakeWebService:IWebService
{
    public string LastError;
    public void LogError(string message)
    {
        LastError = message;
    }
}
```

`Testing the LogAnalyzer with a mock object`

```c#
[Test]
public void Analyze_TooShortFileName_CallsWebService()
{
    FakeWebService mockService = new FakeWebService();
    LogAnalyzer log = new LogAnalyzer(mockService);
    string tooShortFileName="abc.ext";
    log.Analyze(tooShortFileName);
    StringAssert.Contains("Filename too short:abc.ext",
    mockService.LastError);
}
public class LogAnalyzer
{
    private IWebService service;
    public LogAnalyzer(IWebService service)
    {
        this.service = service;
    }
    public void Analyze(string fileName)
    {
        if(fileName.Length<8)
        {
            service.LogError("Filename too short:"
            + fileName);
        }
    }
}
```

Also notice that you aren’t writing the tests directly inside the mock object code. There are a couple of reasons for this:
-  You’d like to be able to reuse the mock object in other test cases, with other asserts on the message.
- If the assert were put inside the handwritten fake class, whoever reads the test would have no idea what you’re asserting. You’d be hiding essential information from the test code, which hinders the readability and maintainability of the test.

在一個測試裡combo多個stub和一個mock是正常的，但如果你有多個mock就代表有問題，因為你一次測太多東西。

### Using a mock and a stub together

考慮更複雜的情況，當web的exception發出時需要寄mail

```c#
if(fileName.Length<8)
{
    try
    {
        service.LogError("Filename too short:" + fileName);
    }
    catch (Exception e)
    {
        email.SendEmail("a","subject",e.Message);
    }
}
```

解決方法：stub webservice, mock mailservice

`Testing the LogAnalyzer with a mock and a stub`

```c#
public interface IEmailService
{
    void SendEmail(string to, string subject, string body);
}
public class LogAnalyzer2
{
    public LogAnalyzer2(IWebService service, IEmailService email)
    {
        Email = email,
        Service = service;
    }
    public IWebService Service
    {
        get ;
        set ;
    }
    public IEmailService Email
    {
        get ;
        set ;
    }
    public void Analyze(string fileName)
    {
        if(fileName.Length<8)
        {
            try
            {
                Service.LogError("Filename too short:" + fileName);
            }
            catch (Exception e)
            {
                Email.SendEmail("someone@somewhere.com",
                "can’t log",e.Message);
            }
        }
    }
}
[TestFixture]
public class LogAnalyzer2Tests
{
    [Test]
    public void Analyze_WebServiceThrows_SendsEmail()
    {
        FakeWebService stubService = new FakeWebService();
        stubService.ToThrow= new Exception("fake exception");
        FakeEmailService mockEmail = new FakeEmailService();
        LogAnalyzer2 log = new LogAnalyzer2(stubService,mockEmail);
        string tooShortFileName="abc.ext";
        log.Analyze(tooShortFileName);
        StringAssert.Contains("someone@somewhere.com",mockEmail.To);
        StringAssert.Contains("fake exception",mockEmail.Body);
        StringAssert.Contains("can’t log",mockEmail.Subject);
    }
}
public class FakeWebService:IWebService
{
    public Exception ToThrow;
    public void LogError(string message)
    {
        if(ToThrow!=null)
        {
            throw ToThrow;
        }
    }
}
public class FakeEmailService:IEmailService
{
    public string To;
    public string Subject;
    public string Body;
    public void SendEmail(string to,
    string subject,
    string body)
    {
        To = to;
        Subject = subject;
        Body = body;
    }
}
```

關於一次assert多個的另外一種處理方式：

```c#
class EmailInfo
{
    public string Body;
    public string To;
    public string Subject;
}
[Test]
public void Analyze_WebServiceThrows_SendsEmail()
{
    FakeWebService stubService = new FakeWebService();
    stubService.ToThrow= new Exception("fake exception");
    FakeEmailService mockEmail = new FakeEmailService();
    LogAnalyzer2 log = new LogAnalyzer2(stubService,mockEmail);
    string tooShortFileName="abc.ext";
    log.Analyze(tooShortFileName);
    EmailInfo expectedEmail = new EmailInfo {
    Body = "fake exception",
    To = "someone@somewhere.com",
    Subject = "can’t log" }
    Assert.AreEqual(expectedEmail, mockEmail.email);
}
public class FakeEmailService:IEmailService
{
    public EmailInfo email = null;
    public void SendEmail(EmailInfo emailInfo)
    {
        email = emailInfo;
    }
}
```

### One mock per test

一個測試裡應該只有一個mock其他都是stub，如果有超過一個mock代表你一次測太多東西。當你的測試太複雜的時候記得問自己誰才是mock，如果你對一個stub做assert，注意這是overspecification的特徵。

>*Overspecification* is the act of specifying too many things that should happen that your test shouldn’t care about; for example, that stubs were called.

Overspecification會導致你檢查太多東西，有可能測試結果是好的結果你的測試是壞的。

### Fake chains: stubs that produce mocks or other stubs

當你遇到這種情況：

```c#
IServiceFactory factory = GetServiceFactory();
IService service = factory.GetService();
```

或這種

```c#
String connstring =
GlobalUtil.Configuration.DBConfiguration.ConnectionString
```

這種時候你要stub一坨拉庫東西最後可能還要一個mock，以下是更乾淨的作法：

```c#
String connstring =GetConnectionString();
Protected virtual string GetConnectionString()
{
    Return GlobalUtil.Configuration.DBConfiguration.ConnectionString;
}
```

>**TIP** Another good way to avoid call chains is to create special wrapper classes around the API that simplify using and testing it. For more about this method, see *Working Effectively with Legacy Code* by Michael Feathers. The pattern is called Adapt Parameter in that book.

### The problems with handwritten mocks and stubs

There are several issues that crop up when using manual mocks and stubs:

- It takes time to write the mocks and stubs.
- It’s difficult to write stubs and mocks for classes and interfaces that have many methods, properties, and events.
- To save state for multiple calls of a mock method, you need to write a lot of boilerplate code within the handwritten fakes.
- If you want to verify that all parameters on a method call were sent correctly by the caller, you’ll need to write multiple asserts. That’s a drag.
- It’s hard to reuse mock and stub code for other tests. The basic stuff works, but once you get into more than two or three methods on the interface, everything starts getting tedious to maintain.
- Is there a place for a fake that is both a mock and a stub? Very rarely. And I mean maybe once or twice in a project. I’ve only seen this a couple of times in the past couple of years myself.

### Summary

- A mock object is like a stub, but it also helps you to assert something in your test.
- Combining stubs and mocks in the same test is a powerful technique, but you must take care to have no more than one mock in each test.
- Stubs that produce other stubs or mocks can be a powerful way to inject fake dependencies into code that uses other objects to get its data.
- One of the most common problems encountered by people who write tests is using mocks too much in their tests (overspecification).
- You may find that writing manual mocks and stubs is inconvenient for large interfaces or for complicated interaction-testing scenarios. It is, and there are better ways to do this, as you’ll see in the next chapter. But often you’ll find that handwritten mocks and stubs still beat frameworks for simplicity and readability. The art lies in when you use which tool.

## Chapter 5: Isolation(mocking) frameworks

*isolation framework*登場—a reusable library that can create and configure fake objects *at runtime*. These objects are referred to as *dynamic stubs* and *dynamic mocks*.  
使用`NSubstitute`

### Why use isolation frameworks

>**DEFINITION** An *isolation framework* is a set of programmable APIs that makes creating fake objects much simpler, faster, and shorter than hand-coding them.

```c#
public interface IComplicatedInterface
{
    void Method1(string a, string b, bool c, int x, object o);
    void Method2(string b, bool c, int x, object o);
    void Method3(bool c, int x, object o);
}
```

`Implementing complicated interfaces with handwritten stubs`

```c#
class MytestableComplicatedInterface:IComplicatedInterface
{
    public string meth1_a;
    public string meth1_b,meth2_b;
    public bool meth1_c,meth2_c,meth3_c;
    public int meth1_x,meth2_x,meth3_x;
    public int meth1_0,meth2_0,meth3_0;
    public void Method1(string a, string b, bool c, int x, object o)
    {
        meth1_a = a;
        meth1_b = b;
        meth1_c = c;
        meth1_x = x;
        meth1_0 = 0;
    }
    public void Method2(string b, bool c, int x, object o)
    {
        meth2_b = b;
        meth2_c = c;
        meth2_x = x;
        meth2_0 = 0;
    }
    public void Method3(bool c, int x, object o)
    {
        meth3_c = c;
        meth3_x = x;
        meth3_0 = 0;
    }
}
```

手寫這個fake除了花時間、笨重以外，想像萬一今天你想要測試某個方法被呼叫幾次怎麼辦?或是你想要回傳特定值?或是你想要呼叫歷史?你的程式馬上就會變得很醜。

### Dynamically creating a fake object

>**DEFINITION** A *dynamic fake object* is any stub or mock that’s created at runtime without needing to use a handwritten (hardcoded) implementation of that object.

本書使用[NSubstitute](http://nsubstitute.github.com/)，不過提到FakeItEasy也不錯。

`Asserting against a handwritten fake object`

```c#
[TestFixture]
class LogAnalyzerTests
{
    [Test]
    public void Analyze_TooShortFileName_CallLogger()
    {
        FakeLogger logger = new FakeLogger();
        LogAnalyzer analyzer = new LogAnalyzer(logger);
        analyzer.MinNameLength= 6;
        analyzer.Analyze("a.txt");
        StringAssert.Contains("too short",logger.LastError);
    }
}
class FakeLogger: ILogger
{
    public string LastError;
    public void LogError(string message)
    {
        LastError = message;
    }
}
```

輕鬆轉換

`Faking an object using NSub`

```c#
[Test]
public void Analyze_TooShortFileName_CallLogger()
{
    ILogger logger = Substitute.For<ILogger>();
    LogAnalyzer analyzer = new LogAnalyzer(logger);
    analyzer.MinNameLength = 6;
    analyzer.Analyze("a.txt");
    logger.Received().LogError("Filename too short: a.txt");
}
```

題外話：以往要mock可不是這麼方便，要先recordy再replay，經過漫長的進化才變成現在支援Arrange-act-assert。

### Simulating fake values

stub的使用方式

`Returning a value from a fake object`

```c#
[Test]
public void Returns_ByDefault_WorksForHardCodedArgument()
{
    IFileNameRules fakeRules = Substitute.For<IFileNameRules>();
    fakeRules.IsValidLogFileName("strict.txt").Returns(true);
    Assert.IsTrue(fakeRules.IsValidLogFileName("strict.txt"));
}
```

限制特定類型的參數，這個叫做`argument matcher`

```c#
[Test]
public void Returns_ByDefault_WorksForHardCodedArgument()
{
    IFileNameRules fakeRules = Substitute.For<IFileNameRules>();
    fakeRules.IsValidLogFileName(Arg.Any<String>())
    .Returns(true);
    Assert.IsTrue(fakeRules.IsValidLogFileName("anything.txt"));
}
```

固定丟出特定exception，可讀性降低不少  
作者補充：(This would be
easier to do in FakeItEasy, in fact, but NSub has more docs, so I chose to use it here.)

```c#
[Test]
public void Returns_ArgAny_Throws()
{
    IFileNameRules fakeRules = Substitute.For<IFileNameRules>();
    fakeRules.When(x =>
    x.IsValidLogFileName(Arg.Any<string>()))
    .Do(context =>
    { throw new Exception("fake exception"); });
    Assert.Throws<Exception>(() =>
    fakeRules.IsValidLogFileName("anything"));
}
```

以下示範`融合`：

`The method under test and a test that uses handwritten mocks and stubs`

```c#
    [Test]
    public void Analyze_LoggerThrows_CallsWebService()
    {
        FakeWebService mockWebService = new FakeWebService();
        FakeLogger2 stubLogger = new FakeLogger2();
        stubLogger.WillThrow = new Exception("fake exception");
        var analyzer2 =
        new LogAnalyzer2(stubLogger, mockWebService);
        analyzer2.MinNameLength = 8;
        string tooShortFileName="abc.ext";
        analyzer2.Analyze(tooShortFileName);
        Assert.That(mockWebService.MessageToWebService,
        Is.StringContaining("fake exception"));
    }
}
public class FakeWebService:IWebService
{
    public string MessageToWebService;
    public void Write(string message)
    {
        MessageToWebService = message;
    }
}
public class FakeLogger2:ILogger
{
    public Exception WillThrow = null;
    public string LoggerGotMessage = null;
    public void LogError(string message)
    {
        LoggerGotMessage = message;
        if (WillThrow != null)
        {
            throw WillThrow;
        }
    }
}
//---------- PRODUCTION CODE
public class LogAnalyzer2
{
    private ILogger _logger;
    private IWebService _webService;
    public LogAnalyzer2(ILogger logger,IWebService webService)
    {
        _logger = logger;
        _webService = webService;
    }
    public int MinNameLength { get; set; }
    public void Analyze(string filename)
    {
        if (filename.Length<MinNameLength)
        {
            try
            {
                _logger.LogError(
                string.Format("Filename too short: {0}",filename));
            }
            catch (Exception e)
            {
                _webService.Write("Error From Logger: " + e);
            }
        }
    }
}
public interface IWebService
{
    void Write(string message);
}
```

上面手寫，下面用Nsub

`Converting the previous test into one that uses NSubstitute`

```c#
[Test]
public void Analyze_LoggerThrows_CallsWebService()
{
    var mockWebService = Substitute.For<IWebService>();
    var stubLogger = Substitute.For<ILogger>();
    stubLogger.When(
        logger => logger.LogError(Arg.Any<string>()))
    .Do(info => { throw new Exception("fake exception");});
    var analyzer =
    new LogAnalyzer2(stubLogger, mockWebService);
    analyzer.MinNameLength = 10;
    analyzer.Analyze("Short.txt");
    mockWebService.Received()
    .Write(Arg.Is<string>(s => s.Contains("fake exception")));
}
```

- lambda雖然讓可讀性降低，不過這是生活在C#的必要之惡，同時可以避免用string呼叫方法，這會讓你refactor時較方便。
- argument matchers可以用在stub也可以用在mock ，[更多的Argument matchers](http://nsubstitute.github.io/help/argument-matchers/)

下面COMPARING OBJECTS AND PROPERTIES AGAINST EACH OTHER  
假設今天要確認ErrorInfo這個object內容：

```c#
[Test]
public void
Analyze_LoggerThrows_CallsWebServiceWithNSubObject()
{
    var mockWebService = Substitute.For<IWebService>();
    var stubLogger = Substitute.For<ILogger>();
    stubLogger.When(
    logger => logger.LogError(Arg.Any<string>()))
    .Do(info => { throw new Exception("fake exception");});
    var analyzer =
    new LogAnalyzer3(stubLogger, mockWebService);
    analyzer.MinNameLength = 10;
    analyzer.Analyze("Short.txt");
    mockWebService.Received()
    .Write(Arg.Is<ErrorInfo>(info => info.Severity == 1000
    && info.Message.Contains("fake exception")));
}
```

那個`And`實在很醜，有些時候isolation frameworks用的越深可讀性越差，必須要自己做取捨，也可以考慮回去使用`手寫`版fake，或者使用`expected object`：

`Comparing full objects`

```c#
[Test]
public void
Analyze_LoggerThrows_CallsWebServiceWithNSubObjectCompare()
{
    var mockWebService = Substitute.For<IWebService>();
    var stubLogger = Substitute.For<ILogger>();
    stubLogger.When(
    logger => logger.LogError(Arg.Any<string>()))
    .Do(info => { throw new Exception("fake exception");});
    var analyzer =
    new LogAnalyzer3(stubLogger, mockWebService);
    analyzer.MinNameLength = 10;
    analyzer.Analyze("Short.txt");
    var expected = new ErrorInfo(1000, "fake exception");
    mockWebService.Received().Write(expected);
}
```

Testing full objects only works when the following are true:

- It’s easy to create the object with the expected properties.
- You want to test all the properties of the object in question.
- You know the exact values of each property, fully.
- The `Equals()` method is implemented correctly on the two objects being compared. (It’s usually bad practice to rely on the out-of-the-box implementation of `object.Equals()`. If `Equals()` is not implemented, then this test will always fail, because by default `Equals()` will return `false`.)

注意比較整個object是會導致你的測試非常脆弱的，隨便一改動測試就會壞掉，你會因為不是正確的原因(有bug產生)而修正測試，而且比較整個object會讓你無法使用argument matchers，有可能一個string只是多了一個空白就讓測試壞掉，另一個選擇是不要比對整個object，你可以選幾個properties搭配argument matchers做比對就好。

### Testing for event-related activities

Events are a two-way street, and you can test them in two different directions:

- Testing that someone is listening to an event
- Testing that someone is triggering an event

`Event-related code and how to trigger it`

```c#
class Presenter
{
    private readonly IView _view;
    public Presenter(IView view)
    {
        _view = view;
        this._view.Loaded += OnLoaded;
    }
    private void OnLoaded()
    {
        _view.Render("Hello World");
    }
}
public interface IView
{
    event Action Loaded;
    void Render(string text);
}
//------ TESTS
[TestFixture]
public class EventRelatedTests
{
    [Test]
    public void ctor_WhenViewIsLoaded_CallsViewRender()
    {
        var mockView = Substitute.For<IView>();
        Presenter p = new Presenter(mockView);
        mockView.Loaded += Raise.Event<Action>();
        mockView.Received()
        .Render(Arg.Is<string>(s => s.Contains("Hello World")));
    }
}
```

Notice the following:

- The mock is also a stub (you simulate an event).(raise event是stub行為，對他assert是mock行為)
- To trigger an event, you have to awkwardly register to it in the test. This is only to satisfy the compiler, because event-related properties are treated differently and are heavily guarded by the compiler. Events can only be directly invoked by their declaring class/struct.


以下為使用stub raise event使用mock assert

`Simulating an event along with a separate mock`

```c#
[Test]
public void ctor_WhenViewhasError_CallsLogger()
{
    var stubView = Substitute.For<IView>();
    var mockLogger = Substitute.For<ILogger>();
    Presenter p = new Presenter(stubView, mockLogger);
    stubView.ErrorOccured +=
    Raise.Event<Action<string>>("fake error");
    mockLogger.Received()
    .LogError(Arg.Is<string>(s => s.Contains("fake error")));
}
```

下面示範如何確認event有在正確的時間被fire

`Using an anonymous delegate to register to an event`

```c#
[Test]
public void EventFiringManual()
{
    bool loadFired = false;
    SomeView view = new SomeView();
    view.Load+=delegate
    {
        loadFired = true;
    };
    view.DoSomethingThatEventuallyFiresThisEvent();
    Assert.IsTrue(loadFired);
}
```

### Current isolation frameworks for .NET

從isolation frameworks的數量就可以知道這東西的需求是高的。本書的append有更多的比較，從中選一個就好，可以降低team member的學習曲線。

>**Why method strings are bad inside tests**  
In many frameworks outside the .NET world, it’s common to use strings to describe
which methods you’re about to change the behavior of. Why is this not great?
If you were to change the name of a method in production, any tests using the
method in a string would still compile and would only break at runtime, throwing an exception indicating that a method could not be found.
>
>With strongly typed method names (thanks to lambda expressions and delegates),
changing the name of a method wouldn’t be a problem, because the method is used
directly in the test. Any method changes would keep the test from compiling, and
you’d know immediately that there was a problem with the test.
>
>With automated refactoring tools like those in Visual Studio, renaming a method is easier, but most refactorings will still ignore strings in the source code. (ReSharper for .NET is an exception. It also corrects strings, but that’s only a partial solution that may prove problematic in some scenarios.)

### Advantages and traps of isolation frameworks

優點：

- Easier parameter verification—Using handwritten mocks to test that a method was given the correct parameter values can be a tedious process, requiring time and patience. Most isolation frameworks make checking the values of parameters passed into methods a trivial process even if there are many parameters.
- Easier verification of multiple method calls—With manually written mocks, it can be difficult to check that multiple method calls on the same method were made correctly with each having appropriate different parameter values. As you’ll see later, this is a trivial process with isolation frameworks.
- Easier fakes creation—Isolation frameworks can be used for creating both mocks and stubs more easily.

可能的缺點：

- overusing an isolation framework when a manual mock object would suffice
- making tests unreadable because of overusing mocks in a test, or 
- not separating tests well enough.

隨時注意你的測試：

- Unreadable test code
- Verifying the wrong things
- Having more than one mock per test
- Overspecifying the tests

以下建議

- 如果你發現你的測試越來越難懂，可以試著拆分
- 測試一個object有沒有註冊一個事件並沒有告訴你任何這個object的功能，測試當事件觸發會發生一些事是比較好的方法
- 通常一個測試指側一件事才是好的，如果你有超過一個mock代表你可能測超過一件事，如果你不知道怎麼為你的測試命名可能也是同樣情況
- 盡可能不使用mock，只有當非用不可時，且應該不是常態
- 如果你有超過5%的mock objects，你有可能就是overspecifying
- 如果你的expectations(x.received().X() and X.received().Y() and so on)，你的測試可能會非常脆弱
- 測試互動是雙面刃，測太多你會看不到全貌，測太少有可能漏掉重要的互動

以下是針對`overspecifying`你可以做的一些事：

- *Use nonstrict mocks when you can (strict and nonstrict mocks are explained in the next chapter)*. The test will break less often because of unexpected method calls. This helps when the private methods in the production code keep changing.
- *Use stubs instead of mocks when you can*. If you have more than 5% of your tests with mock objects, you might be overdoing it. Stubs can be everywhere. Mocks, not so much. You only need to test one scenario at a time. The more mocks you have, the more verifications will take place at the end of the test, but usually only one will be the important one. The rest will be noise against the current test scenario.
- *Avoid using stubs as mocks if humanly possible*. Use a stub only for faking return values into the program under test or to throw exceptions. Don’t verify that methods were called on stubs. Use a mock only for verifying that some method was called on it, but don’t use it to return values into your program under test. Most of the time, you can avoid a mock that’s also a stub but not always (as you saw earlier in this chapter, regarding events).

### Summary

- Mock最後再使用
- 如果你的mock object超過5%你可能overspecifying
- 如果你使用isolation framework寫的測試開始變醜，考慮手寫mock或是拆分測試
- 如果你各種失敗就是寫不出測試你有三種選擇：
  - use a super framework like Typemock Isolator (explained in the next chapter)
  - change the design
  - quit your job

## Chapter 6: Digging deeper into isolation frameworks

The world of isolation frameworks

### Constrained and unconstrained frameworks

"作者自己分類的"

#### Constrained frameworks

>Constrained frameworks in .NET include Rhino Mocks, Moq, NMock, EasyMock, NSubstitute, and FakeItEasy. In Java, jMock and EasyMock are examples of constrained frameworks.

Constrained就是受語言本身與compiler限制。

>In .NET, constrained frameworks are unable to fake static methods, nonvirtual methods, nonpublic methods, and more.

Unconstrained frameworks就是靠黑科技幹壞事。

>*Unconstrained* frameworks in .NET include Typemock Isolator, JustMock, and Moles (a.k.a. MS Fakes). In Java, PowerMock and JMockit are examples of unconstrained frameworks. In C++, Isolator++ and Hippo Mocks are examples of such frameworks.

.NET使用的黑客計較做*profiling* APIs

>In .NET, all unconstrained frameworks are profiler-based. That means they use a set of unmanaged APIs called the profiling APIs that are wrapped around the running instance of the CLR—the Common Language Runtime—in .NET.

簡單來說這東西包含所有CLR code執行時的事件，可以在IL compile之前偷串改，因為事件包含所有程式碼，所以可以對private constructor, static method甚至是不受你控制的第三方程式碼fake。

profiling default為關閉需要被啟用，不過isolation framework通常有add-on可以做這件事。

unconstrained isolation frameworks的優點：

- You can write unit tests for previously untestable code, because you can fake things around the unit of work and isolate it, without needing to touch and refactor the code. Later, when you have tests, you can start refactoring.
- You can fake third-party systems that you can’t control and that are potentially very hard to test with, such as if your objects have to inherit from the base class of a third-party product that contains many dependencies at a lower level (SharePoint, CRM, Entity Framework, or Silverlight, to name a few).
- You can choose your own level of design, rather than be forced into specific patterns. Design isn’t created by a tool; it’s a people issue. If you don’t know what you’re doing, a tool won’t help you anyway. I talk more about this in chapter 11.

unconstrained isolation frameworks的缺點：

- If you don’t pay close attention, you can fake your way into a corner by faking things that aren’t needed, instead of looking at the unit of work at a higher level.
- If you don’t pay close attention, some tests can become unmaintainable because you’re faking APIs that you don’t own. This can happen, but not as often as you might think. From my experience, if you fake a low-enough level of an API in a framework, it’s very unlikely to change in the future. The deeper an API is, the more likely many things are built on top of it, and the less likely it is to change.

關於.NET的unconstrained isolation frameworks

- 使用C++寫成和CLR Profiler API對接(Typemock Isolator)
- Typemock Isolator有專利，可是沒有強制使用的樣子(所以才有其他的feameworks像是JustMock and Moles)
- profiling APIs文件很少(或許是故意的?)，不過可以查 *JitCompilationStarted* and *SetILFunctionBody*
- 理論上所有isolation frameworks的能力應該是一樣的，但實際上還是看該framework提供了甚麼功能給你
- Typemock歷史最悠久，MS Fakes提供一些fake原生lib的功能

Profiler-based frameworks會有效能影響，可是很小，比起他帶來的好處可以無視。

腳本語言像Ruby, Python, JavaScript等等，沒有像C#這麼好用且可讀性佳的isolation frameworks，有可能是框架不夠成熟也可能是測試還沒達到像.NET這樣相同的結論，不過也有可能我們做的事是錯的?

Good isolation frameworks have what I call the big two values:
- Future-proofing
- Usability

`Future-proofing`就是你只能因為正確的理由使測試壞掉(ex:bug)，而`Usability`則是isolation frameworks必須好懂且容易使用，以下是使測試更robust的特徵：

- Recursive fakes
- Ignored arguments by default
- Wide faking
- Nonstrict behavior of fakes
- Nonstrict mocks

#### Recursive fakes

範例：

```c#
public interface IPerson
{
    IPerson GetManager();
}
[Test]
public void RecursiveFakes_work()
{
    IPerson p = Substitute.For<IPerson>();
    Assert.IsNotNull(p.GetManager());
    Assert.IsNotNull(p.GetManager().GetManager());
    Assert.IsNotNull(p.GetManager().GetManager().GetManager());
}
```

#### Ignored arguments by default

好像只有Typemock Isolator有，一直打`Arg.IsAny<Type>`這種東西很浪費時間

#### Wide faking

Typemock屌屌的

```c#
Isolate.Fake.StaticMethods(typeof(HttpRuntime));
```

#### Nonstrict behavior of fakes

如果你的fake太嚴格就會讓測試很容易壞掉，例如在`unit of work`裡面告訴測試某個method應該被呼叫，往後重構使得這個method不再被呼叫可是你的`unit of work`結果還是正確的，卻因此導致測試壞掉。

#### Nonstrict mocks

Argument matching就是一種nonstrict mocks，可以接受任何一種call的mock也是

### Isolation framework design antipatterns

Here are some of the antipatterns found in frameworks today that we can easily alleviate:

- Concept confusion: 例如api名稱包含mock讓你分不清誰是mock誰是stub
- Record and replay: 難讀
- Sticky behavior: 容易讓測試壞
- Complex syntax: 難用

#### Concept confusion

例如api名稱包含mock讓你分不清誰是mock誰是stub

### Summary

- Isolation frameworks are divided into two categories: constrained and unconstrained frameworks.
- profiling APIs
- future-proofing and usability

## Chapter 7: Test hierarchies and organization

講如何配置你的測試們

### Automated builds running automated tests

If you plan to make your team more agile and equipped to handle requirement changes as they come into your shop, you need to be able to do the following:
- Make a small change to your code.
- Run all the tests to make sure you haven’t broken any existing functionality.
- Make sure your code can still integrate well and not break any other projects you depend on.
- Create a deliverable package of your code and deploy it automatically at the push of a button.

在講CI

- 你可能需要各種build configurations和build scripts
- 把你的build script一起上version control才可以串CI

A build process is a logical concept:

- encompassing build scripts
- build integration servers
- build triggers
- shared team understanding and acceptance of how code is deployed and integrated

團隊的agreement很重要(制度阿)

把你的build拆分，才可以當成是function配合CI呼叫

- A continuous integration (CI) build script - compile debug mode and run fast tests
- A nightly build script - build release, run slow tests, deploy to stage...
- A deployment build script - essentially a delivery mechanism

>I call them nightly builds, but they can be run many times a day. At the very least, they run once a night. They give more feedback but take more time to give it.

有些CI servers同時提供build script-related tasks，不過build script應該要一起進版控，這樣回到舊版本時才會有正確的build script

作者不愛用MSBuild因為他覺得xml很難maintain，你會好幾個月睡覺都夢到一堆tag

A CI server’s main jobs are these:

- Trigger a build script based on specific events
- Provide build script context and data such as version, source code, and artifacts
- Provide an overview of build history and metrics
- Provide the current status of all the active and inactive builds

>A trigger can start a build script automatically when certain
events occur, such as source control updates, time passing, or another build configuration failing or succeeding.

你可能會執行一連串的工作來完成最後的build，稱他做build configuration，建議把她限制在一份可執行的build script並進入版控，確保工作的相容性最大化

>Artifacts are the end results of running a build script. They could be binary files, configuration files, or any type of file.

### Mapping out tests based on speed and type

把單元測試與整合測試分開，不需要分成不同的test projects，不同的folder或namespace就夠了。

不分開會發生甚麼事?`people not running your tests`

測試壞掉的幾種可：

- There’s a bug in the code under test.
- The test has a problem in the way it’s written.
- The test is no longer relevant.
- The test requires some configuration to run.

最後一種與開發者最無關，通常會被忽略因為認為他不重要，當這種整合測試藏在你的測試裡面時，很容易浪費時間跟金錢去找不存在的問題，而且每一次發生都會使工程師失去對測試的信任，下次又有測試壞掉的時候可能只會說"噢~那測試偶爾會壞掉，沒事兒沒事兒~"

#### The safe green zone

創造只有unit test的The safe green zone讓工程師不會有`沒事兒沒事兒`心態，當The safe green zone沒過就代表真的有問題而不是configuration出問題了。

這當然不代表整合測試就不應該過，只是整合測試通常比較慢，分開可以讓工程師產能更高，跑更多次的unit test，而在每次unit test過的時候有某些程度的信心。

分開也可以讓整合測試有自己確保能通過測試的環境，也可以詳細的文件記錄如何configuration。

### Ensuring tests are part of source control

廢話

### Mapping test classes to code under test

測試的配置應該滿足以下:

- Look at a project and find all the tests that relate to it
- Look at a class and find all the tests that relate to it
- Look at a method and find all the tests that relate to it

作者的配置測試project的方式：

```c#
xxxProject
xxxProject.UnitTests
xxxProject.IntegrationTests
```

配置測試class有各種方式，討論主要的兩種

#### ONE TEST CLASS PER CLASS OR UNIT OF WORK UNDER TEST

```c#
xxxClass
xxxClass.UnitTests
```

>The one-test-class-per-class pattern (also mentioned in Meszaros’s *xUnit Test Patterns: Refactoring Test Code*) is the simplest and most common pattern for organizing tests.

一個class所有的method測試都塞在一個大測試class裡，缺點是有可能這個測試class會長太大讓你很難看，有時候一個方法的測試直接淹沒其他方法，這也表示那個方法的測試或許太多了。

#### ONE TEST CLASS PER FEATURE

An alternative is creating a separate test class for a particular feature (which could be as small as a method). The one-test-class-per-feature pattern is also mentioned in Meszaros’s book.

```c#
xxxClassTests
xxxClassTestsyyyMethod
```

### Cross-cutting concerns injection

>When you’re dealing with cross-cutting concerns such as time management, or exceptions, or logging, you might end up with code that’s less readable and maintainable when using these techniques.

假設有隻程式使用`DateTime`

```c#
public static class TimeLogger
{
    public static string CreateMessage(string info)
    {
        return DateTime.Now.ToShortDateString() + " " + info;
    }
}
```

而為了讓他能被測試你可能需要`ITimeProvider`，這會讓你每次用到`DateTime`都需要花時間寫這個interface，還讓你的程式變得難讀了。

另一種更好的做法：自己寫一個`SystemTime`包起來，同時確保你的程式不再使用`DateTime`

`Using the SystemTime class`

```c#
public static class TimeLogger
{
    public static string CreateMessage(string info)
    {
        return SystemTime.Now.ToShortDateString() + " " + info;
    }
}
public class SystemTime
{
    private static DateTime _date;
    public static void Set(DateTime custom)
    { _date = custom; }
    public static void Reset()
    { _date=DateTime.MinValue; }
    public static DateTime Now
    {
        get
        {
            if (_date != DateTime.MinValue)
            {
                return _date;
            }
            return DateTime.Now;
        }
    }
}
```

`A test using SystemTime`

```c#
[TestFixture]
public class TimeLoggerTests
{
    [Test]
    public void SettingSystemTime_Always_ChangesTime()
    {
        SistemTime.Set(new DateTime(2000,1,1));
        string output = TimeLogger.CreateMessage("a");
        StringAssert.Contains("01.01.2000", output);
    }
    [TearDown]
    public void afterEachTest()
    {
        SystemTime.Reset();
    }
}
```

有可能會碰到語言問題，Nunit的[CultureInfoAttribute](https://github.com/nunit/docs/wiki/Culture-Attribute)可以幫這個忙

警告：

>This type of external abstraction of a cross-cutting concern allows you to create a fake focal point in your production code instead of many small ones. But it only makes sense for things that are used throughout the system. If you use this for everything, you end up with a system that might be just as hard to read as what you’re trying to avoid.

如何確保大家都用SystemTime->code review或replace tool

### Building a test API for your application

你的測試越寫越多自然會慢慢也把測試重構、新增應用api等等：

- Use inheritance in your test classes for code reuse, guidance, and more.
- Create test utility classes and methods.
- Make your API known to developers.

#### Using test class inheritance patterns

**DRY!!**

>One of the most powerful arguments for object-oriented code is that you can reuse existing functionality instead of recreating it over and over again in other classes—what Andy Hunt and Dave Thomas called the DRY (“don’t repeat yourself”) principle in *The Pragmatic Programmer* (Addison-Wesley Professional, 1999).

阿因為你的測試也是用.NET寫的,不要對使用繼承感到罪惡XD

Implementing a base class can help alleviate standard problems in test code in the following ways:

- Reusing utility and factory methods
- Running the same set of tests over different classes (we’ll look at this one in more detail)
- Using common setup or teardown code (also useful for integration testing)
- Creating testing guidance for programmers who will derive from the base class

介紹三種使用繼承的測試pattern

- abstract test infrastructure class
- Template test class
- Abstract test driver class
  
和兩種可以使用在上面pattern的重構技巧

- Refactoring into a class hierarchy
- Using generics

##### ABSTRACT TEST INFRASTRUCTURE CLASS PATTERN

>The *abstract test infrastructure class pattern* creates an abstract test class that contains essential common infrastructure for test classes deriving from it. Scenarios where you’d want to create such a base class can range from having common setup and teardown code to having special custom asserts that are used throughout multiple test classes.

假設有個LoggingFacility可能很多地方會用到，例如這裡LogAnalyzer與ConfigurationManager都需要使用，這時候會在這兩個class的測試中各別fake LoggingFacility

```c#
//This class uses the LoggingFacility Internally
public class LogAnalyzer
{
    public void Analyze(string fileName)
    {
        if (fileName.Length < 8)
        {
            LoggingFacility.Log("Filename too short:" + fileName);
        }
        //rest of the method here
    }
}
//another class that uses the LoggingFacility internally
public class ConfigurationManager
{
    public bool IsConfigured(string configName)
    {
        LoggingFacility.Log("checking " + configName);
        return result;
    }
}
public static class LoggingFacility
{
    public static void Log(string text)
    {
        logger.Log(text);
    }
    private static ILogger logger;
    public static ILogger Logger
    {
        get { return logger; }
        set { logger = value; }
    }
}
[TestFixture]
public class LogAnalyzerTests
{
    [Test]
    public void Analyze_EmptyFile_ThrowsException()
    {
        LogAnalyzer la = new LogAnalyzer();
        la.Analyze("myemptyfile.txt");
        //rest of test
    }
    [TearDown]
    public void teardown()
    {
        // need to reset a static resource between tests
        LoggingFacility.Logger = null;
    }
}
[TestFixture]
public class ConfigurationManagerTests
{
    [Test]
    public void Analyze_EmptyFile_ThrowsException()
    {
        ConfigurationManager cm = new ConfigurationManager();
        bool configured = cm.IsConfigured("something");
        //rest of test
    }
    [TearDown]
    public void teardown()
    {
        // need to reset a static resource between tests
        LoggingFacility.Logger = null;
    }
}
```

創造一個應用型的base class來重構它，不使用[SetUp]因為會影響可讀性，而且會讓所有測試一定會執行它，看測試的人可能會不知道發生了甚麼事而必須去看base class

(我說這繼承用得還頗暴力)

`A refactored solution`

```c#
[TestFixture]
public class BaseTestsClass
{
    public ILogger FakeTheLogger()
    {
        LoggingFacility.Logger =
        Substitute.For<ILogger>();
        return LoggingFacility.Logger;
    }
    [TearDown]
    public void teardown()
    {
        // need to reset a static resource between tests
        LoggingFacility.Logger = null;
    }
}
[TestFixture]
public class ConfigurationManagerTests:BaseTestsClass
{
    [Test]
    public void Analyze_EmptyFile_ThrowsException()
    {
        FakeTheLogger();
        ConfigurationManager cm =
        new ConfigurationManager();
        bool configured = cm.IsConfigured("something");
        //rest of test
    }
}
[TestFixture]
public class LogAnalyzerTests : BaseTestsClass
{
    [Test]
    public void Analyze_EmptyFile_ThrowsException()
    {
        FakeTheLogger();
        LogAnalyzer la = new LogAnalyzer();
        la.Analyze("myemptyfile.txt");
        //rest of test
    }
}
```

缺點：影響可讀性，其他的開發者不確定如何使用這個base class的API，所以盡量少用，通常使用一個效果就很不錯了，然後千萬不要多層繼承。

##### TEMPLATE TEST CLASS PATTERN

>Let’s say you want to make sure people who test specific kinds of classes in the code never forget to go through a certain set of unit tests for them as they develop the classes; for example, network code with packets, security code, database-related code, or just plain-old parsing code. The point is, you know that when they work on this kind of class in code, some tests must exist because that kind of class has to provide a known set of services with its API.

我要叫他`強迫你要寫這些測試pattern`

假設你有個abstract class叫`BaseStringParser`，然後有三個子類別`XMLStringParser`, `IISLogStringParser`, and `StandardStringParser`，他們都支援從header取得本資訊的方法，首先挑一個寫：

`An outline of a test class for StandardStringParser`

```c#
[TestFixture]
public class StandardStringParserTests
{
    private StandardStringParser GetParser(string input)
    {
        return new StandardStringParser(input);
    }
    [Test]
    public void GetStringVersionFromHeader_SingleDigit_Found()
    {
        string input = "header;version=1;\n";
        StandardStringParser parser = GetParser(input
        string versionFromHeader = parser.GetStringVersionFromHeader();
        Assert.AreEqual("1",versionFromHeader);
    }
    [Test]
    public void GetStringVersionFromHeader_WithMinorVersion_Found()
    {
        string input = "header;version=1.1;\n";
        StandardStringParser parser = GetParser(input);
        //rest of the test
    }
    [Test]
    public void GetStringVersionFromHeader_WithRevision_Found()
    {
        string input = "header;version=1.1.1;\n";
        StandardStringParser parser = GetParser(input);
        //rest of the test
    }
}
```

注意上面使用了helper method `GetParser`而不是使用setup method，這樣才可以裡用constructor每次帶入自己的參數成為每個測試自己的parser

如何強迫子類別也要加入特定測試呢?=>繼承abstract class

`A template test class for testing string parsers`

```c#
[TestFixture]
public abstract class TemplateStringParserTests
{
    public abstract
    void TestGetStringVersionFromHeader_SingleDigit_Found();
    public abstract
    void TestGetStringVersionFromHeader_WithMinorVersion_Found();
    public abstract
    void TestGetStringVersionFromHeader_WithRevision_Found();
}
[TestFixture]
public class XmlStringParserTests : TemplateStringParserTests
{
    protected IStringParser GetParser(string input)
    {
        return new XMLStringParser(input);
    }
    [Test]
    public override
    void TestGetStringVersionFromHeader_SingleDigit_Found()
    {
        IStringParser parser = GetParser("<Header>1</Header>");
        string versionFromHeader = parser.GetStringVersionFromHeader();
        Assert.AreEqual("1",versionFromHeader);
    }
    [Test]
    public override
    void TestGetStringVersionFromHeader_WithMinorVersion_Found()
    {
        IStringParser parser = GetParser("<Header>1.1</Header>");
        string versionFromHeader = parser.GetStringVersionFromHeader();
        Assert.AreEqual("1.1",versionFromHeader);
    }
    [Test]
    public override
    void TestGetStringVersionFromHeader_WithRevision_Found()
    {
        IStringParser parser = GetParser("<Header>1.1.1</Header>");
        string versionFromHeader = parser.GetStringVersionFromHeader();
        Assert.AreEqual("1.1.1",versionFromHeader);
    }
}
```

注意`GetParser`只是個一般的method不含在abstract class，沒多說為什麼

>I use the word *Test* to prefix the abstract methods in the base class, so that people who override them in derived classes have an easier time finding what’s important to override.

##### ABSTRACT “FILL IN THE BLANKS” TEST DRIVER CLASS PATTERN

最終招：把測試寫在base，測試需要的一些實作交給子類別

`A “fill in the blanks” base test class`

```c#
public abstract class FillInTheBlanksStringParserTests
{
    protected abstract IStringParser GetParser(string input);
    protected abstract string HeaderVersion_SingleDigit { get; }
    protected abstract string HeaderVersion_WithMinorVersion {get;}
    protected abstract string HeaderVersion_WithRevision { get; }
    public const string EXPECTED_SINGLE_DIGIT = "1";
    public const string EXPECTED_WITH_REVISION = "1.1.1";
    public const string EXPECTED_WITH_MINORVERSION = "1.1";
    [Test]
    public void GetStringVersionFromHeader_SingleDigit_Found()
    {
        string input = HeaderVersion_SingleDigit;
        IStringParser parser = GetParser(input);
        string versionFromHeader = parser.GetStringVersionFromHeader();
        Assert.AreEqual(EXPECTED_SINGLE_DIGIT,versionFromHeader);
    }
    [Test]
    public void GetStringVersionFromHeader_WithMinorVersion_Found()
    {
        string input = HeaderVersion_WithMinorVersion;
        IStringParser parser = GetParser(input);
        string versionFromHeader = parser.GetStringVersionFromHeader();
        Assert.AreEqual(EXPECTED_WITH_MINORVERSION,versionFromHeader);
    }
    [Test]
    public void GetStringVersionFromHeader_WithRevision_Found()
    {
        string input = HeaderVersion_WithRevision;
        IStringParser parser = GetParser(input);
        string versionFromHeader = parser.GetStringVersionFromHeader();
        Assert.AreEqual(EXPECTED_WITH_REVISION,versionFromHeader);
    }
}
[TestFixture]
public class StandardStringParserTests : FillInTheBlanksStringParserTests
{
    protected override string HeaderVersion_SingleDigit
    {
        get
        {
            return string.Format("header\tversion={0}\t\n", EXPECTED_SINGLE_DIGIT);
        }
    }
    protected override string HeaderVersion_WithMinorVersion
    {
        get
        {
            return string.Format("header\tversion={0}\t\n", EXPECTED_WITH_MINORVERSION);
        }
    }
    protected override string HeaderVersion_WithRevision
    {
        get
        {
            return string.Format("header\tversion={0}\t\n", EXPECTED_WITH_REVISION);
        }
    }
    protected override IStringParser GetParser(string input)
    {
        return new StandardStringParser(input);
    }
}
```

子類別沒有任何測試

##### REFACTORING YOUR TEST CLASS INTO A TEST CLASS HIERARCHY

如何把既有測試重構成上面那樣?

Here’s a list of possible steps for refactoring your test class:

1. Refactor: extract the superclass.
    - Create a base class (BaseXXXTests).
    - Move the factory methods (like GetParser) into the base class.
    - Move all the tests to the base class.
    - Extract the expected outputs into public fields in the base class.
    - Extract the test inputs into abstract methods or properties that the derived classes will create.
2. Refactor: make factory methods abstract, and return interfaces.
3. Refactor: find all the places in the test methods where explicit class types are used, and change them to use the interfaces of those types instead.
4. In the derived class, implement the abstract factory methods and return the explicit types.

##### A VARIATION USING .NET GENERICS TO IMPLEMENT TEST HIERARCHY

>You can use generics as part of the base test class. This way, you don’t need to override any methods in derived classes; just declare the type you’re testing against.

`Implementing test case inheritance with .NET generics`

```c#
//An example of the same idea using Generics
public abstract class GenericParserTests<T> where T:IStringParser
{
    protected abstract string GetInputHeaderSingleDigit();
    protected T GetParser(string input
    {
        return (T) Activator.CreateInstance(typeof (T), input);
    }
    [Test]
    public void GetStringVersionFromHeader_SingleDigit_Found()
    {
        string input = GetInputHeaderSingleDigit();
        T parser = GetParser(input);
        bool result = parser.HasCorrectHeader();
        Assert.IsFalse(result);
    }
    //more tests
    //...
}
//An example of a test inheriting from a Generic Base Class
[TestFixture]
public class StandardParserGenericTests:GenericParserTests<StandardStringParser>
{
    protected override string GetInputHeaderSingleDigit()
    {
        return "Header;1";
    }
}
```

只是一個例子，generic看起來也沒有其他好處，尤其是測試不太需要這種性能提升

#### Creating test utility classes and methods

測試越寫越多就會慢慢產生各種utility methods

You might end up with the following types of utility methods:

- Factory methods for objects that are complex to create or that routinely get created by your tests.
- System initialization methods (such as methods for setting up the system state before testing, or changing logging facilities to use stub loggers).
- Object configuration methods (for example, methods that set the internal state of an object, such as setting a customer to be invalid for a transaction).
- Methods that set up or read from external resources such as databases, configuration files, and test input files (for example, a method that loads a text file with all the permutations you’d like to use when sending in inputs for a specific method and the expected results). This is more commonly used in integration or system testing.
- Special assert utility methods, which may assert something that’s complex or that’s repeatedly tested inside the system’s state. (If something was written to the system log, the method might assert that X, Y, and Z are true, but not G.)

You may end up refactoring your utility methods into these types of utility classes:

- Special assert utility classes that contain all the custom assert methods
- Special factory classes that hold the factory methods
- Special configuration classes or database configuration classes that hold integration-style actions

.NET世界有些不錯的uitlity frameworks例如*Fluent Assertions*

擁有這些unitlity methods不代表保證任何人都會使用它，你經常會遇到工程師重複造輪，所以你需要讓別人知道這些API的存在

### Making your API known to developers

如何讓你的APIs變有名?

- Have teams of two people write tests together (at least once in a while), where one is familiar with the existing APIs and can teach the other, as they write new tests, about the existing benefits and code that could be used.
- Have a short document (no more than a couple of pages) or a cheat sheet that details the types of APIs out there and where to find them. You can create short documents for specific parts of your testing framework (APIs specific to the data layer, for example) or a global one for the whole application. If it’s not short, no one will maintain it. One possible way to make sure it’s up to date is by automating the generation process:
    - Have a known set of prefixes or postfixes on the API helpers’ names (helper [something], for example).
    - Have a special tool that parses out the API names and their locations and generates a document that lists them and where to find them, or have some simple directives that the special tool can parse from comments you put on them.
    - Automate the generation of this document as part of the automated build process.
- Discuss changes to the APIs during team meetings—one or two sentences outlining the main changes and where to look for the significant parts. That way the team knows that this is important and it’s always a consideration.
- Go over this document with new employees during their orientation.

### Summary

- Whatever testing you do—however you do it—automate it, use an automated build process to run it as many times as possible during the day or night, and continuously deliver the product as much as possible.
- Separate the integration tests from the unit tests (the slow tests from the fast ones) so that your team can have a safe green zone where all the tests must pass.
- Map out tests by project and by type (unit versus integration tests, slow versus fast tests), and separate them into different directories, folders, or namespaces (or all of these). I usually use all three types of separation.
- Use a test class hierarchy to apply the same set of tests to multiple related types under test in a hierarchy or to types that share a common interface or base class.
- Use helper classes and utility classes instead of hierarchies if the test class hierarchy makes tests less readable, especially if there’s a shared setup method in the base class. Different people have different opinions on when to use which, but readability is usually the key reason for not using hierarchies.
- Make your API known to your team. If you don’t, you’ll lose time and money as team members unknowingly reinvent APIs over and over again.

## Chapter 8: The pillars of good unit tests

測試三本柱：

- *Trustworthiness*—Developers will want to run trustworthy tests, and they’ll accept the test results with confidence. Trustworthy tests don’t have bugs, and they test the right things.
- *Maintainability*—Unmaintainable tests are nightmares because they can ruin project schedules, or they may be sidelined when the project is put on a more aggressive schedule. Developers will simply stop maintaining and fixing tests that take too long to change or that need to change very often on very minor production code changes.
- *Readability*—This means not just being able to read a test but also figuring out the problem if the test seems to be wrong. Without readability, the other two pillars fall pretty quickly. Maintaining tests becomes harder, and you can’t trust them anymore because you don’t understand them.

### Writing trustworthy tests

所謂值得信賴的測試就是當它壞了你會認為真的是你的程式碼有問題。

一些方式幫助你的測試更值得信任：

- Decide when to remove or change tests
- Avoid test logic
- Test only one concern
- Separate unit from integration tests
- Push for code reviews as much as you push for code coverage

#### Deciding when to remove or change tests

簡單來說就是要定期整理你的測試程式拉

The main reason for removing a test is because it fails. A test can suddenly fail for
several reasons:

- Production bugs—There’s a bug in the production code under test.
- Test bugs—There’s a bug in the test.
- Semantics or API changes—The semantics of the code under test changed but not the functionality.
- Conflicting or invalid tests—The production code was changed to reflect a conflicting requirement.

There are also reasons for changing or removing tests when nothing is wrong with the
tests or code:

- To rename or refactor the test
- To eliminate duplicate tests

##### PRODUCTION BUGS

是最正常的情況，好棒棒。  

##### TEST BUGS

Bugs in tests are notoriously hard to detect, because tests are assumed to be correct.

測試程式長蟲之開發者四階段：

1. *Denial*—The developer will keep looking for a problem in the code itself, changing it, causing all the other tests to start failing. The developer introduces new bugs into production code while hunting for the bug that’s actually in the test.
2. *Amusement*—The developer will call another developer, if possible, and they will hunt for the nonexistent bug together.
3. *Debuggerment*—The developer will patiently debug the test and discover that there’s a problem in the test. This can take anywhere from an hour to a couple of days.
4. *Acceptance and slappage*—The developer will eventually realize where the bug is and will slap their forehead.

修正測試三步驟：

1. Fix the bug in your test.
2. Make sure the test fails when it should.
3. Make sure the test passes when it should.

再次重申當你修正完測試之後最好再確認一次當它正常的時候會pass有bug的時候會fail。

>I’ve seen developers accidentally delete the asserts from their tests when fixing bugs in tests. You’d be surprised how often that happens and how effective step 2 is at catching these cases.

##### SEMANTICS OR API CHANGES

舉個例，假設今天`LogAnalyzer`需要先`Initialize`才能使用，就會讓這個原本存在的測試fail

`A simple test against the LogAnalyzer class`

```c#
[Test]
public void SemanticsChange()
{
    LogAnalyzer logan = new LogAnalyzer();
    Assert.IsFalse(logan.IsValid("abc"));
}
```

修正測試：

`The changed test using the new semantics of LogAnalyzer`

```c#
[Test]
public void SemanticsChange()
{
    LogAnalyzer logan = new LogAnalyzer();
    logan.Initialize();
    Assert.IsFalse(logan.IsValid("abc"));
}
```

因為這種原因而改動測試的體驗是非常差的，尤其程式越長越大的時候。

`A refactored test using a factory method`

```c#
[Test]
public void SemanticsChange()
{
    LogAnalyzer logan = MakeDefaultAnalyzer();
    Assert.IsFalse(logan.IsValid("abc"));
}
public static LogAnalyzer MakeDefaultAnalyzer()
{
    LogAnalyzer analyzer = new LogAnalyzer();
    analyzer.Initialize();
    return analyzer;
}
```

使用`factory method`來降低這類維護成本，有些工具專門在做這件事情，例如`AutoFixture`，不過創個factory method也沒麼大不了。

#### CONFLICTING OR INVALID TESTS

去問你家PM到底要哪個規格拉?

>Seriously, if I catch another person commenting out something instead of deleting it, I will write a whole book titled *Why God Invented Source Control*.

#### RENAMING OR REFACTORING TESTS

就是改得更好讀

#### ELIMINATING DUPLICATE TESTS

重複的測試不一定是壞事：

- The more (good) tests you have, the more certain you are to catch bugs.
- You can read the tests and see different ways or semantics of testing the same thing.

要不要砍你自己決定：

Here are some of the cons of having duplicate tests:

- It may be harder to maintain several different tests that provide the same functionality.
- Some tests may be higher quality than others, and you need to review them all for correctness.
- Multiple tests may break when a single thing doesn’t work. (This may not really be undesirable.)
- Similar tests must be named differently, or the tests can be spread across different classes.
- Multiple tests may create more maintainability issues. 
  
Here are some pros:

- Tests may have small differences and so can be thought of as testing the same things slightly differently. They may make for a larger and better picture of the object being tested.
- Some tests may be more expressive than others, so more tests may improve the chances of test readability.

#### Avoiding logic in tests

測試如果加入越來越多的邏輯就越容易產生bug

>I’ve seen plenty of tests that should have been simple become dynamically logic-changing, random-number-generating, thread-creating file-writing monsters that are little test engines in their own right.

有時候這些東西是必要的，但通常會放在*integration test*而不是*unit test*。

unit test不該有這些東西：

- `switch`, `if`, or `else` statements
- `foreach`, `for`, or `while` loops

測試有邏輯通常代表一次測超過一種東西，且可讀性差又脆弱，很有可能隱藏bug。unit test不應該有control flows甚至連try-catch都不該有，這些會造成以下問題：

- The test is harder to read and understand.
- The test is hard to re-create. (Imagine a multithreaded test or a test with random numbers that suddenly fails.)
- The test is more likely to have a bug or to test the wrong thing.
- Naming the test may be harder because it does multiple things.

如果你必須要寫這種`monster test`，請不要用它來取代任何既有的測試，且必須標明它是`integration test`。

另一種依該要避免地含有邏輯的測試：

```c#
[Test]
public void ProductionLogicProblem()
{
    string user ="USER";
    string greeting="GREETING";
    string actual = MessageBuilder.Build(user,greeting);
    Assert.AreEqual(user + greeting,actual);
}
```

這個測試的問題在expected result是動態生成，他重複了production code的羅邏輯，因為寫測試與寫production code的通常是同一人。實際上這段測試跟production code同樣都少了一個空白。

```c#
[Test]
public void ProductionLogicProblem()
{
    string actual = MessageBuilder.Build("user","greeting");
    Assert.AreEqual"user greeting",actual);
}
```

在測試的世界`hardcod`才是王道。

#### Testing only one concern

>A concern, as explained before, is a single end result from a unit of work: a return value, a change to system state, or a call to a third-party object. For example, if your
unit test asserts on more than a single object, it may be testing more than one concern.

測試超過一件事聽起來不像壞事–直到當你需要為他命名或當測試沒過你需要知道為什麼的時候。

通常test frameworks會被設計一碰到exception就不會在往下跑，如果你沒有把測試拆分，你就沒辦法收集到更多的症狀來解決你的問題。

#### Separate unit from integration tests

再次強調一定要有`Safe green zone`，才能讓工程師相信你的測試願意跑你的測試。

>This green zone is easily created by having a separate unit tests project in which only tests that run in memory, are consistent, and are repeatable exist.

#### Assuring code review with code coverage

>What does it mean when you have 100% code coverage? Nothing, without a code review. Your CEO might have asked all employees to “get over 95% code coverage,” and they might have done exactly what they were asked. Maybe those tests don’t even have asserts. People tend to do what they need to do to achieve a given goal metric.

如果你做了code review和test review確認了所有測試沒問題且覆蓋了所有的code，你就創造了一張安全網防止自己幹蠢事，同時也分享給了你的團隊讓他們受惠。

coverage工具

>To ensure good coverage for your new code, use one of the automated tools (for example, dotCover from JetBrains, OpenCover, NCover, or Visual Studio Pro). My personal recommendation these days is NCrunch, which gives a real-time coverage red/green view of your code that changes as you’re coding.

When you add a new test that was missing, check whether you’ve added the correct
test with these steps:

1. Comment out the production code you think isn’t being covered.
2. Run all the tests.
3. If all the tests pass, you’re missing a test or are testing the wrong thing. Otherwise, there would have been a test somewhere that was expecting that line to be called or some resulting consequence of that line of code to be true, and that missing test would now fail.
4. Once you’ve found a missing test, you’ll need to add it. Keep the code commented out and write a new test that fails, proving that the code you’ve commented is missing.
5. Uncomment the code you commented before.
6. The test you wrote should now pass. You’ve detected and added a missing test!
7. If the test still fails, it means the test may have a bug or is testing the wrong thing. Modify the test until it passes. Now you’ll want to see that the test is OK, making sure it not only passes when it should, but also fails when it should. To make sure the test fails when it should, reintroduce the bug into your code (commenting out the line of production code) and see if the test indeed fails.

### Writing maintainable tests

>Maintainability is one of the core issues most developers face when writing unit tests. Eventually the tests seem to become harder and harder to maintain and understand, and every little change to the system seems to break one test or another, even if bugs don’t exist.

- Testing only against public contracts
- Removing duplication in tests
- Enforcing test isolation.

#### Testing private or protected methods

private method通常是開發者有個好的理由認為他應該是private method，如果對private method做測是有可能會非常脆弱，只要碰到refactor你的測試就會壞掉，儘管最後的整個功能還是正常的。public contract才應該是你測試的目標，如果你想測試一個private method，找到使用他的public method的use case做測試，如果你只針對private method做測試不代表所有的public method都正確使用這個private method。有時候你可能會看到值得測試的private method，你可以考慮以下解法：

##### MAKING METHODS PUBLIC

雖然好像違反OO原則，但把方法變公開不一定是壞事。想要對一個方法寫測試代表對呼叫他的代碼有已知的行為或契約，你只是把他變成`official`。保持private代表的是告訴其他開發者可以改變他的實作而不用怕有不知道的地方使用他。

##### EXTRACTING METHODS TO NEW CLASSES

>If your method contains a lot of logic that can stand on its own, or it uses state in the class that’s relevant only to the method in question, it may be a good idea to extract the method into a new class, with a specific role in the system. You can then test that class separately.

##### MAKING METHODS STATIC

>If your method doesn’t use any of its class’s variables, you might want to refactor the method by making it static. That makes it much more testable but also states that this method is a sort of utility method that has a known public contract specified by its name.

##### MAKING METHODS INTERNAL

>When all else fails, and you can’t afford to expose the method in an official way, you might want to make it internal and then use the [InternalsVisibleTo("Test- Assembly")] attribute on the production code assembly so that tests can still call that method. This is my least favorite approach, but sometimes there’s no choice (perhaps because of security reasons, lack of control over the code’s design, and so on).

#### Removing duplication

LogAnalyzer變得需要init的例子再講一次：

`A class under test and a test that uses it`

```c#
public class LogAnalyzer
{
    public bool IsValid(string fileName)
    {
        if (fileName.Length < 8)
        {
            return true;
        }
        return false;
    }
}
[TestFixture]
public class LogAnalyzerTestsMaintainable
{
    [Test]
    public void IsValid_LengthBiggerThan8_IsFalse()
    {
        LogAnalyzer logan = new LogAnalyzer();
        bool valid = logan.IsValid("123456789");
        Assert.IsFalse(valid);
    }
}
```

`Two tests with duplication`

```c#
[Test]
public void IsValid_LengthBiggerThan8_IsFalse()
{
    LogAnalyzer logan = new LogAnalyzer();
    bool valid = logan.IsValid("123456789");
    Assert.IsFalse(valid);
}
[Test]
public void IsValid_LengthSmallerThan8_IsTrue()
{
    LogAnalyzer logan = new LogAnalyzer();
    bool valid = logan.IsValid("1234567");
    Assert.IsTrue(valid);
}
```

`LogAnalyzer with changed semantics that now requires initialization`

```c#
public class LogAnalyzer
{
    private bool initialized=false;
    public bool IsValid(string fileName)
    {
        if(!initialized)
        {
            throw new NotInitializedException(
            "The analyzer.Initialize() method should be" +
            "called before any other operation!");
        }
        if (fileName.Length < 8)
        {
            return true;
        }
        return false;
    }
    public void Initialize()
    {
        //initialization logic here
        ...
        initialized=true;
    }
}
```

##### REMOVING DUPLICATION USING A HELPER METHOD

`Adding the Initialize() call in the factory method`

```c#
[Test]
public void IsValid_LengthBiggerThan8_IsFalse()
{
    LogAnalyzer logan = GetNewAnalyzer();
    bool valid = logan.IsValid("123456789");
    Assert.IsFalse(valid);
}
[Test]
public void IsValid_LengthSmallerThan8_IsTrue()
{
    LogAnalyzer logan = GetNewAnalyzer();
    bool valid = logan.IsValid("1234567");
    Assert.IsTrue(valid);
}
private LogAnalyzer GetNewAnalyzer()
{
    LogAnalyzer analyzer = new LogAnalyzer();
    analyzer.Initialize();
    return analyzer;
}
```

##### REMOVING DUPLICATION USING [SETUP]

`Using a setup method to remove duplication`

```c#
[SetUp]
public void Setup()
{
    logan=new LogAnalyzer();
    logan.Initialize();
}
private LogAnalyzer logan= null;
[Test]
public void IsValid_LengthBiggerThan8_IsFalse()
{
    bool valid = logan.IsValid("123456789");
    Assert.IsFalse(valid);
}
[Test]
public void IsValid_LengthSmallerThan8_IsTrue()
{
    bool valid = logan.IsValid("1234567");
    Assert.IsTrue(valid);
}
```

#### Using setup methods in a maintainable manner

Setup()太好用了，好用到請你不要亂用他，另外他有以下限制：

- Setup methods can only help when you need to initialize things.
- Setup methods aren’t always the best candidates for duplication removal. Removing duplication isn’t always about creating and initializing new instances of objects. Sometimes it’s about removing duplication in assertion logic, calling out code in a specific way.
- Setup methods can’t have parameters or return values.
- Setup methods can’t be used as factory methods that return values. They’re run before the test executes, so they must be more generic in the way they work. Tests sometimes need to request specific things or call shared code with a parameter for the specific test (for example, retrieve an object and set its property to a specific value).
- Setup methods should only contain code that applies to all the tests in the current test class, or the method will be harder to read and understand.

來看看以下幾種濫用setup的情境：

- Initializing objects in the setup method that are used in only some tests in the class
- Having setup code that’s lengthy and hard to understand
- Setting up mocks and fake objects within the setup method

##### INITIALIZING OBJECTS THAT ARE USED BY ONLY SOME OF THE TESTS

**This sin is a deadly one.**

`A poorly implemented Setup() method`

```c#
[SetUp]
public void Setup()
{
    logan=new LogAnalyzer();
    logan.Initialize();
    fileInfo=new FileInfo("c:\\someFile.txt");
}
private FileInfo fileInfo = null;
private LogAnalyzer logan= null;
[Test]
public void IsValid_LengthBiggerThan8_IsFalse()
{
    bool valid = logan.IsValid("123456789");
    Assert.IsFalse(valid);
}
[Test]
public void IsValid_BadFileInfoInput_returnsFalse()
{
    bool valid = logan.IsValid(fileInfo);
    Assert.IsFalse(valid);
}
[Test]
public void IsValid_LengthSmallerThan8_IsTrue()
{
    bool valid = logan.IsValid("1234567");
    Assert.IsTrue(valid);
}
private LogAnalyzer GetNewAnalyzer()
{
    ...
}
```

Why is the setup method in the listing less maintainable? Because, to read the tests for the first time and understand why they break, you need to do the following:

1. Go through the setup method to understand what’s being initialized.
2. Assume that objects in the setup method are used in all tests.
3. Find out later you were wrong, and read the tests again more carefully to see which test uses the objects that may be causing the problems.
4. Dive deeper into the test code for no good reason, taking more time and effort to understand what the code does.

##### HAVING SETUP CODE THAT’S LENGTHY AND HARD TO UNDERSTAND

>One solution is to refactor the calls to initialize specific things into helper methods that are called from the setup method. This means that refactoring the setup method is usually a good idea.

不要成為重構魔人

>But there’s a fine line between over-refactoring and readability. Over-refactoring can lead to less-readable code. **This is a matter of personal preference**. You need to watch for when your code is becoming less readable. I recommend getting feedback from a partner during the refactoring. We all can become too enamored with code we’ve written, and having a second pair of eyes involved in refactoring can lead to good and objective results. Having a peer do a code review (a test review) after the fact is also good but not as productive as doing it as it happens.

##### SETTING UP FAKES IN THE SETUP METHOD

>Please don’t arrange fakes in a setup method. Doing so will make it hard to read and
maintain the tests.

要讓每個測試清楚看到他的mock和stub

>My preference is to have each test create its own mocks and stubs by calling helper
methods within the test, so that the reader of the test knows exactly what’s going on,
without needing to jump from test to setup to understand the full picture.

##### STOP USING SETUP METHODS

>I’ve stopped using setup methods for tests I write. They’re a relic from a time when it was OK to write crappy, unreadable tests, but that time is over.

另一個去重複的方法：parameterized

>([TestCase] in NUnit, [Theory] in XUnit.net, or  [OopsWeStillDontHaveThatFeatureAfterFiveYears] in MSTest). OK, bad joke, but MSTest still has no
simple support for this.

#### Enforcing test isolation

因為隔離沒做好，測試時好時壞，這種問題非常難查，狼來了的故事也在開發者間發生。

There are several test “smells” that can hint at broken test isolation:

- *Constrained test order*—Tests expecting to be run in a specific order or expecting information from other test results
- *Hidden test call*—Tests calling other tests
- *Shared-state corruption*—Tests sharing in-memory state without rolling back
- *External shared-state corruption*—Integration tests with shared resources and no rollback

##### ANTIPATTERN: CONSTRAINED TEST ORDER

`Constrained test order: the second test will fail if it runs first`

Myriad problems can occur when tests don’t enforce isolation:

- A test may suddenly start breaking when a new version of the test framework is introduced that runs the tests in a different order.
- Running a subset of the tests may produce different results than running all the tests or a different subset of the tests.
- Maintaining the tests is more cumbersome, because you need to worry about how other tests relate to particular tests and how each one affects state.
- Your tests may fail or pass for the wrong reasons; for example, a different test may have failed or passed before it, leaving the resources in an unknown state.
- Removing or changing some tests may affect the outcomes of others.
- It’s difficult to name your tests appropriately because they test more than a single thing.

There are a couple of common patterns that lead to poor test isolation:

- Flow testing—A developer writes tests that must run in a specific order so that they can test flow execution, a big use case composed of many actions, or a full integration test where each test is one step in that full test.
- Laziness in cleanup—A developer is lazy and doesn’t return any state their test may have changed back to its original form, and other developers write tests that depend on this shortcoming knowingly or unknowingly.

These problems can be solved in various manners:

- Flow testing—Instead of writing flow-related tests in unit tests (long-running use cases, for example), consider using some sort of integration testing framework like FIT or FitNesse or QA-related products such as AutomatedQA and WinRunner.
- Laziness in cleanup—If you’re too lazy to clean up your database after testing, your filesystem after testing, or your memory-based objects, consider moving to a different profession. This isn’t a job for you.

##### ANTIPATTERN: HIDDEN TEST CALL

This type of dependency can cause several problems:

- Running a subset of the tests may produce different results than running all the tests or a different subset of the tests.
- Maintaining the tests is more cumbersome, because you need to worry about how other tests relate to particular tests and how and when they call each other.
- Tests may fail or pass for the wrong reasons. For example, a different test may have failed, thus failing your test or not calling it at all. Or a different test may have left shared variables in an unknown state.
- Changing some tests may affect the outcome of others.
- It’s difficult to clearly name tests that call other tests.

How we got here:

- *Flow testing*—A developer writes tests that need to run in a specific order so that they can test flow execution, a big use case composed of many actions, or a full integration test where each test is one step in that full test.
- *Trying to remove duplication*—A developer tries to remove duplication in the tests by calling other tests (which have code they don’t want the current test to repeat).
- *Laziness about separating the tests*—A developer is lazy and doesn’t take the time to create a separate test and refactor the code appropriately, instead taking a shortcut and calling a different test.

Here are some solutions:

- *Flow testing*—Instead of writing flow-related tests in unit tests (long-running use cases, for example), consider using an integration testing framework like FIT or FitNesse, or QA-related products such as AutomatedQA and WinRunner.
- *Trying to remove duplication*—Don’t ever remove duplication by calling another test from a test. You’re preventing that test from relying on the setup and teardown methods in the class and are essentially running two tests in one (because the calling test has an assertion as does the test being called). Instead, refactor the code you don’t want to write twice into a third method that both your test and the other test call.
- *Laziness about separating the tests*—If you’re too lazy to separate your tests, think of all the extra work you’ll have to do if you don’t separate them. Try to imagine a world where the current test you’re writing is the only test in the system, so it can’t rely on any other test.

`Please don't do things like this`

##### ANTIPATTERN: SHARED-STATE CORRUPTION

This antipattern manifests in two major ways, independent of each other:

- Tests touch shared resources (either in memory or in external resources, such as databases, filesystems, and so on) without cleaning up or rolling back any changes they make to those resources.
- Tests don’t set up the initial state they need before they start running, relying on the state to be there.

This type of problem causes a number of symptoms:

- Running a subset of the tests may produce different results than running all the tests or a different subset of the tests.
- Maintaining the test is more cumbersome, because you may break the state for other tests, breaking those tests without realizing it.
- Your test may fail or pass for the wrong reason; a different test may have failed or passed before it, leaving the shared state in a problematic condition, or it may not have cleaned up after it ran.
- Changing some tests may affect the outcomes of other tests, seemingly randomly.

Here is how we got here:

- *Not setting up state before each test*—A developer doesn’t set up the state required for the test or assumes the state was already correct.
- *Using shared state*—A developer uses shared memory or external resources for more than one test without taking precautions.
- *Using static instances in tests*—A developer sets static state that’s used in other tests.

Here are some solutions:

- *Not setting up state before each test*—This is a mandatory practice when writing unit tests. Either use a setup method or call specific helper methods at the beginning of the test to ensure the state is what you expect it to be.
- *Using shared state*—In many cases, you don’t need to share state at all. Having separate instances of an object for each test is the safest way to go.
- *Using static instances in tests*—You need to be careful how your tests manage static state. Be sure to clean up the static state using setup or teardown methods. Sometimes it’s effective to use direct helper method calls to clearly reset the static state from within the test. If you’re testing singletons, it’s worth adding public or internal setters so your tests can reset them to a clean object instance.

##### ANTIPATTERN: EXTERNAL SHARED-STATE CORRUPTION

This antipattern is similar to the in-memory, shared-state corruption pattern, but it
happens in integration-style testing:

- Tests touch shared resources (either in memory or in external resources, such as databases and filesystems) without cleaning up or rolling back any changes they make to those resources.
- Tests don’t set up the initial state they need before they start running, relying on the state to be there.

#### Avoiding multiple asserts on different concerns

>If only one assert fails, you never know if the other asserts in that same test method would have failed or not. You may *think* you know, but it’s an assumption until you can prove it with a failing or passing assert.

這會導致你判斷錯誤，有可能朝錯誤的方向修正。

There are several ways to achieve the same goal:

- Create a separate test for each assert.
- Use parameterized tests.
- Wrap the assert call with try-catch.

##### USING PARAMETERIZED TESTS

```c#
[TestCase]
```

##### WRAPPING WITH TRY-CATCH

don't do thing like this

#### Comparing objects

這是一種合理使用多個assert在同個test裡面的case，不過有些更好的做法。

##### MAKING TESTS MORE MAINTAINABLE

建一個資料class來做完整的比較，注意通常需要override `Equals()`來達成這件事，或許你會覺得很煩，不過有些工具可以幫你自動產生這些方法，例如`ReSharper`。

##### OVERRIDING TOSTRING()

因為Nunit的output是呼叫兩個比較object的ToString()，直接override `ToString()`在裡面印出所有property。

`這招幹暴潮耶`

#### Avoiding overspecification

>An overspecified test is one that contains assumptions about how a specific unit under test (production code) should implement its internal behavior, instead of only checking that the end behavior is correct.

`就檢查太多東西`

Here are ways unit tests are often overspecified:

- A test asserts purely internal state in an object under test.
- A test uses multiple mocks.
- A test uses stubs also as mocks.
- A test assumes specific order or exact string matches when it isn’t required.

##### SPECIFYING PURELY INTERNAL BEHAVIOR

內部的狀態實作經常可能被更動，但那並不影響最後結果。

##### USING STUBS ALSO AS MOCKS

常見的錯誤是去檢查某個method是否被呼叫過，但他有沒有被呼叫根本不影響最後的結果。

##### ASSUMING AN ORDER OR EXACT MATCH WHEN IT’S NOT NEEDED

經常問自己，是否真的要檢查整個object內容？是否只檢查幾個properties就好?或是否可以使用`string.Contains()`而不是`string.Equals()`？

### Writing readable tests

>Without readability the tests you write are almost meaningless. Readability is the connecting thread between the person who wrote the test and the poor soul who has to read it a few months later. Tests are stories you tell the next generation of programmers on a project. They allow a developer to see exactly what an application is made of and where it started.

There are several facets to readability:

- Naming unit tests
- Naming variables
- Creating good assert messages
- Separating asserts from actions

#### Naming unit tests

Naming standards are important because they give you comfortable rules and templates
that outline what you should explain about the test. The test name has three parts:

- *The name of the method being tested*—This is essential, so that you can easily see where the tested logic is. Having this as the first part of the test name allows easy navigation and as-you-type intellisense (if your IDE supports it) in the test class.
- *The scenario under which it’s being tested*—This part gives you the “with” part of the name: “When I call method X *with a `null` value*, then it should do Y.”
- *The expected behavior when the scenario is invoked*—This part specifies in plain English what the method should do or return, or how it should behave, based on the current scenario: “When I call method X with a `null` value, *then it should do Y*.”

>A common way to write these three parts of the test name is to separate them with underscores, like this: `MethodUnderTest_Scenario_Behavior()`.

#### Naming variables

有時候這個比production code的naming更重要，你必須用最快的速度讓讀測試的人知道你想證明甚麼。來看一個例子：

`An unreadable test name`

```c#
[Test]
public void BadlyNamedTest()
{
    LogAnalyzer log = new LogAnalyzer();
    int result= log.GetLineCount("abc.txt");
    Assert.AreEqual(-100,result);
}
```

`-100`到底是幹嘛用的，這時候你只能腦補，或許他是例外?，你有以下兩種較好的處理方式：

- You can change the design of the API to throw an exception instead of returning -100 (assuming -100 is some sort of illegal result value).
- You can compare the result to some sort of constant or aptly named variable, as shown in the following listing.

#### Asserting yourself with meaning

>Avoid writing your own custom assert messages. Please. This section is for those who find they absolutely have to write a custom assert message, because the test really needs it, and you can’t find a way to make the test clearer without it.

寫custom assert message就像寫custom exception message一樣困難。

There are several key points to remember when writing a message for an assert clause:

- Don’t repeat what the built-in test framework outputs to the console.
- Don’t repeat what the test name explains.
- If you don’t have anything good to say, don’t say anything.
- Write what should have happened or what failed to happen, and possibly mention when it should have happened.

#### Separating asserts from actions

好

`Separating the assert from the thing asserted, improving readability`

```c#
[Test]
public void BadAssertMessage()
{
    //some code here
    int result= log.GetLineCount("abc.txt");
    Assert.AreEqual(COULD_NOT_READ_FILE,result);
}
```

不好

`Not separating the assert from the thing asserted, making reading difficult`

```c#
[Test]
public void BadAssertMessage()
{
    //some code here
    Assert.AreEqual(COULD_NOT_READ_FILE,log.GetLineCount("abc.txt"));
}
```

#### Setting up and tearing down

使用setup或teardown可能會讓測試變得難理解，舉例在setup裡面使用mocks或stubs有可能讓別人不知道這東西的存在。最好是寫一個helper method，在每個使用到的測試裡面呼叫他，這樣讀測試的人不需要看多個地方。

>**TIP** I’ve several times written full test classes that didn’t have a setup method, only helper methods being called from each test, for the sake of maintainability. The classes were still readable and maintainable.

### Summary

阿就三本柱講一遍

## Chapter 9: Integrating unit testing into the organization

