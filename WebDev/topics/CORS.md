# Cross-Origin Resource Sharing (CORS)

[Cross-Origin Read Blocking for Web Developers](https://www.chromium.org/Home/chromium-security/corb-for-developers)
[Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

Cross-Origin Resource Sharing (CORS) is a mechanism that uses additional HTTP headers to tell browsers to give a web application running at one origin, access to selected resources from a different origin. A web application executes a **cross-origin HTTP request** when it requests a resource that has a different origin (domain, protocol, or port) from its own.

## Same Origin Policy

[Same-origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)

### Definition of an origin

- Two URLs have the *same origin* if the **protocol**, port (if specified), and host are the same for both.
  - ex: `https://store.company.com/page.html` and `http://store.company.com:81/dir/page.html` is not same origin
- Scripts executed from pages with an `about:blank` or `javascript:` URL **inherit the origin** of the document containing that URL, since these types of URLs do not contain information about an origin server.

## Functional overview

- Simple requests which don’t trigger a CORS preflight.
- Preflighted requests
- Requests with credentials(沒研究)

## Headers

- Response from server
  - **Access-Control-Allow-Origin**
  - Access-Control-Expose-Headers
  - Access-Control-Max-Age
  - Access-Control-Allow-Credentials
  - Access-Control-Allow-Methods
  - Access-Control-Allow-Headers
- Request from client
  - Origin
  - Access-Control-Request-Method
  - Access-Control-Request-Headers

## Related APIs

[Location](https://developer.mozilla.org/en-US/docs/Web/API/Location)

[Location.origin](https://developer.mozilla.org/en-US/docs/Web/API/HTMLHyperlinkElementUtils/origin)

[window.location](https://developer.mozilla.org/en-US/docs/Web/API/Window/location)

[window.frameElement](https://developer.mozilla.org/en-US/docs/Web/API/Window/frameElement)

[window.frames](https://developer.mozilla.org/en-US/docs/Web/API/Window/frames)

## When frameElement null

>Window.frameElement</br> Returns the element (such as `<iframe>` or `<object>`) in which the window is embedded, or null if the element is either top-level or **is embedded into a document with a different script origin**; that is, in cross-origin situations.

## file:// in CORS

If you're trying to do a cross-domain XMLHttpRequest via CORS...

1. Make sure you're testing via `http://`. Scripts running via `file://` have limited support for CORS.
2. Make sure the browser actually supports CORS. (Opera and Internet Explorer are late to the party)