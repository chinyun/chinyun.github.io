---
title: JS-ECMAScript
date: 2019-11-17 18:50:17
tags:
---

# ECMAScript
 
1996 年創造 JavaScript 的 NetScape 公司決定將 JavaScript 提交給標準化組織 ECMA，希望讓這種語言成為國際標準。1997 年 ECMAScript 1.0 發佈，規定了 browser 腳本語言的標準。
使用 ECMAScript 這個名稱的原因是：
  1. 商標註冊：JAVA 是 Sun 公司的商標，根據授權協議 JavaScript 只有 NetScape 公司可以使用。
  2. 可表示這個語言的制定者為 ECMA，保證其開放性和中立性。
標準制定者每個月開一次會，委員會供來自任何人的提案，經過多次開會等一個提案足夠成熟，就可以正式進入標準。每年會正式發佈一次，作為當年正式版本。接下來一年期間根據此版本為基礎做微幅變動，直到隔年草案便正式成為新的正式版本。在 2015 年 6 月發佈的就是 ECMAScript2015 又稱為 ES6，由於在上一次是 ES5.1(2009.11)，ES6 包含了較多的新特性，從 ES5.1 之後到 ES6 訂定的標準。
規範的推動主要會經過以下幾個階段：

- Stage 0: strawman——最初想法的提交。
- Stage 1: proposal（提案）——由TC39至少一名成員倡導的正式提案文件，該文件包括API事例。
- Stage 2: draft（草案）——功能規範的初始版本，該版本包含功能規範的兩個實驗實現。
- Stage 3: candidate（候選）——提案規範通過審查並從廠商那裡收集反饋
- Stage 4: finished（完成）——提案準備加入ECMAScript，但是到瀏覽器或者Nodejs中可能需要更長的時間。

以下簡要的依年份條列從 2015 年至今(2019)加入的新功能：

## ES6

- `let` & `const` & block scope 的概念：避免 `var` 宣告時，出現區域變數覆蓋全域變數或在  `for loop` 中循環變數洩露成為全域變數的副作用發生。
- Arrow Function：節省 code line，而且沒有自己的 `this` 值，使用來自外部的 `this`，作為 object method 的時候不需要再 `.bind(this)`。
- Class
- Promise
- mudule
- Template Strings
- 解構賦值 (destructuring)：從數組中獲取值並賦值到變量中，變量的順序與數組中對象順序對應。
- Array Spread operator
- Object.assign()
- Symbol
- Set, Map, weakSet, weakMap
- 函數參數默認值
- 對象屬性簡寫

## ES7

- includes()：可以使用在 string 或 array，檢查有無某元素，返回 true 或 false 
- exponential operator 指數運算符：**

## ES8

- String padding: `padStart()`,`padEnd()` 新增字串長度到指定長度
- can use 'ending commas' in list or calling 函數參數列表結尾允許逗號
- Object.values(), Object.entries()
- Async Await

## ES9

- Object Spread operator
- `.finally()` 在 promise 完成(resovled or rejected)後執行一個指令
- Asynchronous iterators `for await(... of...)` 
  ```
  async function process(array) {
    for await (let i of array) {
      doSomething(i);
    }
  }
  ```

## ES10

ES10 在 2019 年 2 月中推出，在新版的 Nodejs 和 Chrome 已經可以使用。

- `[].flat()`,`[].flatMap()`

- `Object.fromEntries`
  ```
  const map = new Map([ ['foo', 'bar'], ['baz', 42] ]);
  const obj = Object.fromEntries(map);
  console.log(obj); // { foo: "bar", baz: 42 }

  const arr = [ ['0', 'a'], ['1', 'b'], ['2', 'c'] ];
  const obj = Object.fromEntries(arr);
  console.log(obj); // { 0: "a", 1: "b", 2: "c" }
  ```
- `String.trimStart()` and `String.trimEnd()` 減少空白字串

- `try{} catch{}` 可以省略 catch 回傳的表達參數 `catch(err){} -> catch{}`

- revised Function#toString：
  function.toString() 回傳確實的 Function 程式碼，包括空白字元和註解:
  ```
  function exampleFuncton() {
    // Hello, I'm an ordinary function
  }

  console.log(exampleFunction.toString());
  // function exampleFunction() {
  //     // Hello, I'm an ordinary function
  // }
  ```

- Symbol Description：
  在建構 symbol 時把 description 作第一個參數傳入，就可以通過 `toString()` 取得：
  ```
  const symbolExample = Symbol("Symbol description");
  console.log(symbolExample.toString());
  // 'Symbol(Symbol description)'
  ```

- JSON ⊂ ECMAScript
  The unescaped line separator U+2028 and paragraph separator U+2029 characters were not accepted in the pre-ES10 era.

- well-formed JSON.stringify
  `JSON.stringify()` may return characters between U+D800 and U+DFFF as values for which there are no equivalent UTF-8 characters. However, JSON format requires UTF-8 encoding. The proposed solution is to represent unpaired surrogate code points as JSON escape sequences rather than returning them as single UTF-16 code units.

- stable `Array.sort()`
  The previous implementation of V8 used an unstable quick-sort algorithm for arrays containing more than 10 items. A stable sorting algorithm is when two objects with equal keys appear in the same order in the sorted output as they appear in the unsorted input.

- 新增數據資料的基本型別：`BigInt` (stage 3)
  BigInt is the 7th primitive type: an arbitrary precision integer. The variables can now represent 253 numbers and not just max out at 9007199254740992.

- Dynamic import (stage 3)
  Dynamic `import()` returns a promise for the module namespace object of the requested module. Therefore, imports can now be assigned to a variable using async/await.

- Standardized `globalThis` object (stage 3)

-------------------

# Reference

[What's New in ES10? Javascript Features ES2019
February 16, 2019](https://maksimivanov.com/posts/new-es10-es2019-javascript-features/)
[JavaScript: What’s new in ECMAScript 2019 (ES2019)/ES10?](https://medium.com/@selvaganesh93/javascript-whats-new-in-ecmascript-2019-es2019-es10-35210c6e7f4b)
[ES6、ES7、ES8、ES9、ES10新特性一览](https://juejin.im/post/5ca2e1935188254416288eb2)
[Twelve ES10 Features in Twelve Simple Examples](https://medium.com/better-programming/twelve-es10-features-in-twelve-simple-examples-6e8cc109f3d3)
