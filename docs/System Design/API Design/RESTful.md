# RESTful

[Representational state transfer](https://en.wikipedia.org/wiki/Representational_state_transfer)

[Representational State Transfer (REST)](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)

## Architectural Constraints

### Client–server architecture

### Statelessness

### Cacheability

Responses must, implicitly or explicitly, define themselves as either cacheable or non-cacheable to prevent clients from providing stale or inappropriate data in response to further requests.

Since REST uses `URL` as a `resource identifier`, it's easy to leverage browser cache.
Only `GET` will be cache by browser. In other API paradigm like GraphQL which only uses `POST`, you have to something to use browser cache. ref: [GraphQL & Caching: The Elephant in the Room](https://www.apollographql.com/blog/backend/caching/graphql-caching-the-elephant-in-the-room/)

### Layered system

### Uniform interface



### Code on demand (optional)

## Other

### Versioning

[Best practices for API versioning?](https://stackoverflow.com/questions/389169/best-practices-for-api-versioning)

[Your API Versioning is Wrong](https://dzone.com/articles/your-api-versioning-wrong)

### elements of the resource that are updated by the server

[should I use PUT method for update, if I also update a timestamp attribute](https://stackoverflow.com/questions/5686671/should-i-use-put-method-for-update-if-i-also-update-a-timestamp-attribute)

### Idempotent and Safety

### Nesting

### deleting a bunch of items

[Restful way for deleting a bunch of items](https://stackoverflow.com/questions/2421595/restful-way-for-deleting-a-bunch-of-items/41539479#41539479)

[閒聊 - Web API 是否一定要 RESTful?](https://blog.darkthread.net/blog/is-restful-required/)

### Put vs Patch and Partial Update

### Partial Response

[RESTful API Design: can your API give developers just the information they need?](https://cloud.google.com/blog/products/api-management/restful-api-design-can-your-api-give-developers-just-information-they-need)

## RESTful Resources

[真。淺談RESTful API by TritonHo](https://github.com/TritonHo/slides/blob/master/Taipei%202019-06%20talk/RESTful%20API%20Design-tw-2.2.pdf)

[RESTful Web API 設計指南](https://www.footmark.info/programming-language/design/restful-webapi-design-guide/?fbclid=IwAR3vYxURSQFI57kFt9v2kRU87nnMUUTYlF0WXfl1yyP2W_O6iX-hc7x2h5E)

[REST API: Sorting, Filtering, and Pagination](https://www.taniarascia.com/rest-api-sorting-filtering-pagination/)

[深入浅出 REST](https://www.infoq.cn/article/rest-introduction/)

[什麼是REST跟RESTful?](https://ihower.tw/blog/archives/1542)

[Best Practices for Designing a Pragmatic RESTful API](https://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api)

[HATEOAS](https://restfulapi.net/hateoas/)

[REST: Good Practices for API Design](https://medium.com/hashmapinc/rest-good-practices-for-api-design-881439796dc9)

[Web API design](https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design)

[REST vs. RESTful: The Difference and Why the Difference Doesn’t Matter](https://blog.ndepend.com/rest-vs-restful/)