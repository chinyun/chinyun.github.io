---
title: 'JS hoisting,scope,closure'
date: 2019-11-12 19:59:07
tags:
---

Hoisting、Scope 和 Closure 在 JavaScript 中是很重要的觀念，因爲會影響我們如何撰寫 JavaScript。了解這三個概念可以幫助我們了解 JaveScript 在內文執行的運作原理，尤其是在創建和執行階段，JaveScript 的執行機制會如何理解我們寫的程式碼，並跑出我們想要的結果。

# Hoisting

JavaScript 內文執行的運作方式有一個機制叫做：Hoisting 提升，在執行任何程式碼前，JavaScript 會把**變數和函數的宣告**在編譯階段就放入記憶體，編譯後執行時因為已經宣告了，所以如此即便我們先寫調用某一函式的程式碼，再寫該函式的內容，JavaScript 也還是可以知道這段程式碼的意義，程式碼仍然可以運作：
```
catName("Chloe");

function catName(name) {
  console.log("My cat's name is " + name);
}
/*上面程式的結果是: "My cat's name is Chloe"*/
```
```
num = 6;
num + 7;
var num; 
/* 只要 num 有被宣告，就不會有錯誤 */
```
JavaScript 僅提升宣告的部分，而尚未賦值。如果在使用該變數後才宣告和初始化，那麼該值將是 undefined，以下範例顯示了這個特性。
```
var x = 1; // 給予 x 值
console.log(x + " " + y);  // '1 undefined'，此階段 y 尚未被賦予值
var y = 2;
```
上述程式碼其實是這樣運作的：
```
var x = 1; 
var y; // 宣告 y
console.log(x + " " + y);  // '1 undefined'
y = 2; // 賦值 y
```
**函數宣告的優先權比變數宣告高**，如果 function 調用時有傳參數進來，就會先宣告該參數代表的變數意義並賦值。
```
function test(v) {
  var v
  console.log(v)
  v = 3
}
test(10) // 10
```
需要注意的是，**只有 declaration 宣告式的 function (ex:`function func(){...}`)會被在編譯階段提升**，而 expression 表達式宣告的 function (ex:`let func = function(){...}`)會在執行階段才被存放到記憶體中。

# Scope 作用域

- Execution Context 執行環境 
要了解 Scope 須先知道 **Execution Context 執行環境** 的概念。
Execution Context is a fancy word for describing the environment in which your Javascript code runs.

當 JavaScript engine start up 程式碼準備好開始運行時，就會先建立 **global execution context全域執行環境**，然後建立一個 global object 和 this，在 browser 的環境中，global object 是 window， this === window，在 node.js 環境中， global object 是 global，this === global。
我們可以 assign variable、function 到 global object 中。

執行環境在建立時會經歷兩個階段，分別是 ：
- Creation Phase 創造階段：變數宣告和函數宣告提升，自動跳過函式裡的程式碼。
- Execution Phase 執行階段：由上到下、一行一行地執行程式，賦值也是在這階段。

當 JavaScript engine 看到 function name() 函數被執行，就會創建一個 function name() execution context，新的 function execution context 會被加入到**Execution stack 執行堆疊**，並依序執行(Javascript 是單一執行緒，一次只能做一件事)，執行環境 的堆疊過程是具有 順序性 的：first in last out。

- Scope 作用域是什麼？

**Scope determines the accessibility (visibility) of variables.** Scope is where can I access the variable where's that variable in my code. It just defines the accessibility of variables and functions in the code. JavaScript has function scope: Each function creates a new scope. Variables defined inside a function are not accessible (visible) from outside the function.

Scope 可以說是一個變數的生存範圍，出了這個範圍就無法存取到。在 JavaScript 裡面，可以分為兩種 Scope 作用域：
  - Global Scope：表示全域、任何地方都能存取得到
  - Lexical Scope：variable 被寫下來的那個地方，就是作用域
    - function scope
    - block scope
  it means that only by looking at the source code we can determine which environment the variables in data are avaliable in.
  這與 Lexical Environment 有關，在物理上我們將 code 寫在哪裡，那就是該 variable 或 function 的 Lexical Environment。 In JS our lexical scope (avalible data + variables where the function was defined) determines our avalible variables. Not the function is called.(相反地，有一種叫做 Dynamic scope 的作用域機制 就是在程式執行時才動態決定的) 

- Scope Chain
透過程式碼層層的包裹，由內而外，直到 global scope 的這一條 scope chain，可以幫助找到要找的對象(通常是 variables)被寫下的地方(lexical environment)。

在 ES6 以前，唯一產生作用域的方法就是 function，每一個 function 都有自己的作用域，在作用域外面你就存取不到這個 function 內部所定義的變數，然而 ES6 的時候引入了 let 跟 const，多了 block-scope 的概念。
延伸：[ES6: let, const, Block-Level Scope](https://cythilya.github.io/2016/10/28/es6-let-const-block-level-scope/)
 
因應ES6的出現，使用上建議大家不要再用var來宣告變數，改用let與const，而且優先使用const。
因為const在宣告時必須給定值，並且不能再被更改，這可以有效降低出現錯誤的機會。
同理，如果是需要變更的數值則改用作用範圍較小的let做宣告，來減少錯誤出現的機率，Ex: for迴圈。
-- [JavaScript 宣告: var、let、const](https://www.iware.com.tw/blog-JavaScript%20%E5%AE%A3%E5%91%8A:%20var%E3%80%81let%E3%80%81const.html)

# Closure 閉包

「閉包（英語：Closure），又稱詞法閉包（Lexical Closure）或函式閉包（function closures），是參照了自由變數的函式。這個被參照的自由變數將和這個函式一同存在，即使已經離開了創造它的環境也不例外。閉包是由函式和與其相關的參照環境組合而成的實體。」--wiki
每個宣告的 function 都會儲存著[[Scope]]，而這個資訊裡面就是參照的環境。
「that all functions, independently from their type: anonymous, named, function expression or function declaration, because of the scope chain mechanism, are closures.

from the theoretical viewpoint: all functions, since all they save at creation variables of a parent context. Even a simple global function, referencing a global variable refers a free variable and therefore, the general scope chain mechanism is used;
from the practical viewpoint: those functions are interesting which:
- continue to exist after their parent context is finished, e.g. inner functions returned from a parent function;
- use free variables.」--[ECMA-262-3 in detail. Chapter 6. Closures.](http://dmitrysoshnikov.com/ecmascript/chapter-6-closures/)

scopeChain 把每一層的 AO 和 VO 記錄下來，而變數就紀錄在AO 或 VO 裡，也因為 return 才把 AO 和 VO 保留下來。
closure 其實就是因為 scopeChain 有 reference 到其他 Execution Context 的 AO(active object) 或是 VO(variable object)，所以在離開之後還是可以存取到上層的變數，如果你是以會記住上層資訊的角度來看 closure，那所有的 function 其實都是 closure。

- **閉包是函式記得並存取 Lexical Scope 語彙範疇的能力，可說是指向特定 scope 的參考，因此當函式是在其宣告的 Lexical Scope 語彙範疇之外執行時也能正常運作**。

- 迴圈與閉包搭配使用時的謬誤與陷阱。
  ```
  for (var i = 1; i <= 5; i++) {
    setTimeout(function timer() {
      console.log(i);
    }, i * 1000 );
  }
  ```
  由於 console.log(i) 中的 i 會存取的範疇是 for 所在的範疇（目前看起來是全域範疇，因為 var 宣告的變數不具區塊範疇的特性），因此當 1 秒、2 秒…5 秒後執行 console.log(i) 時，就會去取 i 的值，而此時 for 迴圈已跑完，i 變成 6，因此就會每隔一秒印出一個「6」。
  解決方法可以利用 IIFE（Immediately Invoked Function Expression）把一個 function 包起來並傳入 i 立即執行，所以迴圈每跑一圈其實就會立刻呼叫一個新的 function，因此就產生了新的作用域。不過在 ES6 裡面有了 block scope 的概念以後，你只要簡單地把迴圈裡面用的 var 改成 let 就行了：因為 let 的特性，所以其實迴圈每跑一圈都會產生一個新的作用域。
  關於此題的其他參考：[for迴圈 setTimeout 結合一些示例](https://codertw.com/%E5%89%8D%E7%AB%AF%E9%96%8B%E7%99%BC/231181/)

- 模組模式可經由**建立一個模組實體來調用內層函式，而內層函式由於具有閉包的特性，因此可存取外層的變數和函式。透過模組模式，可隱藏私密資訊，並選擇對外公開的 API**。
- 利用模組依存性載入器或管理器或 ES6 模組來管理模組。

# Reference

[提升（Hoisting）](https://developer.mozilla.org/zh-TW/docs/Glossary/Hoisting)
[我知道你懂 hoisting，可是你了解到多深？](https://github.com/aszx87410/blog/issues/34#)
[秒懂！JavaSript 執行環境與堆疊](https://medium.com/%E9%AD%94%E9%AC%BC%E8%97%8F%E5%9C%A8%E7%A8%8B%E5%BC%8F%E7%B4%B0%E7%AF%80%E8%A3%A1/%E6%B7%BA%E8%AB%87-javascript-%E5%9F%B7%E8%A1%8C%E7%92%B0%E5%A2%83-2976b3eaf248)
[W3C](https://www.w3schools.com/js/js_scope.asp)
[所有的函式都是閉包：談 JS 中的作用域與 Closure](https://github.com/aszx87410/blog/issues/35)
[你懂 JavaScript 嗎？#15 閉包（Closure）](https://cythilya.github.io/2018/10/22/closure/)
[[2019-10-12] 進階 JavaScript - Closure ](https://github.com/healthyspi/weekly-notes/issues/8)
[閉包（Closure）](https://openhome.cc/Gossip/JavaScript/Closure.html)
> 閉包也會用來作為物件私用（private）的模擬，以及名稱空間的管理等
