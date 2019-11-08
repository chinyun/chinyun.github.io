---
title: css pseudo class & pseudo element
date: 2019-11-08 18:39:38
tags:
---

單冒號 (:) 是用在偽類
雙冒號 (::) 則是用在偽元素
偽類 (pseudo class) 就是在選已經存在的東西，比方說 `a:hover` 就是選了已經存在的 `<a>` 的某一個狀態
偽元素 (pseudo element) 就是在創造一個新的假元素，因為他不在 DOM 裡面，而是創造的了一個我們看不見的元素。比如說 `::first-line`，第一行並沒有被任何的 tag 包住，所以在選取的過程就像是用了一個看不到的 tag 把第一行包起來，所以才選得到這行。

使用偽類元素，一方面可以減少頁面中的節點元素，加速頁面渲染速度，另一方面可以為設計動畫提供很多新思維。

# pseudo class

Pseudo-classes are used to provide styles not for elements, but for various states of elements. The pseudo-class concept is introduced to permit selection based on information that lies outside of the document tree or that cannot be expressed using the other *simple selectors*.
用於定義元素的特殊狀態，為了選擇 DOM tree 之外的信息，或者使用其他簡單選擇器不能表達的信息，不會出現在 DOM tree。

Simple selectors includes:
- type selector (`h1, p, div`)
- universal selector (`*`)
- attribute selector (`img[alt]`)
- class selector (`.class`)
- ID selector (`#id`)
- pseudo-class (`:`) 
  - **:active** 
  - **:hover** 設定滑鼠滑過的樣式
  - **:visited** 被訪問過的連結的樣式
  - **:link**
  - **:nth-child()**
  - **:checked**

狀態不存在 DOM 裡面，或是這幾種 simple selectors 選不到的東西，就是 pseudo-class 要去解決的問題。

# pseudo element

Pseudo elements differ from pseudo-classes in that they don’t select states of elements; they select parts of an element. 用於選擇元素的指定部分，創造一個關於 DOM tree 的抽象內容，提供一種方法來引用源文檔中不存在的內容，創造之後會出現在 DOM tree，偽元素也會「繼承」原本元素的屬性。

- **::first-line**：選取第一行
- **::first-letter**：選取第一個字
- **::before**：在原本的元素「之前」加入內容
- **::after**：在原本的元素「之後」加入內容
- **::selection**：選取文字反白後

# Reference

[偽元素 (pseudo element) 和偽類 (pseudo class) 差在哪？](https://stringpiggy.hpd.io/pseudo-element-pseudo-class-difference/)
[css偽類元素的運用以及相應的hover的使用](https://www.itread01.com/content/1549211047.html)
[CSS 偽類 與 偽元素](https://ithelp.ithome.com.tw/articles/10196924)
[使用CSS3 :nth-child(n) 選取器教學](http://csscoke.com/2013/09/21/%E4%BD%BF%E7%94%A8css3-nth-childn-%E9%81%B8%E5%8F%96%E5%99%A8%E8%A9%B3%E8%A7%A3/)
[CSS 偽類 child 和 of-type](https://www.oxxostudio.tw/articles/201405/css-selector.html)
[CSS 偽元素 ( before 與 after )](https://www.oxxostudio.tw/articles/201706/pseudo-element-1.html)
