# System Design

- 有relation需求或需要transaction就RDBMS，沒有就noSQL
- 資料太大就partition，注意不要造成unbalanced，記得用consistent hash
- 速度不夠快就cache，一千零一招LRU
- Load Balancer: 就RR，高級一點就把loading考慮進來

## TODO

排行榜

## Three-tier architecture

[Three-tier architecture](https://en.wikipedia.org/wiki/Multitier_architecture)

## Strategy

### Caching

- CDN
- Redis

### Load Balancing

- Nginx
  - compression
  - https
  - caching
  - rate limit

## Architecture

[架构师之路](https://mp.weixin.qq.com/s?__biz=MjM5ODYxMDA5OQ==&mid=2651959886&idx=1&sn=03e45a5014053607eff5e55ed2c660d7&chksm=bd2d07928a5a8e8454d395e176fa9d346682abfe9dfbf3244f1dead83ee4508aa25121f9b811&scene=25#wechat_redirect)

[PegasusWang 的读书杂记](https://pegasuswang.readthedocs.io/zh/latest/)

[Aspect-oriented software development (AOP)](https://en.wikipedia.org/wiki/Aspect-oriented_software_development)

[來談談 AOP (Aspect-Oriented Programming) 的精神與各種主流實現模式的差異](https://medium.com/cymetrics/aop-caf6a403e07f)

[Cross-cutting concern](https://en.wikipedia.org/wiki/Cross-cutting_concern)

[Service-oriented architecture (SOA)](https://en.wikipedia.org/wiki/Service-oriented_architecture)

[Micro Frontends](https://martinfowler.com/articles/micro-frontends.html)

## Resources

[Grokking the System Design Interview](https://www.educative.io/courses/grokking-the-system-design-interview)

[SystemsExpert](https://www.algoexpert.io/systems/product)

[system-design-interview](https://github.com/checkcheckzz/system-design-interview)

[SYSTEM DESIGN PREPARATION](https://github.com/shashank88/system_design)

[在 17 Media 擔任 SRE 的所見及所聞](https://medium.com/17media-tech/what-i-see-and-hear-as-an-sre-at-17-media-315c97bca8e)

[The Path to Becoming a Software Architect](https://medium.com/@nvashanin/the-path-to-becoming-a-software-architect-de53f1cb310a)

[The Architecture of Open Source Applications](http://www.aosabook.org/en/index.html)

[Architectural pattern](https://en.wikipedia.org/wiki/Architectural_pattern)

[读时模式(schema on read)与写时模式(schema on write)](http://www.zdingke.com/2019/08/27/schema-on-read%E4%B8%8Eschema-on-write/)

### Books

- Software Architecture in Practice (3rd Edition) (SEI Series in Software Engineering)
- Essential Software Architecture
