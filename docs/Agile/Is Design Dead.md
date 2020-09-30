# Is Design Dead

[OG](https://martinfowler.com/articles/designDead.html)

## Planned and Evolutionary Design

- 這世上有兩種設計：Planned and Evolutionary Design
- Designers隨著時間不再開發，不但跟不上技術潮流，也慢慢會失去開發者對你的尊敬
- 蓋房子也會有建築師跟建築工衝突的問題，但比起來軟體業更為明顯，因為兩者的知識分界更小，尤其當designers缺乏對programmers的日常開發知識的時候
- Planned design沒有不好，只是你幾乎不可能預測你的需求變動，尤其是商業面的原因

## The Enabling Practices of XP

- XP提倡Evolutionary Design
- Evolutionary Design一般認為不可行的原因：software change curve
- 想玩XP首先你要先enable它，enable的核心主要有兩個：Testing和CI，也是靠這兩招把software change curve壓平
- XP不僅僅是測試，然後重構而已，設計就發生在每次迭代之間，只是空間跟平衡還在拿捏

## The Value of Simplicity

- Two of the greatest rallying cries in XP are the slogans "Do the Simplest Thing that Could Possibly Work" and "You Aren't Going to Need It" (known as YAGNI).
- 就算我預期下個迭代就需要用到的功能我也不會先做，因為那些不是這次迭代的工作
- 就算cost是0我也不做，因為須要維護

## What on Earth is Simplicity Anyway

- XPE裡所說的四個簡單準則：
  - Runs all the Tests
  - No duplication
  - Reveals all the intention
  - Fewest number of classes or methods
- 簡單事一件很複雜的事情
- "it's easier to refactor over-design than it is to refactor no design."，可以變簡單很好，但是稍微複雜並不是災難
- 最終，重構的意願還是比是否簡單來的重要

## Does Refactoring Violate YAGNI?

- YAGNI是簡單設計的一種實踐方式，重構是讓系統維持簡單的方法

## Patterns and XP

- 有些人認為XP與設計模式衝突，這是因為設計模式遭到濫用
- 有效的設計認為設計模式是值得付出的
- XP是開發流程，而設計模式是設計知識骨幹
- XP強調的是不要使用設計模式直到你需要他
- 對XPers使用設計模式的建議：
  - Invest time in learning about patterns
  - Concentrate on when to apply the pattern (not too early)
  - Concentrate on how to implement the pattern in its simplest form first, then add complexity later.
  - If you put a pattern in, and later realize that it isn't pulling its weight - don't be afraid to take it out again.

## Growing an Architecture

- 軟體架構就是指系統的核心元素，是難以改變的部分，是剩下東西的基礎
- 有一些早期的架構設計還是有點作用，例如你怎麼分層或如何與DB溝通等等
- 重點是不要認為這些架構設計是無法動搖的，隨時要有修改他的準備
- 如果你決定不使用某個東西，思考之後再把他加入會有多困難
- 如果發現你的架構某些部分沒增加任何東西，隨時準備把它簡化

## UML and XP

- 如果你覺得好用就用，不應該是你覺得好用而強迫別人使用，反之亦然
- 當你覺得需要的時候再使用，因為這很花時間
- 記著圖表是拿來溝通用的，有效的溝通就是去掉雜訊，所以不需要每個class都畫，class也不用每個attribute都畫，如果需要完整的資訊那已經在code裡了
- 當你使用圖表來做開始coding前的設計時，謹記：
  - keep them short
  - don't try to address all the details (just the important ones)
  - treat the resulting design as a sketch, not as a final design
- UML用在進行中文檔(on-going documentation)通常沒啥效果，因為要維持它的更新很辛苦，而且他藏在CASE tool裡面沒人會看
- 如果使用在進行中文檔，建議：
  - Only use diagrams that you can keep up to date without noticeable pain
  - Put the diagrams where everyone can easily see them. I like to post them on a wall. Encourage people to edit the wall copy with a pen for simple changes.
  - Pay attention to whether people are using them, if not throw them away.
- UML的另一個使用情境是交接，一樣記得圖表只是幫助溝通凸顯重要的部分，詳細的內容是在你的程式碼庫

## On Metaphor

完全看不懂

[System Metaphor](http://wiki.c2.com/?SystemMetaphor)

>Definition: What ExtremeProgramming (XP) uses to unify an architecture and provide naming conventions. A simple shared story of how the system works, a metaphor. This story typically involves a handful of classes and patterns that shape the core flow of the system being built.

## Do you wanna be an Architect when you grow up?

- 問題在於，當你想練習成為技術領導的時候，你是否該丟棄那些開發日常?
- 一些反對XP的人認為他們最不想要開發人員重構和亂搞設計，可是，如果你不信任你的開發人員，那你幹嘛雇用他們?
- XP的理念是經驗豐富的開發人員把技能傳授給較沒經驗的開發人員，協助他們做決定，而不是靠架構師一個人決定所有重要的事

## Reversibility

- 如果你可以輕易改變你的決定，那你的決定是否正確就不是那麼重要
- evolutionary design就是在做決定時要避免不可逆，不論是延後做決定或是直接做修改不會太困難的決定
- 這就是為什麼敏捷強調你一定要有版控
- 可逆式設計也可以讓問題快速浮現，你可以開一條branch做些實驗然後丟棄

## The Will to Design

- evolutionary design需要有一股收斂他的力量，不用是每個人(如果是那也很好)，通常一或二個團隊成員負責整個設計的完整性，這也是一般在說的"架構師"的責任之一
- 持續關注code base，看哪裡變糟了並在他失去控制前修正他，修正他的不一定要是保持設計的人，但要確保有人去修

## Things that are difficult to refactor in

- XP認為所有東西都應該要能簡單新增，就算是普遍性的功能，例如國際化
- 當你決定在一開始加入國際化，一週又一週地慢慢地新增與維護，你會無意識忽略了這個之後才用的到的東西的成本，而且還有之後需要重構的可能
- YAGNI的部分理由是這些潛在需求其實你最後都不需要，或者不是你期望的樣子，而重構比做出所有可能功能花地工少
- 如果你曾經做過國際化，那你就能根據成本來評斷現在做還是之後再做，而如果你沒做過，你不但沒辦法判斷成本也不知道如何做好他，那你應該之後再做，那個時候你的團隊已經更有經驗，更了解這個領域，更了解需求
- Kent認為stories裡面商業價值是唯一要素，馬丁認為應該也要有一點技術風險

## Is Design Happening?

- evolutionary design最困難的一點是很難判斷是否真的有設計，邊設計邊開發很容易變成只有開發沒有設計
- 當你發現code base變得複雜難運作的時候，就代表缺乏設計，但遺憾的是這是很主觀的，我們沒有可靠的指標來判斷這件事
- 對技術人員來說已經不容易了，對非技術人員更難評斷一個軟體是否設計良好
- 兩點建議：
  - Listen to the technical people. If they are complaining about the difficulty of making changes, then take such complaints seriously and give them time to fix things.
  - Keep an eye on how much code is being thrown away. A project that does healthy refactoring will be steadily deleting bad code. If nothing's getting deleted then it's almost certainly a sign that there isn't enough refactoring going on - which will lead to design degradation. However like any metric this can be abused, the opinion of good technical people trumps any metric, despite its subjectivity.

## So is Design Dead?

Not by any means, but the nature of design has changed. XP design looks for the following skills

- A constant desire to keep code as clear and simple as possible
- Refactoring skills so you can confidently make improvements whenever you see the need.
- A good knowledge of patterns: not just the solutions but also appreciating when to use them and how to evolve into them.
- Designing with an eye to future changes, knowing that decisions taken now will have to be changed in the future.
- Knowing how to communicate the design to the people who need to understand it, using code, diagrams and above all: conversation.
