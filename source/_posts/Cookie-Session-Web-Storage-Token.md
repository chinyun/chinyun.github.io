---
title: 'Cookie, Session, Web Storage, Token'
date: 2019-11-01 22:24:34
tags:
---

根據 google 開發者指南，選擇正確的網頁存儲機制對本地設備和基於雲端的服務器存儲都很重要，良好的存儲引擎可以確保以可靠的方式保存信息，同時減少佔用的網路頻寬並提升響應能力，正確的存儲緩存策略是實現離線移動網頁體驗的核心構建基礎。
--[Never Load the Same Resource Twice - 網頁存儲概覽](https://developers.google.com/web/fundamentals/instant-and-offline/web-storage)

# Session 是什麼？
session 可以被翻譯作「一系列動作、具有上下文意義的狀態」或被簡單的翻作「會話」、「工作階段」。
由於 http 協定是無狀態(stateless)的設計，server 和 client 不會一直保持連線狀態，也不會有雙方狀態的即時更新，每一次的請求和回應的都是獨立的，沒辦法記憶內容，而 session 就是讓 client 和 server 之間保持狀態的解決方案。
session 可以說是一種機制，可以讓伺服器記住訪問者的身份，避免短時間內重複載入，不同程式語言對於 session 機制有不同的實現方法。

# Session 機制的實作

### 網址

將 request 內容附加在網址上，server 透過網址資訊判斷 client 狀態。 

### Cookies

cookie 是最常見的方式，可以把資訊記錄在本機端的文檔中，網站只能儲存使用者確實輸入過的內容而無法存取電腦內的資料。
cookie 的機制就是當 clinet 發送 request 時，server 收到 request 後透過 Set-Cookie 語法讓瀏覽器儲存一些內容(設置 cookie)，而這些內容會在瀏覽器發送 request 時一併送達 server，server 透過 cookie 內容決定狀態。

- cookie-based session: 把所有 session 狀態存在 cookie 裡面並且加密
- 在 cookie 只儲存 session id，其餘狀態(session data)都存在 server，透過 session id 來向 server 請求訪問資料。
> 但 cookie 的問題是認證不認人，一旦 cookie、session id 被偷走，別人就可以偽造身份來登入，這也就是為什麼有時候如果駭客取得一大串 session id 的時候，有用戶會發現帳戶被系統自動登出，讓該 session id 失效。
> 由於 session 代表意義廣泛、多處有使用到此詞，因此有時候口語中講到 session 也會直接指存在 cookie 中的 session id 和 session data。
- 要注意 HttpOnly 與 Secure 的設定值

cookie-based authentication 流程：

1. User enters their login credentials.
2. Server verifies the credentials are correct and creates a session which is then stored in a database.
3. A cookie with the session ID is placed in the users browser.
4. On subsequent requests, the session ID is verified against the database and if valid the request processed.
5. Once a user logs out of the app, the session is destroyed both client-side and server-side.

### Token

Token-based authentication 是無狀態的，server 不會保留有關哪些用戶已登入或已發出哪些 token 的紀錄，取而代之的是，對 server 的每個請求都帶有一個 token，server 使用該 token 來驗證請求的真實性。
The token is generally sent as an addition Authorization header in the form of Bearer {JWT}, but can additionally be sent in the body of a POST request or even as a query parameter. 

一般 token 的流程如下：

1. User enters their login credentials.
2. Server verifies the credentials are correct and returns a signed token.
3. This token is stored client-side, most commonly in local storage - but can be stored in session storage or a cookie as well.
4. Subsequent requests to the server include this token as an additional Authorization header or through one of the other methods mentioned above.
5. The server decodes the JWT and if the token is valid processes the request.
6. Once a user logs out, the token is destroyed client-side, no interaction with the server is necessary.

Token 優於傳統 cookie-based 方法的原因為：
- Stateless, Scalable, and Decoupled，
使用 token-based appoarch，後端不需要保存 token 的紀錄，每個 token 包含驗證所需的資訊，server的工作只要給予成功登入的請求 signed token，並且驗證後續的 token 是否一致。 Third party services such as **Auth0** can handle the issuing of tokens and then the server only needs to verify the validity of the token.
- Cross Domain and CORS，不同於 cookies 只能在單網域或子網域中運作，token-based approach with CORS enabled 所以可以被不同 services 和 domains 訪問。

### JWT (JSON Web Token)

A JSON Web Token is comprised of three parts: the header, payload, and signature.
Header:
```
{
  'typ': 'JWT', # 聲明類型
  'alg': 'HS256' # 加密的方法: HMAC、SHA256、RSA 進行 Base64 編碼
}
```
Payload: 用來放用來放必要的資料(通常是用來認證使用者的資料)或額外的資料(metadata)，存放溝通訊息。
Signature: 由三個部分組成，加密後的 header、加密後的 payload以及 secret，secret 要保存在 server 端，JWT 的簽發驗證都必須使用這個 secret，當其他人得知這個 secret 就表示 client-side 可以自己簽發 JWT，因此任何場景都不應該外流。

Tokens Are Signed, Not Encrypted. Token could be signed by the HMACSHA256 algorithm, and the header and payload are Base64URL encoded, it is not encrypted. Base64URL can be decoded.
It should go without saying that sensitive data, such as passwords, should never be stored in the payload. If you must store sensitive data in the payload or your use case calls for the JWT to be obscured, you can use JSON Web Encryption (JWE). JWE allows you to encrypt the contents of a JWT so that it is not readable by anyone but the server.

Commonly, the JWT is placed in the browser's local storage and this works well for most use cases.There are some issues with storing JWTs in local storage to be aware of. Unlike cookies, local storage is sandboxed to a specific domain and its data cannot be accessed by any other domain including sub-domains.

You can store the token in a cookie instead, but the max size of a cookie is only 4kb so that may be problematic if you have many claims attached to the token. Additionally, you can store the token in **session storage** which is similar to local storage but is cleared as soon as the user closes the browser.

### Web Storage

HTML5 提供的 web storage 方法分成兩種： session storage 和 local storage。
這是一種可以讓網頁儲存資料於本地端的技術，作用與 cookie 相同，但與只有 4k 容量的 cookie 不同，基本上一般瀏覽器支援 5M 的儲存空間。而且 cookie 會在伺服器和用戶端之間旅行，而 web storage 則是儲存於用戶端，不會佔用網路頻寬、不影響網站效能，因此可以把安全性低而量較大的資料以 web storage 形式儲存。

web storage 儲存形式為 string，存取的程式指令寫在用戶端的 JavaScript 中，資料結構為 key/value pair。

- local storage: 永久性儲存在瀏覽器中，關閉瀏覽器也不會消失，除非手動刪除。
- session storage: 只存在單一分頁，開新分頁即是新的 session，資料無法跨分頁分享。

隔離：瀏覽器會把不同網站的 local storage 隔離開來，基於「同源政策」避免不同網域的訪問者進行惡意攻擊，某個網站的網頁所儲存於 local storage 的資料，其他網站是看不到的，只有來自相同網站的網頁才能共享資料。

- 不同協定視為不同源： HTTP 和 HTTPS 不同源
- 主機名稱： 相同 host name 底下的虛擬路徑屬於相同來源，不同主機名稱視為不同源

# Session 安全設計原則

- 需有一定期限的生命週期（expire機制）
- 盡量利用原有架構 session 管理的機制
- 登入成功後的 sessoin id 必須強制更換

# Notes
- [深入 Session 與 Cookie：Express、PHP 與 Rails 的實作](https://github.com/aszx87410/blog/issues/46)
sessionID 如何產生、sessionID 儲存方式、session 資訊儲存方式

- [Day24 - Cookie在express上的應用—登入實作為例。](https://ithelp.ithome.com.tw/articles/10187343)

# Reference
- [淺談 Session 與 Cookie：一起來讀 RFC](https://github.com/aszx87410/blog/issues/45)
- [Node.js: Cookie and Session](https://cythilya.github.io/2015/08/18/node-cookie-and-session/)
- [我遇過最難的 cookie 問題](https://github.com/aszx87410/blog/issues/17)
- [Cookies vs. Tokens: The Definitive Guide](https://dzone.com/articles/cookies-vs-tokens-the-definitive-guide)

# 延伸閱讀
- [CSRF, XSS, SQL injection]()
- [HTTPS/HTTP/JSON/AJAX]()
- [CORS 同源政策]()
- [Hash function & Encryption]()
- [Cache & HTTP Cache]()
