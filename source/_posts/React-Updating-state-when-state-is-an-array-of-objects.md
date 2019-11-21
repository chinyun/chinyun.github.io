---
title: 'React: Updating state when state is an array of objects'
date: 2019-10-12 19:06:41
tags:
---

當 state 的結構是 array of objects 的時候，更新 state 的方法需要考慮到 data structure 和 react update 機制。

# Immutable Data Structures

由於 react 的 state update 機制是觸發 render function 後會讓 virtual dom 和 real dom 比對只重繪有更新的部分，做出最低必要的更新。
因此每次變動 state 的時候需要這樣寫：
```
this.setState({
  obj: {
    ...this.state.obj,
    id: 2
  }
})
  
this.setState({
  list: [...this.state.arr, 123]
})
```

而不能這樣寫：
```
#wrong
const newObject = this.state.obj
newObject.id = 2;
this.setState({
  obj: newObject
})
  
#wrong
const arr = this.state.arr;
arr.push(123);
this.setState({
  list: arr
})
```

也就是說每次改動 state 的時候，都是產生一個新的物件，而不是直接對現有的 object 進行變更，這就是 Immutable data 的概念。

# 如何正確的更新 state？

由於 JavaScript 的資料結構特性，我們需要用 `Object Spread Operator` 和 `Object.assign({}, defaults, options)` 等方法在 React 中更新 state。

- Objects, Array 深、淺拷貝

由於在 JavaScript 中 Objects, Array 的資料型別在賦值的時候，是用 pass by reference 的方法，與原始型別（primitive type）的直接傳值（pass by value）是不一樣的。

所以我們在複製 Objects, Array 的時候會利用函式處理，而不會直接用等號賦值。
但賦值的時候因為傳值的差異，有 淺拷貝（Shallow copies）和深拷貝（Deep copies）的差異。


淺拷貝: 
Shallow copies duplicate as little as possible. A shallow copy of a collection is a copy of the collection structure, not the elements. With a shallow copy, two collections now share the individual elements.

深拷貝:
Deep copies duplicate everything. A deep copy of a collection is two collections with all of the elements in the original collection duplicated.

對 Shallow copy 來說，只是複製 collection structure，而不是 element。**所以在指派第二層物件時會出現問題。**

而 Deep Copy 是整個複製，包含 element。**所以當我們在使用多層物件時，要用 deep copy。**

- 正確的更新 state

當我的 state 如下：
```
this.state = {
  journeyList: [
    {id: 1, name: "Japan, Tokyo"},
    {id: 2, name: "Japan, Kyoto"},
    {id: 3, name: "Europe 12 DAYS Travel"}
  ]
}
```

Update function would look like this:
```
updateJourney = (journey) => {
    const index = this.state.journeyList.findIndex((item)=> item.id === journey[0].id);
    if (index !== -1) {
      this.setState({
        journeyList: [
          ...this.state.journeyList.slice(0, index),
           Object.assign({}, this.state.journeyList[index], journey[0]),
           ...this.state.journeyList.slice(index + 1)
        ]
      });
    }
  };

```

若 journey 為：

```
[{id: 4, name: 'Singapore 7 days trip'}]

```

則我的 state 就會變成：
```
this.state = {
  journeyList: [
    {id: 1, name: "Japan, Tokyo"},
    {id: 2, name: "Japan, Kyoto"},
    {id: 3, name: "Europe 12 DAYS Travel"},
    {id: 4, name: 'Singapore 7 days trip'}
  ]
}
```

# Use Redux to update state

上述寫法當開發規模拓展的時候，每一次操作都需要深拷貝一次 state 並 assign 內容， code 維護上會較困難，因此可以考慮使用 Redux 的 state update pattern。

Redux uses a concept of state reducers which each work on a specific slice of the state of your application. That way you don't have to manually dig through your entire state each time you want to affect a deep change.

相關文章可以參考：[React State Management & Redux](https://chinyun.github.io/myblog/2019/10/09/React-State-Management-Redux/)

# Reference

[StackOverflow](https://stackoverflow.com/questions/37662708/react-updating-state-when-state-is-an-array-of-objects)

[React 性能優化大挑戰](https://blog.techbridge.cc/2018/01/05/react-render-optimization/)

[關於JAVASCRIPT中的SHALLOW COPY(淺拷貝)及DEEP COPY(深拷貝)](https://dustinhsiao21.com/2018/01/07/javascript-shallow-copy-and-deep-copy/)

# 延伸閱讀

[JS-Shallow Copy & Deep Copy](https://chinyun.github.io/myblog/2019/11/13/JS-Shallow-Copy-Deep-Copy/)
[JS-pass by value or reference](https://chinyun.github.io/myblog/2019/11/13/JS-pass-by-value-or-reference/)
[React State Management & Redux](https://chinyun.github.io/myblog/2019/10/12/React-Updating-state-when-state-is-an-array-of-objects/)
