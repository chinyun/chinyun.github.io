---
title: CORS
date: 2019-11-02 21:00:05
tags:
---

# 什麼是 CORS?

Cross-Origin Resource Sharing 跨來源資源共享，這是由 W3C 頒布的一種瀏覽器技術規範，透過傳輸 HTTP Header 判斷阻擋或允許不同來源網域的資源存取。

當使用者請求一個不是目前文件來源，例如不同網域(domain/host)、通訊協定(protocol)或通訊埠(port)的資源時，會建立一個**跨來源 HTTP 請求**(cross-origin http request)。

而瀏覽器基於安全性考量，程式碼所發出的跨來源 HTTP 請求會受到限制，若不受限制的話，駭客可以透過撰寫程式碼跨域撈資料來進行跨站腳本攻擊(XSS)，而這個限制只有在瀏覽器上，若在本機使用 node.js 則沒有限制。

## 同源政策（same-origin policy）

在同源政策（same-origin policy）中規範了那寫資源可以跨源存取，哪些會受到限制。
同源的定義簡單如下：
- 不同網域（Domain）
- 不同通訊協定：HTTP, HTTPS, FTP
- 不同連接埠號（Port）

只要瀏覽器發的 request 與 server 所屬不同源，request 就會被瀏覽器阻擋。不過即便擋下來，其實 request 已經發出去，server 也確實給予瀏覽器 response，只是瀏覽器基於同源政策，因此不把拿到的回應給你的 JavaScript 去做進一步的處理。

一般來說跨來源寫（Cross-origin writes）、跨來源嵌入（Cross-origin embedding）是被允許的，而跨來源讀取（Cross-origin reads）是受限制的。

1. 受同源政策管理的跨來源請求：
XMLHttpRequest 及 Fetch 都遵守同源政策(same-origin policy)，**這代表網路應用程式所使用的 API 除非使用 CORS 標頭否則只能請求與應用程式相同網域的 HTTP 資源**。
如果想開啟跨來源 HTTP 請求的話， server 必須在 response 的 header 加上 `Access-Control-Allow-Origin: *`表示接受所有不同來源的跨域請求，或者可以指定接受特定網址的跨域請求：
```
Access-Control-Allow-Origin: http://foo.example
```

其他方法還有：
- Access-Control-Allow-Headers：指定哪些 HTTP 標頭可以於實際請求中使用。
- Access-Control-Allow-Methods：GET, POST, PUT，存取資源所允許的方法，用來回應預檢請求。
- Access-Control-Expose-Headers：瀏覽器能夠存取伺服器回應當中哪些標頭。
- Access-Control-Max-Age：預檢請求之結果可以被快取的秒數。
- Access-Control-Allow-Credentials：用於驗證請求中，用來告知瀏覽器是否允許 client-side 向 server-side 傳送 cookie，默認為 false：CORS 規範會阻止跨域 AJAX 向 server-side 發送 cookie。

2. 不受同源政策管理的 html tags 包括：`<script src='' />`、`<img/>`、`<video/>`、`<audio/>`、`<iframe/>`、`<link rel='stylesheet' href />` 這些資源本來就能夠跨域存取。

## JSON with Padding(JSONP)

JSONP 是一個利用 script tag 不受同源政策限制的特性，直接載入帶參數的 js 程式碼的跨來源請求實作方法，實務上在操作 JSONP 的時候，Server 通常會提供一個callback的參數讓 client 端帶過去。

Twitch API 有提供 JSONP 的版本，我們可以直接來看範例：

URL: https://api.twitch.tv/kraken/games/top?client_id=xxx&callback=receiveData&limit=1
```
receiveData({"_total":1067,"_links":{"self":"https://api.twitch.tv/kraken/games/top?limit=1","next":"https://api.twitch.tv/kraken/games/top?limit=1\u0026offset=1"},"top":[{"game":{"name":"Dota 2","popularity":63361,"_id":29595,"giantbomb_id":32887,"box":{"large":"https://static-cdn.jtvnw.net/ttv-boxart/Dota%202-272x380.jpg","medium":"https://static-cdn.jtvnw.net/ttv-boxart/Dota%202-136x190.jpg","small":"https://static-cdn.jtvnw.net/ttv-boxart/Dota%202-52x72.jpg","template":"https://static-cdn.jtvnw.net/ttv-boxart/Dota%202-{width}x{height}.jpg"},"logo":{"large":"https://static-cdn.jtvnw.net/ttv-logoart/Dota%202-240x144.jpg","medium":"https://static-cdn.jtvnw.net/ttv-logoart/Dota%202-120x72.jpg","small":"https://static-cdn.jtvnw.net/ttv-logoart/Dota%202-60x36.jpg","template":"https://static-cdn.jtvnw.net/ttv-logoart/Dota%202-{width}x{height}.jpg"},"_links":{},"localized_name":"Dota 2","locale":"zh-tw"},"viewers":65622,"channels":376}]})
```
結合起來就是：
```
<script src="https://api.twitch.tv/kraken/games/top?client_id=xxx&callback=receiveData&limit=1"></script>
<script>
  function receiveData (response) {
    console.log(response);
  }
</script>
```

利用 JSONP 也可以存取跨來源的資料。但 JSONP 的缺點就是你要帶的那些參數永遠都只能用附加在網址上的方式（GET）帶過去，沒辦法用 POST。

# Reference

[輕鬆理解 Ajax 與跨來源請求](https://blog.techbridge.cc/2017/05/20/api-ajax-cors-and-jsonp/)
[[JS] 同源政策與跨來源資源共用（CORS）](https://pjchender.github.io/2018/08/20/%E5%90%8C%E6%BA%90%E6%94%BF%E7%AD%96%E8%88%87%E8%B7%A8%E4%BE%86%E6%BA%90%E8%B3%87%E6%BA%90%E5%85%B1%E7%94%A8%EF%BC%88cors%EF%BC%89/)
