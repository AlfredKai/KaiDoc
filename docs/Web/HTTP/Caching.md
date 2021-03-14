# Caching

[HTTP caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)

[Prevent unnecessary network requests with the HTTP Cache](https://web.dev/http-cache/)

[HTTP Caching](https://developers.google.com/web/fundamentals/performance/get-started/httpcaching-6)

## Cacheable

[Cacheable](https://developer.mozilla.org/en-US/docs/Glossary/cacheable)

## Cache Control

- no-store / no-cache
- public / private

[Cache-Control](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control)

## Revved resources (Cache Busting)

[What Is Cache Busting?](https://www.keycdn.com/support/what-is-cache-busting)

### Query String

Query strings on the other hand, have been known to cause caching issues. Some proxies or CDNs aren't even able to cache files that contain query strings and it is recommended not to use them.

## Validation

[HTTP conditional requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/Conditional_requests)

[304 Not Modified](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/304)

[Last-Modified](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Last-Modified)

[ETag](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag)

- Last-Modified with If-Modified-Since
- weak ETag with If-None-Match
- ETag with If-Match
- If-None-Match > If-Modified-Since

## Misc

- default "好像是" **Private**
- 第一個request會被加上max-age=0看來是無法使用private cache
- public, max-age=0與no-cache感覺沒甚麼不同
- 如何產生e-tag沒有明確的規範
