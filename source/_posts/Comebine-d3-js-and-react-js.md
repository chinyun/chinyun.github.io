---
title: Comebine d3.js and react.js
date: 2019-10-08 16:09:54
tags:
---

# Why use D3
D3 is a JavaScript library for visualizing data with HTML, SVG, and CSS.
D3 是一個利用資料集來操作文件的 JavaScript 函式庫，可以讓你繪製出用 HTML、SVG 及 CSS 做出來的多元化動態資料圖形，D3 遵照著網頁標準，因此現今的瀏覽器皆可直接支援瀏覽，不需要另外安裝任何框架或是套件，D3 很強大的一個功能就是資料視覺化和資料驅動(Data-Driven) 的 DOM 操作。
以資料驅動（ Data-Driven ）的方式在網頁上動態地操作 DOM，可以更有效率的處理巨量資料。

D3 特色包括：

1. Flexibility：可以創造特殊圖形，語法彈性大
2. Elegance：流暢的動態轉場效果，簡潔的程式碼
3. Community：有龐大使用者社群支援，有許多開放資源

# How do D3 work

- Use D3 Select to grab hold of elements on the screen.
- Use D3 Append to add SVGs onto the selections.
- Use D3 Attr to set attributes of SVGs to make them appear on the screen.
- Use method chaining to write cleaner code.
- D3 is read in the data as an array of JSON objects. No matter the data comes from .TSV, .CSV or .JSON file.
- The attributes of SVGs are actually coming from some data.
- D3 gives us the power to selectively edit the SVGs onthe screen depending on what each data point is saying.

## Data Binding
D3 的 Data Binding 機制，讓資料和 DOM 節點形成一對一的對應關係；當資料改變時， D3 會去更新相對應的 DOM 節點。

`d3.select()`&`d3.selectAll()`：選取 DOM 節點。
`d3.data()`：傳遞資料，將每一筆資料綁定一個 DOM 節點，當資料更新時， D3 會知道這筆資料對應的 DOM 節點。


## General Update Pattern
D3 透過 enter() 和 exit() 來動態控制網頁上元素：

- `d3.enter()`：比對網頁上元素，篩選出尚未綁定資料的節點，再用 `.append()` 新增。
- `d3.exit()`：比對網頁上元素，篩選出不存在相對應資料的節點，再用`.remove()` 移除。

更新資料的的情形包括：

1. JOIN new data with old elements
2. EXIT old elements not present in new data
3. UPDATE old elements present in new data.
4. ENTER new elements present in new data.

# How to combine D3 and React

### 呈現 UI 的邏輯不同

- React：透過 State 改變觸發 Render()，根據最新的 State 重繪 UI
- D3：透過觸發 Update Function 來載入新 Data ，根據 Data 產生 UI 

如果想要在 React 元件中寫 D3 視覺圖表，就需要思考如何將兩者結合，使資料更新時，
能夠同時在畫面上動態呈現視覺圖表變化。

### 分工合作，做各自擅長的

React 負責 DOM Render，D3 負責 SVG 繪圖：
- 運用 React Refs 屬性操作 DOM 節點
- 當資料更新時由 React 重繪 UI：Lifecycle Method（componentDidMount、 componentDidUpdate）
- 將 SVG 繪圖交給 D3，負責 attribute & style 

Assign the reference of the DOM element to a component property.
```
# in the react component
...
render() {
  const paths = this.props.data.map(d => <path key={d.name}/>);
  return (
    ...
    <div>
      <svg ref={el => this.svgEl = el}>
        {paths}
      </svg>
    </div>
    ...
  )
}

```

將 D3 Update Function 寫成 React Component 的元件 method
```
updateStyleAndAttrs = () => {
  ...
  d3.select(this.svgEl)
    .selectAll('path')
    ...
  ...
}
```

在元件生成完成後和元件更新（State 變更）完成時觸發 D3 Update Function
```
componentDidMount = () => {
  this.updateStyleAndAttrs();
}

componentDidUpdate = () => {
  this.updateStyleAndAttrs();
}
```

# Reference

[D3 Docs](https://github.com/d3/d3/wiki/TW-Home)
[D3 Gallery](https://github.com/d3/d3/wiki/Gallery)
[D3.js Quick Start Guide](https://reurl.cc/k5EY23)
[Codepen](https://codepen.io/thecraftycoderpdx/pen/jZyzKo)
[Shubo's Notes](http://shubo.io/react-d3/)
