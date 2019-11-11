---
title: JS prototype
date: 2019-11-11 21:09:26
tags:
---

筆記 JavaScript 中非常重要的概念：繼承 inheritance、原型 prototype 和原型鍊 prototype chain。

# Object-oriented programming(OOP)

JavaScript 並不像 Java、C++ 這些典型的物件導向語言具有「類別」（class）來區分概念與實體（instance）或天生具有繼承的能力，而 JavaScript 只有「物件」，因此只能利用設計模式來模擬這些功能。在 JavaScript 世界中，到底是怎麼實現物件導向的概念的？
JavaScript 的物件透過原型 (Prototype) 機制相互繼承功能，與典型的物件導向 (OO) 程式語言相較，其運作方式有所差異。
在討論 物件導向 JavaScript 之前，需要先知道物件導向程式設計 (Object-oriented programming；OOP)的意義。
OOP 基本概念是：**採用物件（objects）來模塑真實的實物世界**：也就是在程式中的呈現是透過 objects 來塑造其模型，並提供簡單方式存取其「難以或不可能採用的功能」。物件可裝載相關的資料與程式碼，資料部分是你塑造某個模型的資訊，而程式碼部分則用是操作行為(Method)實現。

為了簡化程式撰寫，我們可以為某個複雜東西建立簡單的模型，藉以代表其最重要的概念或特質，且該模型建立方式極易於搭配我們的程式設計用途：譬如用「類別」建立物件實體 Object instance — 該物件包含了類別中所定義的資料與功能。

在根據類別建立物件實體時，就是執行類別的「建構子 Constructor 函式」所建立，而這個「根據類別來建立物件實體」的過程即稱為「實體化 Instantiation」，物件實體就是從類別實體化而來。

我們可根據某一個類別建立許多新的子類別，新的子類別可繼承 (Inherit) 其母類別的資料與程式碼特性。你可重複使用所有物件類型共有的功能，而不需再複製之。若功能需與類別有所差異，則可直接於其上定義特殊功能。

# inheritance

一個物件可以存取其他物件的屬性 properties、方法 methods，就叫做繼承 inheritance。
繼承可以分成兩種，一種是 classical inheritance 類別繼承，這種方式用在 C# 或 JAVA 當中；另一種則是 JavaScript 所使用的，是屬於 prototypal inheritance 原型繼承。
在「典型 OO」中，你必須定義特定的類別物件，才能定義哪些類別所要繼承的類別，而 JavaScript 使用不同的系統：「繼承」的物件並不會一併複製功能過來，而是透過原型鍊連接其所繼承的功能，亦即所謂的原型繼承 (Prototypal inheritance)。
基於 JavaScript 運作的方式 (如原型鍊等)，物件之間的功能共享一般稱為「委託 (Delegation)」，即特定物件將功能委託至通用物件類型。「委託」其實比繼承更精確一點。因為「所繼承的功能」並不會複製到「進行繼承的物件」之上，卻是保留在通用物件之中。

# prototype

Object 之間可以互相成為各自的 Prototype，被繼承的 Object 將會繼承父 Object 的 Prototype 所有屬性。[[Prototype]]，是一個設定（寫入） Object 的 Prototype 的接口，是一個內部屬性(internal property)，它并不允許外部存取。

每個實例對象（object）都有一個私有屬性（稱之為 `__proto__`）指向它的構造函數的原型對象（prototype）。該原型對像也有一個自己的原型對象(`__proto__`) ，層層向上直到一個對象的原型對象為 `null`。根據定義，`null` 沒有原型，並作為這個原型鏈中的最後一個環節。

幾乎所有 JavaScript 中的對象都是位於原型鏈頂端的 Object 的實例。

prototype 這個屬性，是 JavaScript 中最讓人混淆的名稱之一。你可能會認為`this`屬性即指目前物件(current object)的原型物件(prototype object)，但它其實不是原型 (應該是可透過 `__proto__`存取的內部物件(internal object)才對)。prototype是一個物件(object)，內含了你定義所應該繼承的成員。

遵循ECMAScript標準，`someObject.[[Prototype]]` 符號是用於指向 someObject 的原型。從 ECMAScript 6 開始，[[Prototype]] 可以通過 `Object.getPrototypeOf()` 和 `Object.setPrototypeOf()` 訪問器來訪問。這個等同於 JavaScript 的非標準但許多瀏覽器實現的屬性`__proto__`。

`__proto__` 發音 dunder prototype，最先被 Firefox使用，後來在 ES6 被列為Javascript 的標準內建屬性的。它的出現是為了解決讀寫 `Object.prototype`的麻煩，提供一個快捷讀寫的 API，而且它是透過連結內部屬性 `[[Prototype]]` 完成這個功能 (by javascripttutorial)。

但它不應該與構造函數 func 的 prototype 屬性相混淆。被構造函數創建的實例對象的 [[prototype]] 指向 func 的 prototype 屬性。 Object.prototype 屬性表示 Object 的原型對象。

舉例：
```
function Person(name, age) {
  this.name = name;
  this.age = age;
}
  
Person.prototype.log = function () {
  console.log(this.name + ', age:' + this.age);
}
  
var nick = new Person('nick', 18);
console.log(nick.__proto__ === Person.prototype) // true

var peter = new Person('peter', 20);
console.log(nick.log === peter.log) // true
```

當我在呼叫nick.log()的時候，JavaScript 是怎麼找到這個 function 的？
因為 nick 這個 instance 本身並沒有 log 這個 function。但根據 JavaScript 的機制，nick 是 Person 的 instance，所以如果在 nick 本身找不到，它會試著從Person.prototype去找。
可是，JavaScript 怎麼知道要到這邊去找？所以一定是 nick 跟 Person.prototype 會透過某種方式連接起來，才知道說要往哪邊去找 log 這個 function。
而這個連接的方式，就是`__proto__`。
（附註：其實比較好的方式是用`Object.getPrototypeOf()`，但這邊為了方便起見，還是使用比較常見的`__proto__`，更詳細的說明可參考：[MDN: Object.prototype.proto](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/proto)）
`nick.__proto__` 會指向 `Person.prototype`，`Person.prototype` 和 `nick.__proto__`是兩個不同的概念：當一個 Person 的物件被創建，被創建物建 nick 的 api 街口 `nick.__proto__` 會被指向到 `Person.prototype` 指向的物建。

假如Person.prototype還是沒找到 log，那就繼續依照這個規則，去看`Person.prototype.__proto__`裡面有沒有 log 這個 method，就這樣一直不斷找下去，直到找到某個東西的`__proto__`是 null 為止，就表示這邊是最上層了，而這一個透過`__proto__`不斷串起來的鍊，就叫做原型鍊。透過原型鍊可以達成類似繼承的功能，呼叫自己 parent class 的 method。

# prototype chain

新的物件實例透過建構子函式 constructor() 產生後，其核心將透過一種稱為原型鏈 Prototype chain 的機制傳遞，由 prototype 定義的參照鏈連在一起。

常有人說 JavaScript 是原型架構的程式語言 — 各個物件均具備 1 組原型物件作為範本物件，用以繼承函式與屬性。物件的原型物件可能也具備原型物件，並繼承了其上的函式與屬性。這就是我們所謂的「原型鍊 (Prototype chain)」，同時正好說明為何 A 物件的屬性與函式是透過 B 物件的屬性與函式所定義。

精確點說，**這些屬性與函式都是透過物件的建構子函式所定義，並非物件實例本身**。

傳統的 OOP 都是先定義了類別，接著在建立物件實例之後，在類型上定義的所有屬性與函式均複製到此實例。但 JavaScript 不會複製這些屬性與函式，卻是在物件實例與其建構子之間設定連結 (原型鍊中的連結)，只要順著原型鍊就能在建構子之中找到屬性與函式。

繼承的成員是在一個物件的 prototype 屬性 (你也能稱之為子命名空間 sub namespace) 中定義的成員，也就是以「`Object.prototype.`」開頭的成員；並非只以「`Object.`」開頭的成員。prototype 屬性值就是 1 個物件，基本上儲存了許多我們想「讓原型鍊上的物件一路繼承下去」的屬性與函式。
在 JavaScript 主控台中輸入 `Object.prototype.` 會看到 Object 的 prototype 屬性中所定義的許多函式，而繼承自 Object 的物件也能找到這些函式。只要試著尋找如 String、Date、Number、Array 等全域物件的原型上定義的函式與屬性，就會看到 JavaScript 中的其他原型鍊繼承範例。

# Reference

[了解 JavaScript 中，繼承 inheritance、原型 prototype 和原型鍊 prototype chain 的概念](https://pjchender.blogspot.com/2016/06/javascriptprototypeprototype.html)
[你懂 JavaScript 嗎？#19 原型（Prototype）](https://cythilya.github.io/2018/10/26/prototype/)
[MDN Web Docs:继承与原型链](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
[該來理解 JavaScript 的原型鍊了](https://blog.techbridge.cc/2017/04/22/javascript-prototype/)
[Javascripter 必須知道的繼承 prototype, [[prototype]], `__proto__`](https://medium.com/@peterchang_82818/javascripter-%E5%BF%85%E9%A0%88%E7%9F%A5%E9%81%93%E7%9A%84%E7%B9%BC%E6%89%BF%E5%9B%A0%E5%AD%90-prototype-prototype-proto-object-class-inheritace-nodejs-%E7%89%A9%E4%BB%B6-%E7%B9%BC%E6%89%BF-54102240a8b4)
