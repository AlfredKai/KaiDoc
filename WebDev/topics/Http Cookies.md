# HTTP cookies

[HTTP cookies](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/Cookies)

[使用Nodejs設定cookie](https://nodejs.org/dist/latest-v8.x/docs/api/http.html#http_response_setheader_name_value)

## Headers

```http
Set-Cookie: <cookie-name>=<cookie-value>
```

`Expires`:過期時間

`Max-Age`:可維持最大時間

`Secure`:只可經由加密請求HTTPS傳送

`HttpOnly`:不可透過JS的`document.cookie`取得，避免XSS攻擊

`SameSite`:不可以跨站請求方式寄送，避免CSRF攻擊

## 第三方 cookies

Cookies 會帶有他們所屬的網域名。若此網域和你所在的頁面網域相同，cookies 即為第一方（first-party）cookie，不同則為第三方（third-party）cookie。第一方 cookies 只被送到設定他們的伺服器，但一個網頁可能含有存在其他網域伺服器的圖片或組件（像橫幅廣告）。透過這些第三方組件傳送的 cookies 便是第三方 cookies，經常被用於廣告和網頁上的追蹤。

>Site domain does not match cookie domain

## SameSite=None

[Reject insecure SameSite=None cookies](https://www.chromestatus.com/feature/5633521622188032)

## 安全

[Cross-site scripting (XSS)](https://developer.mozilla.org/en-US/docs/Glossary/Cross-site_scripting)

[CSRF (Cross-Site Request Forgery)](https://developer.mozilla.org/en-US/docs/Glossary/CSRF)

[Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/CORS)

>`XSS` exploits the trust a user has for a particular site, `CSRF` exploits the trust that a site has in a user's browser.

`XSS`是代碼注入，`CSRF`是拿到權限執行命令
