---
title: 'CSRF, XSS, SQL injection'
date: 2019-11-02 20:32:24
tags:
---

Web Security 網頁安全泛指針對網頁、網站上的資訊安全，涵蓋程式漏洞、邏輯漏洞、資訊洩漏等。

#Cross Site Request Forgery(XSRF or CSRF)

指在受害者不知情狀況下，被其他網域借用身份來完成未經同意的 HTTP Request。
當網站使用 cookie-based authentication，會把 session id 和 session data 存在 cookie 中，剛使用者曾在其他網站登入而未登出，並且進入惡意網站操作時，攻擊者利用可以進行跨域 http 請求的 http tag(ex: img、iframe等)埋入惡意的 request 程式碼，觸發後因為使用者未登出，因此攻擊者獲得 cookie 資料，攻擊者得以冒用使用者身份通過驗證。得以任意改變使者者在資料庫的資料。

同源政策對於連結(links)、重新導向(redirect)、表單(form)等的跨來源存取都是寬鬆的，因此駭客利用此特性做 CSRF 攻擊。

如何防範：
1. 使用 token-based authentication
2. 使用 HttpOnly 來防止 Cookie 被 JavaScript 讀取
3. 目前各大成熟的框架，都有針對傳遞資料做 CSRF 驗證，網站會產生的一組密碼，並在請求送出時，檢查那組密碼是否正確。

Cross Site Request Forgery attacks are not an issue if you are using JWT with local storage. On the other hand, if your use case requires you to store the JWT in a cookie, you will need to protect against XSRF. XSRF are not as easily understood as XSS attacks. Explaining how XSRF attacks work can be time-consuming, so instead, check out this really good guide that explains in-depth how XSRF attacks work. Luckily, preventing XSRF attacks is a fairly simple matter. To over-simplify, protecting against an XSRF attack, your server, upon establishing a session with a client will generate a unique token (note this is not a JWT). Then, anytime data is submitted to your server, a hidden input field will contain this token and the server will check to make sure the tokens match. Again, as our recommendation is to store the JWT in **local storage**, you probably will not have to worry about XSRF attacks.

One of the best ways to protect your users and servers is to have a **short expiration time** for tokens. That way, even if a token is compromised, it will quickly become useless. Additionally, you may maintain a blacklist of compromised tokens and not allow those tokens access to the system. Finally, the nuclear approach would be to change the signing algorithm, which would invalidate all active tokens and require all of your users to log in again. This approach is not easily recommended, but is available in the event of a severe breach.

# Cross Site Scripting(XSS)

「XSS攻擊通常指的是通過利用網頁開發時留下的漏洞，通過巧妙的方法注入惡意指令程式碼到網頁，使用戶載入並執行攻擊者惡意製造的網頁程式。 這些惡意網頁程式通常是JavaScript，但實際上也可以包括Java，VBScript，ActiveX，Flash或者甚至是普通的HTML。」--wikipedia

使用者以為安全進入網站後反而被導向到釣魚網站、甚至被竊取帳號密碼、個人資料。
目前 XSS 攻擊的種類大致可以分成以下幾種類型：
- Stored XSS (儲存型)：
會被保存在伺服器資料庫中的 JavaScript 代碼引起的攻擊即為 Stored XSS，最常見的就是論壇文章、留言板等等，因為使用者可以輸入任意內容，若沒有確實檢查，那使用者輸入如 ｀`<script>` 等關鍵字就會被當成正常的 HTML 執行，標籤的內容也會被正常的作為 JavaScript 代碼執行。

- Reflected XSS (反射型)：
由網頁後端直接嵌入由前端使用者所傳送過來的內容造成的，最常見的就是以 GET 方法傳送資料給伺服器時，伺服器未檢查就將內容回應到網頁上所產生的漏洞。

- DOM-Based XSS (基於 DOM 的類型):
網頁上的 JavaScript 在執行過程中，沒有詳細檢查，輸入任意的內容都會被建立成有效的 DOM 物件，包含嵌入的代碼也會被執行，使得操作 DOM 的過程代入了惡意指令。但這樣的攻擊除非攻擊者親自到受害者電腦前輸入，否則不可能讓受害者輸入這種惡意代碼，通常需要搭配前兩個手法；讓內容保存在伺服器資料庫中、或是以反射型的方式製造出內容，再藉由JavaScript 動態產生有效的 DOM 物件來運行惡意代碼。

如何防範：
1. 前兩種 Stored、Reflected 的類型都必須由後端進行防範，除了必要的 HTML 代碼，任何允許使用者輸入的內容都需要檢查，刪除所有「`<script>`」、「 `onerror=`」及其他任何可能執行代碼的字串。

2. DOM-Based 則必須由前端來防範，但基本上還是跟前面的原則相同。
另外不同的一點就是應該選擇正確的方法、屬性來操作 DOM，譬如`document.getElementById('show_name').innerHTML = name;` 中的`innerHTML `屬性代表插入的內容是合法的 HTML 字串，所以字串會解析成 DOM 物件，此處使用` innerText`會被保證作為純粹的文字，也就不可能被插入惡意代碼執行了。

Cross Site Scripting attacks occur when an outside entity is able to execute code within your website or app. The most common attack vector here is if your website allows **inputs** that are not properly sanitized. **If an attacker can execute code on your domain, your JWT tokens are vulnerable.** Our CTO has argued in the past that XSS attacks are much easier to deal with compared to XSRF attacks because they are generally better understood. **Many frameworks, including Angular, automatically sanitize inputs and prevent arbitrary code execution.** If you are not using a framework that sanitizes input/output out-of-the-box, you can look at plugins like caja developed by Google to assist. Sanitizing inputs is a solved issue in many frameworks and languages and I would recommend using a framework or plugin vs building your own.

# SQL injection

「Injection 是一種駭客常用的攻擊概念，透過注入惡意的程式碼的方式，破壞原本程式碼的語意，來達到攻擊的效果。SQL 是網站常用來取得資料的程式語法，SQL Injection 便是駭客透過修改 SQL 語句，改變他的語意，達成竊取資料/破壞資料的行為。」
--[一次看懂 SQL Injection 的攻擊原理](https://medium.com/@jaydenlin/淺談駭客攻擊-網站安全-一次看懂-sql-injection-的攻擊原理-b1994fd2392a)

「在輸入的字串之中夾帶SQL指令，在設計不良的程式當中忽略了字元檢查，那麼這些夾帶進去的惡意指令就會被資料庫伺服器誤認為是正常的SQL指令而執行，因此遭到破壞或是入侵。」
--wikipedia

「在 SQL 語法中常會出現攻擊者可以透過更改語法邏輯或加入特殊指令的方式，在未設定過濾惡意程式碼的情況下，資料庫伺服器（DataBase）會直接接收使用者所輸入的 SQL 指令並執行攻擊代碼，使攻擊者能夠取得最高權限，得以擅自竊取、修改、挪動或刪除資料的可能。此類漏洞無疑的會對公司或商號造成相當大的損傷。」 
--[攻擊行為－SQL 資料隱碼攻擊 SQL injection](https://ithelp.ithome.com.tw/articles/10189201)

常見攻擊手法：
- Authorization Bypass（略過權限檢查）
- Injecting SQL Sub-Statements into SQL Queries（注入 SQL 子語法）
- Exploiting Stored Procedures（利用預存程序）

防範手法：
- 使用 SQL query builder for JavaScript - knex.js：

Read carefully from knex documentation how to pass values to knex raw (http://knexjs.org/#Raw).

If you are passing values as parameter binding to raw like:
```
knex.raw('select * from foo where id = ?', [1])
```
In that case parameters and query string are passed separately to database driver protecting query from SQL injection.

Other query builder methods always uses binding format internally so they are safe too.

Biggest mistake that one can do with knex raw queries is to use javascript template string and interpolate variables directly to SQL string format like:
```
knex.raw(`select * from foo where id = ${id}`) // NEVER DO THIS 
```
One thing to note is that knex table/identifier names cannot be passed as bindings to driver, so with those one should be extra careful to not read table / column names from user and use them without properly validating them first.

Refer from:[Does Knex.js prevent sql injection?](https://stackoverflow.com/questions/49665023/does-knex-js-prevent-sql-injection/49665379)

- 使用 ORM(object relational mapping)：
在資料庫和 model資料容器之間的框架，他可以幫助開發者更簡便安全的去資料庫讀取資料，透過 ruby, java 等程式語言，去操作資料庫語言。同時因為是操作程式語言，若 query 中的值不符合預期格式，框架會自動擋掉而不會讓 SQL injection 成功。目前大部分的網站都是使用框架來開發，而這些框架都是使用 ORM 來處理他們的資料庫，因此在 ORM 的保護下，不是我們預期的資料格式，而是 SQL 語法的話，具有基本的保護能力。

- 參數化查詢（parameterized query 或 parameterized statement）：
是指在設計與資料庫連結並存取資料時，在需要填入數值或資料的地方，使用參數（parameter）來給值，這個方法目前已被視為最有效可預防SQL注入攻擊的攻擊手法的防禦方式。 -- [參數化查詢](https://www.wikiwand.com/zh-tw/%E5%8F%83%E6%95%B8%E5%8C%96%E6%9F%A5%E8%A9%A2)
- 使用 Regular expression 驗證過濾輸入值與參數中惡意代碼，將輸入值中的單引號置換為雙引號。
- 限制輸入字元格式並檢查輸入長度。
- 資料庫設定使用者帳號權限，限制某些管道使用者無法作資料庫存取。

# Reference
[CSRF](https://www.gnucitizen.org/blog/csrf-demystified/)
[網頁安全】給網頁開發新人的 XSS 攻擊 介紹與防範](https://forum.gamer.com.tw/Co.php?bsn=60292&sn=11267)
[資安常見攻擊( Web ) 與學習方法](https://ithelp.ithome.com.tw/articles/10209227)
