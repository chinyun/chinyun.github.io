---
title: JS-module
date: 2019-11-16 00:19:36
tags:
---

# 什麼是 module？為什麼要用 module？

### 什麼是 module？
Modules are just clusters of code. Good authors divide their books into chapters and sections; good programmers divide their programs into modules.
在進行軟體專案時，複雜性總是伴隨，妥善應用抽象、介面和他們底層的概念，將某項功能的需要注意的複雜性最小化，直到他變成單一功能的分支，也就是將一個大的 program 拆分成互相依賴的小文件、再用簡單的方法拼裝起來，透過妥善設計的介面(API)讓使用者知道如何引用一個 module ，這樣的思維就是模組化開發思維。

Good module should be highly self-contained with distinct functionally, allowing them to be shuffled, removed, or added as neccesary, without disrupting the system as a whole.

透過提供一個讓系統其他部分操作的公開介面(API)，這個介面(API)裡面有元件公開的方法或屬性，那些方法和屬性也可以稱為接觸點，也就是可在介面(API)公開互動的東西。接觸點越多，需要操作的地方也越多，也有較高的彈性，因為有大量的功能，所以可能也會更難理解、使用。
介面(API)有兩種用途：幫助我們開發元件的新功能、讓使用介面的人(元件、系統)可以受惠於公開的功能，而不需考慮那項功能的背後細節如何實作。因此設計良好合適的介面可以增加功能開發的生產力，當我們持續使用類似的API外貌，就不需要每次都重新擬定新的設計，使用者也可以放心使用。

### 為什麼要模組化開發？

隨著專案日漸複雜，全域變量容易互相衝突，影響專案的維護性和可拓展性。使用模組化開發方法的好處有：
 1. 提升維護性 (Maintainability)
 2. 命名空間 (Namingspacing)
 3. 提供可重用性 (Reusability)

在 JavaScript 中，管理變量 variable 是一切 coding 的基礎，看起來似乎是越少變量越容易管理，不過透過 scope 的概念可以讓變量在單一作用域內產生作用，不同作用域的變量便不會互相干擾，然而當你想要共用變量或資料時卻只能透過全域變量的設定，或用 callback function 將變量傳出來，設定全域變量容易產生命名衝突，而且當所有變量都需要在全域中，就必須按照一定順序編程。當需要移除舊程式碼、製作新功能時，就會有困難。所以有 module 開發的必要性。

----------

# JavaScript Module History

早期 JavaScript 沒有 module 體系，其他語言都有這個功能，例如：Ruby 的 require、Python 的 import，連 css 都有 @import。因為有開發上的實際需要，所以社群制定了一些 module pattern 的加載方法，最主要的有 CommonJS 和 AMD 規範，前者用於 server 後者用於 browser。 

- **AMD(Async Module Definition)** 非同步載入模組規範
- **CommonJS** 同步模組載入規範，Node.js 遵守其規範，透過 require 進行模組同步載入，並透過 exports, module.exports 來輸出模組。主要實現為 Node.js 伺服器端的同步載入和瀏覽器端的 Browserify。
- CMD(Common Module Definition) 依賴就近，延遲執行
- **CND-Based, inline-script, Script Tags** 最傳統的 `<script></script>`引入方式，但開發大型專案時容易產生弊端：
  - global variable 污染、衝突
  - 文件只能按照順序載入，須由開發者自行判斷模組與函式庫之間的依賴性
  - 資源與版本難以維護
- UMD(Universal Module Definition) 為了兼容 AMD 和 CommonJS。
- **ES6 Module** (2015年)
  ES6 Module 的標準中定義了 JavaScript 的模組化方式，自此產生了 JavaScript 原生的模組語法，其功能可以取代之前社群使用的規範，提供靜態的宣告式 API、以及採用 promise 的動態宣告式，在編譯時就確定 module 之間的依賴關係，以及輸出和輸入的變量。而 CommonJS 只能在運行時確定這些東西。目前 browser 和 Node.js 對 ES6 模組支援度還不完整，大部分還需要透過 babel 轉譯器進行轉譯。

----------

# ES6 module 現代模組化開發方法

在 ES6 module 中，每個檔案都是一個模組，有自己的範圍和環境，通過 `export` 命令輸出指定 code，再用 `import` 命令輸入文件。（自動採用嚴格模式）
過去 server 開發時會使用一個叫做 Browserify 的 module bundler， Browserify  使用 CommonJS 語法，會產生一個 bundle.js 檔案，讓我們能夠上傳並發佈網站。而現在我們會使用 **ES6 module + Webpack + Babel**，Webpack 也是一個類似於 Browserify 的 module bundler，但 Webpack 可以結合 Babel 轉譯器，讓瀏覽器也讀懂 ES6，並產生 bundle.js 檔案。
現代的模組化開發多使用以下工具(庫)：

### NPM (Node Package Manager)

一個強大的套件管理工具、線上資料庫，擁有龐大的社群支持，一開始是為了 Node.js 而發展出來的套件工具，隨著發展變成開發者社群用來 share code、package 的資料庫，透過 npm 提供的指令，可以管理套件版本、加載套件、卸除套件等。使用套件做開發，簡化重複的程式碼，可以加快開發速度和提升程式維護性。

npm 在初始化專案時會產生一個 package.json 的檔案，裡面紀錄專案所使用的 packages 版本紀錄，透過檔案紀錄可以確保團隊工作時所有人使用的版本一致。

其他套件管理工具還有由 facebook 開發的 Yarn，目前也有部分人在使用。

----------

### Wepack

Wepack is a static module bundler for morden JavaScript applications.
運行在 Node.js 環境的一個開發工具。

- 可以使用 CommonJS + AMD + ES6 module 規範。
- 使用 import 語法和 export 的語法，在 browser 上實現模組化。
- 能夠將圖片、css 等資源，和 js 檔案都一併打包。
- 能夠編譯 Sass、Less、JS 擴充語法(JSX, Coffee Script, TypeScript)
- 可用的擴充 plugin 很多

實作方法：在 `webpack.config.js` 中設定要進行 bundle 的檔案。自己編寫設定檔，透過指令去驅動的自動化工具，所有的動作都需要自己寫規則去做編譯，將我們寫好的前端框架檔案(preprocess)編譯成目前 browser 看得懂的內容並打包成一包的完整檔案，提交給 server，會在專案底下創建一個 webpack.config.js 和 index.js 檔案。

----------

### Babel

Babel is a JavaScript compiler that takes your morden JS code and returns browser compatible JS(older JS code).
有很多開發工具（ex:create-react-app）都自動搭載了 babel 作為轉譯工具。他也可以轉換 react 的 jsx 還有 typescript。

----------

### Gulp

自動化工作流程的 library。可以 compile css 也可以 compile JavaScript。
[官網](https://gulpjs.com)

----------

### Eslint

程式碼 coding style 檢查工具。

----------

# Reference

深入學習 JavaScript 模組化設計 - NICOLAS BEVACQUA 著 O'REILLY 
[搞懂為何設定 REACT、JSX、ES2015、BABEL、WEBPACK 的學習筆記](http://blog.turn.tw/?p=3532)
