# Agile

- 組織系統的不同想法會不同
- 自組織：讓你的團隊自己去學習
- 每一個feature team裡面至少有一半以上是senior

## The State of Agile Software in 2018

大師開講
<https://martinfowler.com/articles/agile-aus-2018.html>

現在敏捷看似一片光明，因為他是主流，但實際上卻有許多問題，其中三個主要問題：

1. ighting the Agile Industrial Complex and its habit of imposing process upon teams
2. raising the importance of technical excellence
3. organizing our teams around products (rather than projects)

    As we look at the state of agile: at a surface level, things are looking very good.

十年前有人請馬丁去演講，直接說明你可以談任何主題，但不要跟敏捷有關。  
而現在敏捷到處都是。  
所以以前談論敏捷會直接得到很大的反彈，但這樣就能帶來一些重要的溝通，但現在你會聽到他們直接說"噢~我們已經在跑敏捷了~"，然後你到了那裏，發現跟你想的完全不一樣。

### The first problem: Faux-agile

    Our challenge now is dealing with faux-agile

所以我們的挑戰是對付*人造敏捷*，`Dark Agile` or `Dark Scrum`

>Our challenge at the moment isn't making agile a thing that people want to do, it's dealing with what I call faux-agile: agile that's just the name, but none of the practices and values in place.

人造敏捷比假裝敏捷還糟糕(假裝至少只是假裝，人造敏捷是你真的做了可是亂來)

問題太大，作者區分三個主要問題來討論

    Our first problem is dealing with the Agile Industrial Complex

讓做事的人選擇如何做事是敏捷最基本的假設

>In particular, one of the really central things that impressed me very much about the early agile advocates was this realization that people are operating at the best when they choose how they want to work.

談到敏捷宣言時，雖然他們並沒有順序關係，但特別談到第一條"個人與互動重於流程與工具"，你如果想要成功的軟體開發，你要找到好的人。

    A team should not only choose its process, but continue to evolve it

敏捷過程就是你會一直進化他`

>"if you're doing Extreme Programming the same way as you were doing it a year ago, you're no longer doing Extreme Programming".

20世紀初，*Frederick Taylor*影響了整個工業，他喜歡研究如何讓人更有效率，他認為員工平均而言是懶惰、腐敗且愚笨的，所以不讓做事的的人決定如何做事而是有另一群更聰明受更良好教育的人來決定如何做事。

但是拜託，我們討論的是*軟體開發*

>The agile movement was part of trying to push that, to try to say, "The teams involved in doing the work should decide how it gets done," because let's face it, we're talking about software developers here. People who are well paid, well educated, hopefully, well-motivated people, and so they should figure out what in their particular case is necessary.

"Particular case"是非常重要的，我們面對不同的技術不同的環境不同的人，不可能有任何一種方法適用任何人

    The Agile Industrial Complex imposing methods on people is an absolute travesty

敏捷提倡者也部會說敏捷適用於任何地方，重點是`The team doing work decides how to do it. That is a fundamental agile principle.`  
甚至是如果團隊不想跑敏捷，不跑敏捷對這個團隊而言就是最敏捷的

### The second problem: technical excellence

    The second problem is the lack of recognition of the importance of technical excellence.

許多敏捷會議不怎麼談論技術，這是很悲慘的，因為技術人員是真正做事的人，所以我們應該更關注這些技能：如何培育他、如何發展他且如何讓他更重要

`你們有多少人懂Refactor?`他就是敏捷適應快速變化的核心技能

敏捷有兩個大重點，一個是團隊優先，團隊決定如何做事，另一個就是團隊快速改變的能力

>When I summarize agile to people, I usually say there's two main pieces to it. One, I've already talked about, the primacy of the team, and the team's choices of how they do things, but the other is our ability to change rapidly, to be able to deal with change easily.

哥是定義`refactoring`的人我應該清楚他的意思

    Refactoring is lots of small changes, none of which change the observable part of the software

重構是許多小的改變，這些改變你從外面是看不出來的，通常為的是新增功能，而舊的架構不是很好新增，所以你重構使的新功能更容易新增上去：

>Kent Beck summarizes this. "When you want to make a change, first, make the change easy. (Warning, this may be hard.) Then make the easy change".

`refactoring is a constant thing`

想要快速做變化的一個重點是快速了解整個程式在幹嘛，不過如果你程式模組化做得好，你就不用了解整個程式在幹嘛。

為什麼重構很重要？因為重構的過程中你致力於讓程式**更好閱讀**，不用每次都需要把過程重新了解一遍，你重構的過程同時讓新的功能**更容易新增**，所以你可以更快速的做變化，而一但你能快速新增東西，你就不需要在一開始的時候讓你的程式能做任何事，可以等需要再加入。這就是**Yagni** - "You aren't gonna need it"原則(你用不到啦原則)，不要在你還不需要的時候加入功能，你只會讓程式更難懂。

不過這個方法如果沒有良好的重構技能，是完全沒用的。

而重構又仰賴著測試、仰賴著持續整合，同時你還需要持續發布你才能用非常非常快的頻率發布你的軟體。

### The third problem: product-oriented view

用產品看世界

    We should get rid of software projects and use a product-oriented view

中間一大段完全看不懂

溝通是一切

>There should be an ongoing conversation between software developers, business people, and anybody involved in making that customer experience better.

### Summary

- **Get rid of the Agile Industrial Complex and the idea of imposing stuff on teams**. Let teams work out the way they should work themselves.
- **Raise the importance of technical excellence**, and never forget that when writing software, the technology side is really vital, and
- **organize around products**.

雖然講了一堆壞事，但是講者並不悲觀，回到敏捷宣言發布後的六個月，有人提議我們應該成立一個組織，詢問了17位敏捷宣言作者是否該扮演特殊腳色：

    The manifesto authors said "no" to a special role in the movement's future

"new people will come in and do great things"

    because we let go, we ended up with a community that could tackle problems we hadn't imagined

開放歡迎後來者正是敏捷的特性之一，不需要悲觀只需要持續改變我們就擁有未來
