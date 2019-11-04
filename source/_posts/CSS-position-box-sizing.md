---
title: CSS position & box-sizing
date: 2019-10-31 18:48:51
tags:
---

# CSS position

**static** 預設，只有套用此預設的元素屬於「不會被特別定位」

**relative** 表現與 static 一樣，除非有設定 top, right, bottom, left 等其他屬性，會使元素相對的調整其原本該出現的所在位置，不管「相對定位」過的元素如何在頁面上移動位置或增加多少空間，都不會影響到原本其他元素所在的位置。

**fixed** 相對於瀏覽器視窗來定位，表示即便頁面捲動他還是會固定在相同位置，使用 top, right, bottom, left 來定位，固定元素不會保留它原本在頁面上應有的空間，不會跟其他元素的配置相干擾。

**absolute** 元素的定位是他所屬的上層容器的相對位置，若元素的上層沒有可以被定位的元素，那就會以相對於整個網頁，也就是 `<body>` 的相對位置。當畫面捲動時元素會隨頁面捲動。

# box-sizing
```
-webkit-box-sizing: border-box;
-moz-box-sizing: border-box;
box-sizing: border-box;
```
元素的內距和邊框不會增加元素本身的寬度。

其他屬性值：
**content-box** 預設，長寬值只包含 content，不包括 border 和 padding。
**inherit** 從上層元素繼承

# display
- **none** The element is completely removed.
- **inline** Displays an element as an inline element (like <span>). Any height and width properties will have **no effect**.
- **block** Displays an element as a block element (like <p>). It starts on a new line, and takes up the whole width.
- **inline-block** Displays an element as an inline-level block container. The element itself is formatted as an inline element, but you can apply height and width values.
- **flex** Displays an element as a block-level flex container.
- **table** Let the element behave like a <table> element
- **table-cell** Let the element behave like a <td> element
- **table-row** Let the element behave like a <tr> element

# Reference
https://zh-tw.learnlayout.com/toc.html
https://www.w3schools.com/cssref/pr_class_display.asp
https://www.oxxostudio.tw/articles/201501/css-flexbox.html

