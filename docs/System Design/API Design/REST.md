# REST

[Representational state transfer](https://en.wikipedia.org/wiki/Representational_state_transfer)

[Representational State Transfer (REST)](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)

[理解本真的 REST 架构风格](https://www.infoq.cn/article/understanding-restful-style/)

[What Is Hypermedia?](https://smartbear.com/learn/api-design/what-is-hypermedia/)

## REST vs RESTful

REST is a software architectural style, not API paradigm. A web API that obeys the REST constraints is informally described as RESTful. RESTful web APIs are typically loosely based on HTTP methods to access resources via URL-encoded parameters and the use of JSON or XML to transmit data.

## HTTP vs REST

>在互联网行业，实践总是走在理论的前面。Web 发展到了 1995 年，在 CGI、ASP 等技术出现之后，沿用了多年、主要面向静态文档的 HTTP/1.0 协议已经无法满足 Web 应用的开发需求，因此需要设计新版本的 HTTP 协议。在 HTTP/1.0 协议专家组之中，有一位年轻人脱颖而出，显示出了不凡的洞察力，后来他成为了 HTTP/1.1 协议专家组的负责人。这位年轻人就是 Apache HTTP 服务器的核心开发者 Roy Fielding，他还是 Apache 软件基金会的合作创始人。

>HTTP/1.1 协议设计的极为成功，以至于发布之后整整 10 年时间里，都没有多少人认为有修订的必要。用来指导 HTTP/1.1 协议设计的这套理论框架，最初是以备忘录的形式在专家组成员之间交流，除了 IETF/W3C 的专家圈子，并没有在外界广泛流传。Fielding 在完成 HTTP/1.1 协议的设计工作之后，回到了加州大学欧文分校继续攻读自己的博士学位。第二年（2000 年）在他的博士学位论文 Architectural Styles and the Design of Network-based Software Architectures 中，Fielding 更为系统、严谨地阐述了这套理论框架，并且使用这套理论框架推导出了一种新的架构风格，并且为这种架构风格取了一个令人轻松愉快的名字“REST”——Representational State Transfer（表述性状态转移）的缩写。

**REST** is the way **HTTP** should be used.

## Architectural Constraints

>在论文的前三章中，Fielding 在批判性继承前人研究成果的基础上，建立起来一整套研究和评价软件架构的方法论。这套方法论的核心是“架构风格”这个概念。架构风格是一种研究和评价软件架构设计的方法，它是比架构更加抽象的概念。一种架构风格是由一组相互协作的架构约束来定义的。架构约束是指软件的运行环境施加在架构设计之上的约束。
 --理解本真的 REST 架构风格

### Client–server architecture

Separation of concerns is the principle behind the client-server constraints. By separating the user interface concerns from the data storage concerns, we improve the portability of the user interface across multiple platforms and improve scalability by simplifying the server components.

### Statelessness

Each request from client to server must contain all of the information necessary to understand the request, and cannot take advantage of any stored context on the server. **Session state is therefore kept entirely on the client**.

- Visibility is improved because a monitoring system does not have to look beyond a single request datum in order to determine the full nature of the request.
- Reliability is improved because it eases the task of recovering from partial failures.
- Scalability is improved because not having to store state between requests allows the server component to quickly free resources.

### Cacheability

**Responses** must, implicitly or explicitly, define themselves as either cacheable or non-cacheable to prevent clients from providing stale or inappropriate data in response to further requests.

Since REST uses `URL` as a `resource identifier`, it's easy to leverage browser cache.
Only `GET` will be cache by browser. In other API paradigm like GraphQL which only uses `POST`, you have to something to use browser cache. ref: [GraphQL & Caching: The Elephant in the Room](https://www.apollographql.com/blog/backend/caching/graphql-caching-the-elephant-in-the-room/)

### Layered system

The layered system style allows an architecture to be composed of hierarchical layers by constraining component behavior such that each component cannot "see" beyond the immediate layer with which they are interacting. By restricting knowledge of the system to a single layer, we place a bound on the overall system complexity and promote substrate independence. Layers can be used to encapsulate legacy services and to protect new services from legacy clients, simplifying components by moving infrequently used functionality to a shared intermediary. Intermediaries can also be used to improve system scalability by enabling load balancing of services across multiple networks and processors.

The primary disadvantage of layered systems is that they add overhead and latency to the processing of data, reducing user-perceived performance. For a network-based system that supports cache constraints, this can be offset by the benefits of shared caching at intermediaries.

The combination of layered system and uniform interface constraints induces architectural properties similar to those of the uniform pipe-and-filter style. Within REST, intermediary components can actively transform the content of messages because the messages are self-descriptive and their semantics are visible to intermediaries.

### Uniform interface

By applying the software engineering principle of generality to the component interface, the overall system architecture is simplified and the visibility of interactions is improved. Implementations are decoupled from the services they provide, which encourages independent evolvability. The trade-off, though, is that a uniform interface degrades efficiency, since information is transferred in a standardized form rather than one which is specific to an application's needs.

**The REST interface is designed to be efficient for large-grain hypermedia data transfer, optimizing for the common case of the Web, but resulting in an interface that is not optimal for other forms of architectural interaction.**

- identification of resources
- manipulation of resources through representations
- self-descriptive messages
- hypermedia as the engine of application state

#### identification of resources

#### manipulation of resources through representations

#### self-descriptive messages

#### hypermedia as the engine of application state

[HATEOAS](https://en.wikipedia.org/wiki/HATEOAS)

[Richardson Maturity Model](https://en.wikipedia.org/wiki/Richardson_Maturity_Model)

[你的REST不是REST？](https://www.ithome.com.tw/voice/128528)

[Why I Hate HATEOAS](https://jeffknupp.com/blog/2014/06/03/why-i-hate-hateoas/)

[REST APIs must be hypertext-driven](https://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven)

### Code on demand (optional)

REST allows client functionality to be extended by downloading and executing code in the form of applets or scripts. This simplifies clients by reducing the number of features required to be pre-implemented. Allowing features to be downloaded after deployment improves system extensibility. However, it also reduces visibility, and thus is only an optional constraint within REST.

## Difficulties

### Versioning

[Best practices for API versioning?](https://stackoverflow.com/questions/389169/best-practices-for-api-versioning)

[Your API Versioning is Wrong](https://dzone.com/articles/your-api-versioning-wrong)

### Bulk/Partial Operations

[Restful way for deleting a bunch of items](https://stackoverflow.com/questions/2421595/restful-way-for-deleting-a-bunch-of-items/41539479#41539479)

[閒聊 - Web API 是否一定要 RESTful?](https://blog.darkthread.net/blog/is-restful-required/)

[RESTful API Design: can your API give developers just the information they need?](https://cloud.google.com/blog/products/api-management/restful-api-design-can-your-api-give-developers-just-information-they-need)

[Parameter Serialization](https://swagger.io/docs/specification/serialization/?sbsearch=nested%20parameters)

### ??

- 统一接口 + 超文本驱动，带来了最大限度的松耦合。允许服务器端和客户端程序在很大范围内，相对独立地进化。对于设计面向企业内网的 API 来说，松耦合并不是一个很重要的设计关注点。
- http status code/content-type/method are all customizable (6.3.1.2 Extensible Protocol Elements)
- Besides HTTP(s), Websocket is another protocol supported by the browser.
- why we need uniform interface
  - https://stackoverflow.com/questions/25172600/rest-what-exactly-is-meant-by-uniform-interface
  - Intermediaries can also be used to improve system scalability by enabling load balancing of services across multiple networks and processors.
  - intermediaries no need to be updated before a new method can be deployed
  - know how to cache

## Misc

[should I use PUT method for update, if I also update a timestamp attribute](https://stackoverflow.com/questions/5686671/should-i-use-put-method-for-update-if-i-also-update-a-timestamp-attribute)

## RESTful Resources

[真。淺談RESTful API by TritonHo](https://github.com/TritonHo/slides/blob/master/Taipei%202019-06%20talk/RESTful%20API%20Design-tw-2.2.pdf)

[REST API: Sorting, Filtering, and Pagination](https://www.taniarascia.com/rest-api-sorting-filtering-pagination/)

[深入浅出 REST](https://www.infoq.cn/article/rest-introduction/)

[Principles & Best practices of REST API Design](https://blog.devgenius.io/best-practice-and-cheat-sheet-for-rest-api-design-6a6e12dfa89f)
