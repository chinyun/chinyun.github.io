---
title: HTTPS/HTTP/JSON/AJAX/RestfulAPI
date: 2019-11-02 23:52:26
tags:
---

# How is the Internet work?

現代網路運作的基礎主要由瀏覽器、伺服器以及網路資料的傳遞組成，當使用者在自己電腦透過瀏覽器瀏覽網頁時，會從瀏覽器發出造訪某網頁網址的請求，而該網址所屬的網站伺服器接收到請求內容後，返回包含了 HTML、CSS、JavaScript 等的檔案，瀏覽器接到回應之後將網站呈現在畫面上，上述這個瀏覽器發出請求(request)、伺服器給予回應(response)的過程就是一般我們在瀏覽器輸入網址後，到瀏覽器渲染出頁面之間發生的事情。

網路資料傳遞的原則：
1. 有標準化的內容格式
2. 資料結構分為 header 和 body
3. 用狀態碼 http status 標準化傳輸結果
4. 用動詞 http method 標準化動作： GET、POST、DELETE、PUT、PATCH
5. protocol 協定：定義資料交換的格式，標準化促成規模化 ex: http、https

# HTTP vs. HTTPS

- HTTP(Hyper Text Transfer Protocol) 超文本傳輸協定
In 1989 Tim Berners-Lee invented the World Wide Web and built the HTTP for transferring HTML document around the world. HTTP is a protocol or the rules that we use over the wires. It is a foundation of any data exchange on the web.
  - HTTP Method
    - GET：透過 API 把資料內容撈出來，這些內容通常是可以對外公布，不需要密碼帳號就可以取得的資料。
    - POST：新增一筆資料
    - DELETE：刪除資料
    - PUT：更新一筆資料，如果存在這筆資料就會覆蓋過去
    - PATCH：部分更新資料

- HTTPS(Hyper Text Transfer Protocol Secure) 超文本安全傳輸協定
HTTPS(Hyper Text Transfer Protocol Secure) use TLS and SSL technology to encrypt the information that transported. Only the server or client web could have the secret key to read the message.

主要是由網景公司(Netscape)開發並內建於其瀏覽器中，之後廣泛發展於網際網路上，用於對資料進行壓縮傳輸及傳輸後解壓縮回正確資訊的操作。隨著網路的發達，資訊技術越來越透明，HTTP 的開放明文顯得在網際網路中，不再是這麼安全的傳送協定，知名企業網站及全球級大型網站 Google、Facebook 等皆採用 HTTPS 加密，作為預設連線方式，因此有使用者登入系統或或存取到機密敏感資料頁面等有建立會員、購物車、金流、刷卡機制服務的網站選擇使用HTTPS的傳送協定。

HTTPS 其實就是在原本的 HTTP 協定中延伸加入 SSL(Secure Sockets Layer,傳輸層安全協議) 或 TLS(Transport Layer Security，傳輸層安全) 憑證的安全連線技術。SSL/TLS 憑證是網頁伺服器和瀏覽器之間以加解密方式溝通的安全技術標準，透過憑證內的公開金鑰加密資料傳輸至伺服器端，伺服器端用私密金鑰解密來證明自己的身份，取得有效憑證後在網際網路上傳輸加密過的資料以達到資安的目的。

當您在伺服器與連線至伺服器的瀏覽器上安裝 SSL 憑證時，SSL 憑證的存在會觸發 SLL (或 TLS) 協議，這將加密伺服器與瀏覽器之間 (或伺服器之間) 發送的資訊；詳細資料顯然更加複雜一些。

運作流程：
1. SSL 在 TCP 連線建立後開始運作，啟動 SSL信號交換。
2. 伺服器發送憑證至使用者，同時附上大量規格 (包括哪個版本的 SSL/TLS 以及使用哪個加密方法等。)
3. 然後使用者檢查憑證的有效性，並選擇雙方都支援的最高等級的加密，並使用這些方法啟動安全的工作階段。有很多組方法可用優勢各異，被稱為密碼組。
4. 為了確保所有傳輸訊息的完整性與真實性，SSL 與 TLS 協議也包括使用訊息驗證程式碼的驗證程序。這些方法聽上去冗長又複雜，但在現實生活中是瞬間實現的。

SSL 直接在傳輸控制協議之上 (TCP) 運作，像安全感毛毯一樣有效。其允許最高的協議層不變，同時仍然提供安全連線。所以在 SSL 層下，其他協議層可以照常運作。若 SSL 憑證使用正確，所有攻擊者將看到哪個 IP 與通訊埠已連線以及大概在發送多少資料。他們或許可以終止連線，但是伺服器與使用者可以知道這是由第三方造成的。但是，他們無法攔截任何資料，所以使攻擊從根本上變得無效。

取得SSL憑證的方法就是向憑證廠商購買，
SSL憑證費用依照加密的等級、販售廠商而有所不同，可以依照網站性質與企業規模選擇SSL的加密程度以及衡量費用支出。一般來說，SSL憑證的發行廠商是不會影響搜尋排名，但是Google Chrome於2017年公布賽門鐵克(Symantec)發行的SSL視為無效化，並於2018/3/7公布Chrome 66連上賽門鐵克的SSL時呈現的警告畫面，被Chrome 視為無效的SSL憑證發行商名單:Thawte、VeriSign、Equifax、GeoTrust、RapidSSL、Symantec。

# AJAX and JSON

AJAX (**Asynchronous JavaScript And XML/JSON**) 非同步的 JavaScript 與 XML 技術，這是在2005年由Jesse James Garrett所發明的術語，描述一個使用多種既有技術的新方法。這些技術包括 HTTPS/HTTP 通訊協定、發 HTTP Request 方法(例如 fetch api)、XHR(XMLHttpRequest)或 JSON。使用 AJAX 能使網頁應用程式更快速、即時的更動介面內容，而不需要刷新整個頁面，讓程式更快回應使用者的操作，可以在網頁操作過程當中，不用刷新網頁就可以得到新的資訊。

- JavaScript：在 AJAX 技術裡，使用 JavaScript 將所有技術串連，當 server 回傳對應的資料給 client 時，我們可以透過 JavaScript 去解析和拆解這些資料，並使用 DOM 提供的方法把資料放進網頁。

- 非同步的 JavaScript：使用非同步的請求和回應，這表示當使用者點擊網頁中的一個連結時，即使該操作還未結束，仍可以對網頁做其他操作，同步的話則無法。

> 非同步 Asynchronous：指可以同時進行多件任務，而不需等待前一任務結束。同步 Synchronous:必須等待上一任務完成才能進行下一個。

- XML/JSON：
  - XML(Extensible Markup Language)：可延伸標記式語言是一種標記式語言，設計用來傳輸和儲存 data。標記指電腦所能理解的資訊符號，通過此種標記，電腦之間可以處理包含各種資訊的文章等。如何定義這些標記，既可以選擇國際通用的標記式語言，比如HTML，也可以使用像XML這樣由相關人士自由決定的標記式語言，這就是語言的可延伸性。XML是從標準通用標記式語言中簡化修改出來的。
  - JSON(JavaScript Object Notation)：是一種由道格拉斯·克羅克福特構想和設計、輕量級的資料交換語言，將結構化資料 (structured data) 呈現為 JavaScript 物件的標準格式，常用於網站上的資料呈現、傳輸 (例如將資料從伺服器送至用戶端，以利顯示網頁) JSON 可能是物件或字串，當你想從 JSON中讀取資料時，JSON可作為物件；當要跨網路傳送 JSON 時，就會是字串，因為 JSON 可以通過一個標準的 JS 函數解析(JSON.parse()將接收到的 JSON 內容解析成 JavaScript Object、JSON.stringfy()將 Object 轉成 sever 可以傳輸的 JSON string)，我們可以將任何 Object 轉換成 JSON，並使用 JSON 和 server 溝通，再由 JSON 轉換收到的訊息成 JavaScript Object。

兩者都是以讓人閱讀的文字為基礎，可以被轉譯成程式語言使用，讓文件能夠很容易地讓人去閱讀，同時又很容易讓電腦程式去辨識的語言格式和語法，不過 XML 相較之下更難被解析(parse)，須使用專屬的解析器，而 JSON 不須使用 end tag，JSON is parse into a ready-to-use JS Object，讀寫更簡潔、快速，可以使用 arrays(objects)格式來儲存資料，雖然 JSON 是以 JavaScript 語法為基礎，但可獨立使用，且許多程式設計環境亦可讀取 (剖析) 並產生 JSON。所以現在多使用 JSON 格式來傳輸資料。

- XHR(XMLHttpRequest)：是一個 JavaScript 物件，他的功能是用來向 server 發送非同步請求，在 XHR 技術剛被開發出來時，
使用 XML 作為請求、回應的資料格式，因此被稱為 XMLHttpRequest。現今提到 XMLHttpRequest 時已經不限於使用 XML 資料格式，也可以傳遞 XML、JSON、String 等多種資料格式。

- AJAX 進化史：
  - XHR(XMLHttpRequest) 大約10幾年前
  - XHR(XMLHttpRequest) v2 大約10年前
  - jQuery + XHR
  - 現在：Fetch API、Axios、Request、SuperAgent、Supertest等
    - Fetch：由 Mozilla 和 Google browser 在 2015年3月發佈實作的消息，它有不同於XHR思考角度的設計，基於 promise 的語法結構，而且設計足夠低階所以有更多彈性可擴展設定。目前不同瀏覽器版本還有不完全支援的，所以需要安裝 polyfill 當作暫時的解決方案。
    - Axios：Promise-based HTTP library for performing HTTP requests on both Node.js and Browser. It supports all mordern browser, even an included support for IE 8+. It's built on top of XMLHttpRequest for making AJAX calls. It lets you make HTTP requests from both the browser and the server. 
      - Intercept requests and responses before they are carried out.
      - Transform request and response data using promises.
      - Automatically transforms JSON data.
      - Cancel live requests.
      - 可以對 response setTimeout
      - Support protection against CSRF.
      - Has support for upload progress.
    - Request - A Simplified HTTP Client：The Request library is one of the simplest ways to make HTTP calls. The structure and syntax are very similar to that of how requests are handled in Node.js.
    ```
    var request = require('request');
    request('http://www.google.com', function (error, response, body) {
      console.log('error:', error); // Print the error if one occurred
      console.log('statusCode:', response && response.statusCode); // Print the response status code if a response was received
      console.log('body:', body); // Print the HTML for the Google homepage.
    });
    ```

Making HTTP calls from the client-side wasn’t this easy a decade ago. A front-end developer would have to rely on XMLHttpRequest which was hard to use and implement. The modern libraries and HTTP clients make the front-end features like user interactions, animations, asynchronous file uploads, etc., easier. 

# RESTful API

- API (Application Programming Interface) 應用程式介面
串接 API 指的就是在開發時透過程式介面操作資料；一個應用程式通常會開發其程式介面提供串接、讀取資料或應用服務，提供一套標準讓任何想接入應用程式服務的開發者，可以遵循規範、根據介面的設計撰寫程式碼，來得到想要的服務或資料。串接 API 的本質就是交換資料，通常我們會根據文件說明來使用他人寫好的 API，若要主動提供 API 時則需定義要給什麼資料。

- Web API 或 HTTP API
透過 HTTP 協定的 API，也就是用 HTTP 格式來提供資料的 API 可以稱為 Web API/HTTP API。根據 HTTP 規範，伺服器端和客戶端進行請求和回應時，會使用定義明確的請求格式(request format)和回應格式(response format)，request format 指的是 client-side 需使用特定的 HTTP verb 對 url 發出請求，response format 則是指 server-side 回應時採用的資料格式，有可能是 XML、JSON等。

HTTP 的組成元素：

| HTTP request | HTTP response | 
| -------- | -------- | 
| HTTP Method | Response Status |
| HTTP Headers | Response Headers |
| Request Body | Response Body | 

HTTP URL 組成：例如`https://www.example.com/photos?page=1`
通訊協定 `https://`、網域名稱 DNS `www.example.com`、路徑 path `/photos`、參數 parameter` ?page=1`(又稱 query string)

- RESTful API
RESTful 風格也是一種格式(format)，**RESTful API 就是指一種寫 API 格式的風格建議，目的是讓大家的寫法更有一致性。**
  - REST 
  REST 是 Representational State Transfer 的縮寫，由 Roy Fielding 博士在 2000 年的博士論文中所提出。他同時也是 HTTP 規範的主要作者之一。REST 是一種軟體架構風格（並非標準），目的是幫助在世界各地不同軟體、程式在網際網路中能夠互相傳遞訊息。而每一個網頁都可視為一個資源（resource）提供使用者使用，以資源操作的概念(指的是對某項資源，譬如 User、Post 等，指派 Show、Edit 等動作)，結合 URL path 與 HTTP Method ，目的是使 URL path 更為簡潔、容易被理解，除了介面簡潔之外，尚有增加快取 cache 效率、提升 api 活用性等優點。換句話說， REST 風格可以單從 HTTP request 就能看出如何操作伺服器的資料。

  REST 的一個最重要的觀念就是 resources (特定資訊的資源)，每一個 resource 由一個 global identifier (即 URL)所表示。為了操作這些 resources，網路的 components (client 跟 server) 透過標準化的介面 (HTTP) 來溝通並交換這些 resources 的 representations (實際上傳達資訊的文件)。
  任意數量的 connectors (如 clients, servers, caches, tunnels 等) 可以居中 request，但是都不可以 “seeing past” (不需要其他layer層)。這樣的應用程式跟一個 resource 互動根據兩件事情: resource 的 URL 跟要做的動作 — 它不需要知道是否有 caches, proxies, gateways, firewalls, tunnels, 或其他任何藏在 sever 之間的東西。這個應用程式只需要知道資訊的格式 (representation)，通常是 HTML 或 XML 或圖片什麼的。

  - RESTful
  設計為 REST 的系統稱為 RESTful。RESTful 路由便是充分應用 REST 風格的路由設置。而你可以透過 HTTP URL(Uniform Resource Locator)，也就是這些資源的地址(俗稱網址)，來取得這些資源並在你的瀏覽器上使用。
  RESTful API 建議我們使用 Standard HTTP Verb 來撰寫 API，並根據 API 接口的提供目的規劃 URL，例如 /resouces、/resouces/:id 來代表 resource collection 以及 individual resource 的 path 來規劃 API 介面。
  路由(routing)是指定義 app 如何將 client requset 指向一個正確的接口(endpoint)並做出相應的後續處理(給予 response)，一個 route 通常由下組成：
  `app.METHOD(PATH, HANDLER)`
  METHOD means HTTP request method, PATH is a path on the server and the HANDLER is the function executed when the routes is matched. PATH 是開放給 client 的 route，透過不同 path 導向不同動作。
  舉例：一個簡單的 route based on express.js：
  ```
  app.get('/', function(req, res){
    res.send('hello world');
  })
  ```
  `'/'`表示根目錄(root route)

# Reference

[Top JavaScript Libraries for Making AJAX Calls](https://dzone.com/articles/top-javascript-libraries-for-making-ajax-calls)
[Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
[HTTPS的威力？「S」對SEO與網站經營的四大重要性](https://www.awoo.com.tw/blog/https-seo/)
[SSL 憑證](https://www.websecurity.digicert.com/zh/hk/security-topics/what-is-ssl-tls-https)
[為何HTTPS憑證有貴有便宜還更可以免費？讓我們從CA原理開始講起。](https://progressbar.tw/posts/98)
