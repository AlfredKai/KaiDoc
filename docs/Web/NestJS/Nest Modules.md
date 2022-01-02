# Nest Modules

Nest的各個元件就是以module為單位，使用module就可以享受到Nest的IoC container功能（[why we need IoC container](https://stackoverflow.com/questions/871405/why-do-i-need-an-ioc-container-as-opposed-to-straightforward-di-code)），module除了處理route的controller以外，provider才是真正提供功能的地方，使用import來引入其他的module並使用他們的provider，使用export將此module要提供的provider開放出來，export就好比這個module的pulbic interface。
