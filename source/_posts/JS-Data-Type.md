---
title: JS Data Type
date: 2019-11-11 17:50:01
tags:
---

# JavaScript Data Type

JavaScript 的資料類型總共有七種，其中 6 種被稱為 **原始型別 primitive data types**: string, number, boolean, null, undefined, symbol (new in ECMAScript 2015)，以及最後一種 Object 對象資料型別。

  - **number**：number for numbers of any kind: integer or floating-point.
  - **string**：string for strings. A string may have one or more characters, there’s no separate single-character type.
  - **boolean**：boolean for true/false.
  - **null**：null for unknown values – a stand alone type that has a single value null.
  - **indefined**：undefined for unassigned values – a standalone type that has a single value undefined.
  - **object**：object for more complex data structures. 包括 function`function(){}`, array`[]`, object`{key:value}`等不同對象物件資料類型。
  - **symbol**：symbol for unique identifiers.

JavaScript 是弱型別，也能說是動態的程式語言，這代表不必特別宣告變數的型別，程式在運作時，型別會自動轉換，這也代表可以用不同的型別使用同一個變數。
A variable in JavaScript can contain any data. A variable can at one moment be a string and at another be a number:
```
let message = "hello";
message = 123456;
<!-- no error -->
```
Programming languages that allow such things are called “dynamically typed”, meaning that there are data types, but variables are not bound to any of them.

# JavaScript Equals

在 JavaScript 語法中，比較值是否相等有分為一般相等比較和嚴格相等比較兩種：
- Double Equals == 一般相等比較，若等號兩邊的資料類型不同，會轉換為相同類型再比較
- Triple Equals === 嚴格相等比較，若類型不同即為不相等

字串的比較：依字典順序，按字母逐個進行比較
```
alert('Z'>'A') -> true
alert('Bee'>'Be') -> true
alert('Glow'>'Glee') -> true
alert('a'<'A') -> false；在 unicode 中 小寫 a 大於 大寫 A
```
若為不同類型比較會先轉換為數字(number)在判定大小：
```
alert('2'>1) -> ture
alert('01'== 1) -> true
alert('2'>'12') -> true
alert(0 == '') -> true
alert(true == 1) -> true
alert(false == 0) -> true
```
```
let a = 0;
alert(Boolean(a)) -> false
let b = '0';
alert(Boolean(b)) -> true
alert(a == b) -> true
```

# Type Conversions 類型轉換

- String()
```
let value = true;
alert(typeof value);  -> boolean
value = String(value); -> "true"
alert(typeof value); -> string
```

- Number()
  - undefined -> NaN
  - null -> 0
  - true 和 false -> 1 and 0
  - string  
    - “按原樣讀取”字符串，兩端的空白會被忽略。空字符串變成 0。轉換出錯則輸出 NaN。
    - 幾乎所有的算術運算符都將值轉換為數字進行運算，加號 + 是個例外：如果其中一個運算元是字符串，則另一個也會被轉換為字符串。
      `alert( 1 + '2' ); -> '12' `
- Boolean()
  - 0, null, undefined, NaN, ""  -> false
  - 1, 其他值  -> true

- 總結注意點：
  - 當用 Number() 轉換型別成 number 時，須注意 "" 空字符會轉為 0，undefined 會變成 NaN。
  - 用 Boolean() 轉換型別成 boolean 時， 0 為 false；1 為 true，而 null, undefined, NaN 及 "" 都會是 false。

# null, NaN, undefined

- null 表示無、空值，未知的特殊值，typeof(null) -> object，這是一個誤解，因為 typeof 用型別標籤來辨別，null的型別標籤為 000，符合 object 的 type 所以會產生這個結果，這可以說是一種 bug，詳見:[typeof](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/typeof)
- NaN 表示不是一個數字(not a number)，typeof(NaN) -> number
- undefined 表示未被賦值，有宣告但未給予數值、字串等值，typeof(undefined) -> undefined，不等於 not defined，not defined 是沒有宣告系統不認得的意思

# Reference

[javascript.info:type-conversions](https://zh.javascript.info/type-conversions)
[JavaScript 的資料型別與資料結構](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Data_structures)
[型別（Type）](https://cythilya.github.io/2018/10/24/object/)
