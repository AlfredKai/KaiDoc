# Testing

## Art of Unit Test

- 三種驗證方式
  - 驗證回傳
  - 驗證狀態
  - 驗證交互作用
- 解耦合用隔離元件
  - Stub(代替有回傳值的相依物件)
  - Mock(代替驗證狀態與驗證交互作用的物件)
  - Fake(Stub與Mock的總稱)
- Dependency Injection(不喜歡OO的同學可以不用加)
  - Constructor
  - Property + Default
  - Virtual and Override
  - Static Factory
- 測試三本柱
  - Trustworthy
    - 避免測試中有邏輯
  - Maintainable
    - 測試隔離
    - 避免多個關注點
    - 避免overspecification
    - DRY
  - Readable
    - Naming
    - AAA原則

## Resources

[Practical Object-Oriented Design Book 心得＆整理](https://zxuanhong.medium.com/practical-object-oriented-design-book-%E5%BF%83%E5%BE%97-%E6%95%B4%E7%90%86-9-final-11c461ca2b0a)
