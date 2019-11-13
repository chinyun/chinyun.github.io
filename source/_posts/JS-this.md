---
title: JS this
date: 2019-11-11 23:25:13
tags:
---

# this

在物件導向程式語言裡面，this 概念指的是 instance 本身。
舉例：
```
class Car {
  setName(name) {
    this.name = name
  }
  
  getName() {
    return this.name
  }
}
  
const myCar = new Car()
myCar.setName('hello')
console.log(myCar.getName()) // hello
```

然而和一般物件導向的程式語言 Java 或 C++ 等不同，在 JavaScript 裡面，你在任何地方都可以存取到 this，所以在 JavaScript 裡的 this 跟其他程式語言慣用的那個 this 有了差異，這就是為什麼 this 難懂的原因。

```
function hello(){
  console.log(this)
}
  
hello()
```

一旦脫離了物件導向，也就是在 class 外面的 this，其實沒有太大的意義。
在這種很沒意義的情況下，this 的值在瀏覽器底下就會是 `window`，在 node.js 底下會是 `global`，如果是在嚴格模式，this 的值就會是 `undefined`。這個規則就是所謂的「預設綁定」。

## this 值的改變

- 可以用 call、apply 與 bind 改變 this 的值：
```
'use strict';
function hello(a, b) {
  console.log(this, a, b)
}
  
hello.call('yo', 1, 2) // yo 1 2
hello.apply('hihihi', [1, 2]) // hihihi 1 2
```
call 跟 apply 的差別就是 apply 在傳參數時要用 array 包起來。

```
class Car {
  hello() {
    console.log(this)
  }
}
  
const myCar = new Car()
myCar.hello() // myCar instance
myCar.hello.call('yaaaa') // yaaaa
```
可以把原本的 this 值覆蓋掉。

```
'use strict';
function hello() {
  console.log(this)
}
  
const myHello = hello.bind('my')
myHello() // my
myHello.call('call') // my
```
使用 bind 之後，call 方法也沒有辦法覆蓋掉。
如果是在**非嚴格模式**底下，無論是用 call、apply 還是 bind，你傳進去的如果是 primitive 都會被轉成 object：
```
function hello() {
  console.log(this)
}
  
hello.call(123) // [Number: 123]
const myHello = hello.bind('my')
myHello() // [String: 'my']
```

- this 是在運行時求值的，可以適用於任何 function，從不同 object 調用同一個 function 可以會有不同 this 的值。

```
const obj = {
  value: 1,
  hello: function() {
    console.log(this.value)
  }
}
  
obj.hello() // 1
```

this 的值跟作用域跟程式碼的位置在哪裡完全無關，只跟「你如何呼叫」有關。

作用域的概念舉例：
```
var a = 10
function test(){
  console.log(a)
}
  
const obj = {
  a: 'ojb',
  hello: function() {
    test() // 10
  },
  hello2: function() {
    var a = 200
    test() // 10
  }
}
  
test() // 10
obj.hello()  
obj.hello2()  
```
無論我在哪裡，無論我怎麼呼叫test這個 function，他印出來的 a 永遠都會是全域變數的那個 a(// 10)，因為作用域就是這樣運作，test 在自己的作用域裡面找不到 a 於是往上一層找，而上一層就是 global scope，這跟你在哪裡呼叫 test 一點關係都沒有。test 這個 function 在宣告的時候就把 scope 給決定好了。

但 this 卻是完全相反，this 的值會根據你怎麼呼叫它而變得不一樣，例如使用 call、apply 跟 bind 可以用不同的方式去呼叫改變 this 的值。如果 function 是在物件下調用，那麼 this 則會指向此物件，無論 function 是在哪裡宣告。使用物件的方法調用時 this 會指向調用的物件。**宣告的位置不重要，重要的是呼叫的方法。**

## reference

[JavaScript 的 this 到底是誰？](https://wcc723.github.io/javascript/2017/12/12/javascript-this/)
[淺談 JavaScript 頭號難題 this：絕對不完整，但保證好懂](https://blog.techbridge.cc/2019/02/23/javascript-this/)

## 補充:在 react 中的 bind(this)用法

[what is the usage of : this.method.bind(this)](https://stackoverflow.com/questions/42434232/what-is-the-usage-of-this-method-bindthis)

```
import React from 'react';

class App extends React.Component {
   constructor() {
      super();
      this.state = {
         data: []
      }
      this.setStateHandler = this.setStateHandler.bind(this);
   };

   setStateHandler() {
      var item = "setState..."
      var myArray = this.state.data;
      myArray.push(item)
      this.setState({data: myArray})
   };

   render() {
      return (
         <div>
            <button onClick = {this.setStateHandler}>SET STATE</button>
            <h4>State Array: {this.state.data}</h4>
         </div>
      );
   }
}

export default App;
```
`this.setStateHandler().bind(this)` sets the context for the function `setStateHandler()` to be the class object. This is necessary so that you could call `this.setState({...})` inside the method, because `setState()` is the method of React.Component. If you do not `.bind(this)` you would get an error that `setState()` method is undefined.

[React 與 bind this 的一些心得](https://medium.com/reactmaker/react-%E8%88%87-bind-this-%E7%9A%84%E4%B8%80%E4%BA%9B%E5%BF%83%E5%BE%97-323c8d3d395d)
當使用 extend React.Component 的方式去宣告元件的時候，React 確實會綁定 this 到元件內，但是卻有以下特定的地方才會被綁進去生命周期函式，例如 componentDidMount 等等
render 內其他自己定義的 property 就不會被綁入 this ，而且 this 會被指到 windows 這個全域上。
