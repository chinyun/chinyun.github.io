---
title: JS-pass by value or reference
date: 2019-11-13 17:35:56
tags:
---

問題：JavaScript 到底是 pass by value 還是 pass by referece？

根據 JavaScript 的資料型態，可以分為兩大類：基本型別 primitive 和 物件型別 object。
基本型別的資料以 純值的形態存在，例如：number、string、boolean、null、undefined、symbol，而 object 的資料可能為 純值 或 多種不同型別組合而成的 物件。

# 基本型別傳值 pass by value，Object 型別傳址 pass by referece

```
var a = 10;
var b = a;
console.log(a === b); //true

var c = 5;
var d = 5;
console.log(c === d); //true
```
`var b = a;` 表面上看起來變數 b 的內容是透過複製變數 a 而來，實際上變數 b 是去建立了一個新的值，然後將變數 a 的內容複製了一份存放到記憶體， a 和 b 其實是存在於兩個不同的記憶體位置，因此變數 a 和變數 b 彼此獨立互不相干，即使更改 a 的內容， b 的值也不會變：

```
a + 2;
console.log(a); //12
console.log(b); //10
```

基本型別是不可變的 (immutable)，當修改、更新值時，與那個值的副本完全無關，像這種情況，我們通常稱作「傳值」 (pass by value)。

如果是物件型別的資料：

```
var obj1 = { a: 10 };
var obj2 = { a: 10 };
console.log(obj1 === obj2); //false

var obj3 = { b: 20 };
var obj4 = obj3;
console.log(obj3 === obj4); //true
```

「物件」這類資料型態，在 JavaScript 中是透過「引用」的方式傳遞資料的。
當建立起一個新的物件並賦值給一個變數(`var obj3 = { b: 20 };`)的時候，JavaScript 會在記憶體的某處存放這個物件(`{ b: 20 }`)，再將變數(`obj3`)指向這個物件的存放位置，因此當`var obj4 = obj3;`的時候，其實是將 obj4 這個變數也指向了 `{ b: 20 }`這個實體。
這種透過引用的方式來傳遞資料，接收的其實是引用的「參考」而不是值的副本時，
我們通常會稱作「傳址」 (pass by reference)。

# 特殊情況：

```
var coin1 = { value: 10 };

function changeValue(obj) {
  obj = { value: 123 };
}
changeValue(coin1);
console.log(coin1.value);   // ？
```

答案會是 10，因為當coin1指向的資料被做為 **function 的參數**傳入 function 時，即使資料在 function 內部被重新賦值，外部變數的內容都不會被影響。
在這種情況底下， JavaScript 會將 obj 指向一個新建的 object `{ value: 123 }`，不會影響到外部的 coin1 指向的位址的物件內容。

如果不是 重新賦址 而是 修改傳入 的內容：

```
var coin1 = { value: 10 };
function changeValue(obj) {
  obj.value = 123;
}

changeValue(coin1);
console.log(coin1.value);   // 123
```
此時變數 coin1 所指向的資料內容被改變，傳入 function 作為 obj 參數的值的 coin1 在 function 內被修改，改變了 value 的值。

# 垃圾回收 Garbage collection

對於開發者來說，JavaScript 的內存管理是自動的、無形的。我們創建的原始值、對象、函數……這一切都會佔用內存。當某個東西我們不再需要時會發生什麼？ JavaScript 引擎如何發現它、清理它？
JavaScript 中主要的內存管理概念是**可達性 Reachability**，簡言之，可達值是那些以某種方式可訪問或可用的值，它們保證存儲在內存中。
這裡列出固有的可達值基本集合，這些值明顯不能被釋放：

1. 當前函數的局部變數和參數。Local variables and parameters of the current function.
2. 嵌套調用時，當前調用鏈上所有函數的變量與參數。Variables and parameters for other functions on the current chain of nested calls.
3. 全局變數。Global variables.
4. 還有一些其他的內部變數。(there are some other, internal ones as well)

這些值被稱作根 root。

如果一個值可以通過 引用 或 引用鏈，從根值訪問到，則認為這個值是 可達的。
比方說，如果局部變量中有一個對象，並且該對象具有引用另一個對象的 property，則該對像被認為是可達的。而且它引用的內容也是可達的。

在 JavaScript 引擎中有一個被稱作垃圾回收器(garbage collector)的東西在後台執行。它監控著所有對象的狀態，並刪除掉那些已經不可達的。

# Reference

[深入探討 JavaScript 中的參數傳遞：call by value 還是 reference？](https://blog.techbridge.cc/2018/06/23/javascript-call-by-value-or-reference/)
[重新認識 JavaScript: Day 05 JavaScript 是「傳值」或「傳址」？](https://ithelp.ithome.com.tw/articles/10191057)
[Garbage collection](https://zh.javascript.info/garbage-collection)
[[筆記] 談談 JavaScript 中 by reference 和 by value 的重要觀念](https://pjchender.blogspot.com/2016/03/javascriptby-referenceby-value.html)
