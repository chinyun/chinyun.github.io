---
title: JS-Shallow Copy & Deep Copy
date: 2019-11-13 19:12:02
tags:
---

# 為什麼會有深淺拷貝的差異？

因為 JavaScript 中，基本型別(primitve)與物件型別(object)的值的賦予方式不同，基本型別是 pass by value，物件型別是 pass by sharing。相關介紹可看：[JS-pass by value or reference](https://chinyun.github.io/myblog/2019/11/13/JS-pass-by-value-or-reference/)

因為值賦予方式的差異，在複製物件例如 object, array 等資料類型時，根據拷貝資料的形式，可以分為淺拷貝(shallow copy)及深拷貝(deep copy)。
對淺拷貝來說，只是複製 collection structure，而不是 element，With a shallow copy, two collections now share the individual elements. Collections can be diverse data structures which stores multiple data items.
因此對於基本型別來說，淺拷貝(用等號賦值)會傳值，但對物件型別來說，淺拷貝是傳遞 reference，讓兩者可以共用一個記憶體的物件資料，這樣的話在指派物件型別的資料的第二層或更深層內容時，會同時影響兩個地方。

淺拷貝只複製指向某個物件的指標，而不複製物件本身，新舊物件還是共用同一塊記憶體。
而深拷貝是整個複製，包含element，會另外創造一個一模一樣的物件，新物件跟原物件不共用記憶體，修改新物件不會改到原物件。所以當我們在使用有多層結構的物件資料時，要盡量用深拷貝。
一般物件如果用等號賦值：
```
var obj1 = { a: 10, b: 20, c: 30 };
var obj2 = obj1;
obj2.b = 100;

console.log(obj1); // { a: 10, b: 100, c: 30 } <-- b 被改到了
console.log(obj2); // { a: 10, b: 100, c: 30 }
```

# 如何進行淺拷貝、深拷貝？

一般而言 基本型別的拷貝方法就是用 等號賦值，而物件型別例如 array 或 object等就有很多方式，依照拷貝的層次深度可以分為淺拷貝和深拷貝：如果物件中屬性的值也是物件，只能複製到**第一層**物件的屬性，而無法複製到屬性值的物件(第二層)，就無法達到實際的複製，而是會與舊物件一起共用同一塊記憶體；這樣的複製方法稱為「淺拷貝」。相反地，深拷貝會另外創造一個一模一樣的物件，新物件跟原物件不共用記憶體，修改新物件不會改到原物件。

- 淺拷貝方法
  - **Array.concat**：一般Array.concat的用法是合併兩個陣列
  ```
  var alpha = ['a', 'b', 'c'],
    numeric = [1, 2, 3];
  var alphaNumeric = alpha.concat(numeric);
​  console.log(alphaNumeric); // ['a', 'b', 'c', 1, 2, 3]
  ```
  - **Array.slice**：一般Array.slice()的方法是複製一個新的陣列，可帶入參數 `Array.slice(start, end)`，當不輸入參數值的話會直接複製一個。
  ```
  var animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];
  console.log(animals.slice(2));
  // Array ["camel", "duck", "elephant"]
  ​
  console.log(animals.slice(2, 4));
  // Array ["camel", "duck"]
  ​
  console.log(animals.slice());
  // Array ['ant', 'bison', 'camel', 'duck', 'elephant'];
  ```
  - **手動複製**
  ```
  var obj1 = { a: 10, b: 20, c: 30 };
  var obj2 = { a: obj1.a, b: obj1.b, c: obj1.c };
  obj2.b = 100;

  console.log(obj1); // { a: 10, b: 20, c: 30 } <-- 沒被改到
  console.log(obj2); // { a: 10, b: 100, c: 30 }
  ```
  - **Object.assign**：用來合併物件，用法為 `Object.assign(target, ...source)`若目標物件為空物件則可視為複製一個source的物件。
  `Object.assign({}, obj1)`的意思是先建立一個空物件{}，接著把obj1中所有的屬性複製過去，因為Object.assign跟我們手動複製的效果相同，所以一樣**只能處理深度只有一層的物件**，沒辦法做到真正的 Deep Copy，不過如果要複製的物件只有一層的話可以使用他。

  - **展開運算子(Spread Operator)** 也只能實現一層的拷貝
  ```
  let obj = {name:'john', age:{child: 18}}
  let copy = {...obj};

  copy.name = 'mike';
  copy.age.child = 99;

  console.log(obj);  //{name:"john", age:{child: 99}}
  console.log(copy); //{name:"mike", age:{child: 99}}
  ```

- 深拷貝方法
  - **JSON.parse(JSON.stringify(object_array))**:
    - JSON.parse():把字串轉成物件
    - JSON.stringify():把物件轉成字串
  ```
  let obj1 = { a:{b:10} };
  let obj2_string = JSON.stringify(obj1);
  console.log(obj2_string); //"{"a":{"b":10}}";
​
  let obj2 = JSON.parse(obj2_string);
  console.log(obj2); //{a:{b:10}}
​
  obj2.a.b = 20;
  console.log(obj1); //{a:{b:10}}
  console.log(obj2); //{a:{b:20}}
  ``` 
  只有可以轉成JSON格式的物件才可以這樣用，像 function、Set、Map..等型態就沒辦法轉成 JSON。

  - **jQuery `$.extend`**
  ```
  var $ = require('jquery');

  var obj1 = {
    a: 1,
    b: { f: { g: 1 } },
    c: [1, 2, 3]
  };

  var obj2 = $.extend(true, {}, obj1);
  console.log(obj1.b.f === obj2.b.f); // false
  ```
  - **lodash `_.cloneDeep`**
  ```
  var _ = require('lodash');
  var obj1 = {
    a: 1,
    b: { f: { g: 1 } },
    c: [1, 2, 3]
  };

  var obj2 = _.cloneDeep(obj1);
  console.log(obj1.b.f === obj2.b.f); // false
  ```
  - **自己寫**
  例如下面這個在 react app 裡面用 slice() 和 Object.assign(target, ...sources) 來改變深層結構資料的方法：
  [GitHub Repo:Tripper-app](https://github.com/chinyun/Tripper-app/blob/master/src/containers/App.js)
  ```
  updateBudgets = (journey, journeyId) => {
    const index = this.state.journeyList.findIndex(item => item.id === journeyId);
    if (index !== -1) {
      this.setState({
        journeys: [
          ...this.state.journeys.slice(0, index),
          Object.assign({}, this.state.journeys[index], journey[0]),
          ...this.state.journeys.slice(index + 1)
        ]
      })
    }
  };
  ```

# Reference

[關於JAVASCRIPT中的SHALLOW COPY(淺拷貝)及DEEP COPY(深拷貝)](https://dustinhsiao21.com/2018/01/07/javascript-shallow-copy-and-deep-copy/)
[[Javascript] 關於 JS 中的淺拷貝和深拷貝](https://larry850806.github.io/2016/09/20/shallow-vs-deep-copy/)
