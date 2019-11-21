---
title: JS-Data Structure
date: 2019-11-14 21:34:23
tags:
---

# JavaScript 資料結構

資料結構是電腦中儲存、組織資料的方式，在 JavaScript 中，資料可以分做基本型別和物件型別，基本型別儲存一個值，物件型別儲存更複雜的資料，為了更簡便的存取複雜的資料，JavaScript 使用有規則可循的結構來管理資料。

The key distinctions between primitives and objects:
- A primitive
  - Is a value of a primitive type.
  - There are 6 primitive types: string, number, boolean, symbol, null and undefined.
- An object
  - Is capable of storing multiple values as properties.
  - Can be created with {}, for instance: {name: "John", age: 30}. There are other kinds of objects in JavaScript: functions, for example, are objects.
  - One of the best things about objects is that we can store a function as one of its properties.


資料結構：
- index 管理：透過索引(index)數字，一個數字代表一個資料
- key-value-pair：使用 key-value(通常是一個字串)對應到資料

在 Ruby 語言中，使用 index 的叫做 Array，使用 key-value-pair 的叫做 Hash。
而在 JavaScript 中，使用 index 的也叫做 Array，使用 key-value-pair 的有兩種： Object 和 Map。

JavaScript 為這些資料結構內建了一些方法來方便開發者使用、管理資料:

## JavaScrip Objects methods list

- Array 陣列
  - 其他資料結構：
    - Stack 堆疊：last-in-first-out，支持 push, pop operation
    - Queue 佇列：first-in-first-out. A Queue is one of most common uses of an array. In computer science, this means an ordered collection of elements which supports two operations: push, shift.
    在 JavaScript 中的 array 可以用作 queue 也可以用作 stack，從陣列的前/後來添加或刪除元素。
  - JavaScrip 陣列內建支援的方法
    - pop()：對 array 從尾刪除元素
    - push()：對 array 從尾添加元素
    - shift()：對 array 從頭刪除元素
    - unshift()：對 array 從頭添加元素
    - concat()：返回一個新數組：複製當前數組的所有成員並向其中添加 items。如果有任何items 是一個數組，那麼就取其元素。不會改變原 array。
    - slice(start, end)：複製並返回切下的部分，不會改變原 array。 
    - splice(index, 刪除幾個, "插入")：返回切下的部分，會改變原 array。
    - split()：複製一組 array，將 string 轉為 array。
    - join()：複製一組 array，將 array 內容轉為 string。
    - sort()：對原 array 做排序
    - reverse()：對原 array 做倒序
    - indexOf(item, from)：從 from index 開始由右至左找 item，返回符合的第一個 index
    - lastIndexOf(item, from)：從 from index 開始由左至右找 item，返回符合的第一個 index
    - includes(item, from)：從 from index 開始由右至左找 item，返回 true/false

- Object Array 物件陣列
  是指陣列元素為 object 或 array 的多層結構資料。針對這種類型的資料，JavaScrip也提供一些有用的方法來協助管理：
  - map(function(item, index, array){ 遍歷每一個元素後，返回新 array })
  - find(function(item, index, array){ 查詢到返回 true，return 第一個符合的元素 })
  - filter(function(item, index, array){ 元素通過過濾器時返回 true，return 符合元素 })
  - reduce(function(accumulator, num){})

- Object
  - Object.assign(target, ...source) 產生一個新的 object 或合併 objects
  - Object.keys(obj) 返回一個由 keys 組成的陣列
  - Object.values(obj) 返回一個由 values 組成的陣列
  - Object.entries(obj) 返回一個 key-value-pair 組成的陣列
  - 自創 methods

- Map 也是 key-value-pair 物件，但是他可以直接被 iterate、 key 可以使用任何資料型別；object的 key 只能是 string。
  - set(key, value): stores the value by the key.
  - get(key): return the value if the key exists, `undefined` if keys don't exist.
  - has(key): return true/false whether the key exists or not
  - delete(key): remove value by the key
  - clear(): removes every thing from the set
  - size: counts the elements
  - keys()
  - values()
  - entries()

- Set is a collection of values, where each value may occur only once.
  - add(value): add a value, return set itself.
  - delete(value): removes the value, return true/false.
  - has(value): return true/false whether the value exists or not.
  - clear(): removes every thing from the set
  - size: counts the elements
  - keys()
  - values()
  - entries()

- WeakMap & WeakSet
  通常情況下，當某數據存在於內存中時，對象的屬性或者數組的元素或其他的數據結構將被認為是可以獲取的並留存於內存。在一個正常 Map 中，我們將某對象存儲為鍵還是值並不重要。它將會被一直保留在內存中，就算已經沒有指向它的引用。
  - WeakSet
    - WeakSet 是一種特殊的 Set，它不會阻止 JavaScript 將它的元素從內存中移除。
    - 它和 Set 類似，但是我們僅能將對象添加進 WeakSet（不可以是基礎類型）
    - 僅當對象存在其他位置的引用時它才存在於 set 中。
    - 支持 add()、has() 和 delete()，不支持 size、keys() 也不支持迭代器。

  - WeakMap
    - WeakMap 也不會阻止 JavaScript 將它的元素從內存中移除，而且它和 Map 的區別是它的鍵必須是對象，不能是基礎類型的值。
    - 支持 get(key)、set(key, value)、delete(key, value)、has(key)
    - 但不支持 keys()、values()、entries()，我們不能對它進行迭代，所以沒有辦法獲取它的所有鍵值。
    - WeakMap 的目的是，我們可以僅**當該對象存在時，為對象存儲一些內容**。但我們並不會因為存儲了對象的一些內容，就強制對像一直保留在內存中。

## 基本型別的 methods list

There are many things one would want to do with a primitive. So JavaScript provides methods to call. JavaScript allows us to work with primitives (strings, numbers, etc.) as if they were objects. 

The solution is:
The “object wrappers” are different for each primitive type and are called: String, Number, Boolean and Symbol. Thus, they provide different sets of methods.

JavaScript 允許訪問字符串，數字，布爾值和符號的方法和屬性。當進行訪問時，創建一個特殊的“包裝對象”，它提供額外的功能，運行後即被銷毀。除 null 和 undefined 以外的基本類型都提供了許多有用的方法。從形式上講，這些方法通過臨時對象工作，但 JavaScript 引擎可以很好地調整以優化內部，因此調用它們並不需要太高的成本。

- Number
  JavaScript 有一個內置的 Math 對象，它包含了一個小型的數學函數和常量庫。
  - Math.random()
  - Math.max(a, b, c...) / Math.min(a, b, c...)
  - Math.pow(n, power)
  分數可使用(須記住使用分數時會損失精度):
  - Math.floor 向下舍入，3.1 變成 3，-1.1 變成 -2。
  - Math.ceil 向上舍入，3.1 變成 4，-1.1 變成 -1。
  - Math.trunc 刪除小數點後的所有內容而不捨入：3.1 變成 3，-1.1 變成-1，IE 瀏覽器不支援。
  - Math.round 向最近的整數舍入：3.1 變成 3，3.6變成4，-1.1變成-1。
  - num.toFixed(precision) 將點數後的數字四捨五入到 n 個數字並返回結果以字符串表示
  檢查是否為特殊 Number？
  - isNaN
  - isFinite
  檢視字符串並返回 Number：他們從字符串中“讀出”一個數字。如果發生錯誤，則返回收集的數字。有時候 parseInt / parseFloat 會返回 NaN。一般發生在沒有數字可讀的情況下。
  - parseInt(str, radix) 返回整數。第二個參數指定了數字系統的基礎，因此 parseInt 還可以解析十六進制數字，二進制數字等字符串。
  - parseFloat() 可以返回浮點數

- String
  - string.length 字符串長度
  - string[0] 或 string.charAt(0):
    如果沒有找到字符，[] 返回 undefined，而 charAt 返回一個空字符串
  - toLowerCase()
  - toUpperCase()
  - trim() 刪除前後的空格
  - 查找字符的方法：
    - indexOf(substr, pos)
    - includes(substr, pos)
    - endsWith()
    - startsWith()
  - 獲取字符串的方法：
    - slice(start [, end]) 返回從 start 到（但不包括）end 的字符串部分。
    - substring(start [, end]) 返回 start 和 end 之間的字符串部分。這與 slice 幾乎相同，但它允許 start 大於 end。
    - substr(start [, length]) 從 start 開始返回給定 length 的字符串部分。

# Reference

[javascript.info/map-set-weakmap-weakset](https://zh.javascript.info/map-set-weakmap-weakset)
[javascript.info/primitives-methods](https://javascript.info/primitives-methods)
