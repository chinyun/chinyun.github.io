---
title: React lifecycle and state
date: 2019-10-12 20:15:41
tags:
---

# React lifecycle method & state updating 


# 提升 React 效能：保持 virtual DOM 的一致

- DOM (Document Object Model)是什麼？

DOM 文件物件模型，是 Html, XML, SVG 等文件的程式介面，提供一種文件樹的結構化表示方法，可以讓程式存取並改變文件架構、風格（style）和內容，透過 DOM 結構，文件擁有屬性、函式節點，物件可以和事件處理程序連結，當事件被觸發，執行處理程序來存取或改變文件。

- React 如何改變 DOM 以及如何提升效能？

由於讀取更新 DOM 損耗 browser 的執行率，每一次改變便 render 整個頁面在 JavaScript 是很慢的，React 使用 Virtual DOM 虛擬 DOM 的一個強大的 Render 系統，只需更新 DOM 而不需要從 DOM 讀取，以減少運算。
同時， React 的 diff 演算法，當 render function running 得時候，讓 virtual dom 和 real dom 比對只重繪有更新的部分，做出最低必要的更新。

官方文件：
When you use React, at a single point in time you can think of the render() function as creating a tree of React elements. On the next state or props update, that render() function will return a different tree of React elements. React then needs to figure out how to efficiently update the UI to match the most recent tree.

- 開發者提升 React 應用程式的方法

盡可能的減少讀取和更新 DOM
We must minimize the amount of work that we do to the DOM.
