---
title: JS Class
date: 2019-11-11 18:30:54
tags:
---

# JavaScript Class

In Object-oriented programming, a class is an extensible program-code-template for creating objects, providing initial values for state(member variables) and implementations of behavior(member functions or methods).class 是一種可以拓展的編程模板，用來創建對象(object)，提供狀態初始值(變量)和行為實現(函數或方法)。

JavaScript 雖然具有物件導向程式語言的特性，但與 Java、C++ 等以類別為基礎的程式設計規範不同，JavaScript 是一種以原型(prototype-based)為基礎的語言。

由於開發上經常需要創建許多相似類型的對象(object)，例如針對 users、goods等。在 JavaScript 中，可以用構造函數(Constructor)和 operator "new" 來達成這個使用目的，但在 ES6 中有一個更進階的用法叫做 "Class"。

ECMAScript 6 中引入了類別 (class) 作為 JavaScript 現有原型程式(prototype-based)繼承的語法糖。語法糖可以讓現有語法操作變得更容易。類別 (class)語法並不是要引入新的物件導向繼承模型到 JavaScript 中，而是提供一個更簡潔的語法來建立物件和處理繼承。

Class 允許使用更簡潔的結構語法來定義 prototype-based 的類別。在 prototype-based 類別中，所有 methods 都存在 prototype 屬性中，藉以與其他延伸出去的類別共用，如此可以節省 memory 空間。
比較傳統原型繼承的寫法和使用 class 的寫法會發現，class 省略了 prototype chaining 並且整個 class 的宣告定義集中描述在 class 區塊內，幫助讀者了解這段程式的作用範圍，使 class 的目的更為清楚。

- Constructor 構造函數
  - 是一種常規函數，函數名稱首字必須大寫
  - 創建時必須使用 new 來操作
  - 需在 prototype 物件上定義方法
  - 示範：
  ```
  function User(name) {
    this.name = name;
    this.isAdmin = false;
  }

  User.prototype.sayHi = function() {
    console.log(this.name);
  }

  let user = new User("Jack"); // 創建一個 User Constructor

  alert(user.name); -> Jack
  alert(user.isAdmin); -> false
  user.sayHi() -> Jack
  ```
  當一個函數作為 new User(...)執行時，它執行以下步驟：
  1. 一個新的空對像被創建並分配給 this。
  2. 函數體執行。通常它會修改 this，為其添加新的屬性。
  3. 返回 this 的值。
  所以 new User("Jack") 的结果是相同的對象。


- Class
  - 使用 constructor() 建構子來定義初始狀態，而且創建時也必須使用 new 來操作。
  建構子(constructor)方法是一個特別的方法，用來建立和初始化一個類別的物件，一個類別只能有一個建構子(constructor)。
  Class 的 constructor()函數是默認的 method，即使沒有寫出來也會自動生成一個默認的空構造函數，並加入到 class.prototype上。
  class syntax：
  ```
  class User {
    constructor(name) {
      this.name = name;
    }

    sayHi() {
      alert(this.name);
    }
  }
  let user = new User("John");
  user.sayHi();
  ```
  此時 User.prototype 包括 constructor 和 sayHi 兩個 methods。

  - class expression 像一般 function 一樣，也可以寫作沒有名稱的表達式
  ```
  let User = class {
    sayHi() {
      alert("Hello");
    }
  };

  function makeClass(phrase) {
    // 返回並宣告一個 class
    return class {  
      sayHi() {
        alert(phrase);
      };
    };
  }
  let User = makeClass("Hello");

  new User().sayHi(); // Hello
  ```

  - 使用 extend() 以及 super()來繼承父類的參數給子類。
  關鍵字 extends 是在類別宣告或是類別敘述中建立子類別的方法。若在子類別中有建構子(constructor)，要使用this前則必須先呼叫super()函式：在子類別建構子中用關鍵字 super 來呼叫父類別的建構子。
  ```
  const createPersonClass = name => class extends User {
    constructor() {
      super(name)
    }
  }
  ```

  - 內部 method 之間不用逗號區分，與一般物件不同，可以避免我們描述二者時產生混淆
  - class 結構中的 code 都自動開啟了嚴格模式(use strict)
  - class 的宣告不會被 hoisting 提升到最高作用區，與函式宣告式不同，編譯器尚未抵達和執行 class 宣告之前無法建立或存取class
  - Just like literal objects, classes may include getters/setters, computed properties etc.

# Reference

[javascript.info](https://javascript.info/class)
[mozilla.org - MDN Web Docs](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Classes)
