---
title: "HTTP caching"
---
## No caching
```
Cache-Control: no-store
```
The cache could not store anything. Each request must send to server.
Even you click back button in browser, it still reload resources.

## Cache but revalidate
```
Cache-Control: no-cache
```
`no-cache` does not mean do not cache. It mean revalidate with server every time before using a cached version of the URL.
It using ETag or Last-Modified to revalidate resources.

[https://web.dev/http-cache](https://web.dev/http-cache)
[https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)
