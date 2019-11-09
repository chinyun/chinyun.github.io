---
title: DOM Event
date: 2019-11-09 16:48:30
tags:
---

# DOM (Document Object Model)

W3C(World Wide Web Consortium) 全球資訊網協會訂定了 DOM 的標準，將 DOM 區分為三部分：core DOM、XML DOM、HTML DOM。HTML DOM 是用來取得、修改、新增、刪除 HTML 元素，有了明確的標準不同瀏覽器才能提供一套相同的操作方式給開發者使用。
DOM 的樹狀結構是一種有階層(父子關係)的資料結構，以節點(node)代表一個物件，物件本身具有屬性和方法，上層的節點為父節點(parent)下層節點為子節點(child)，一個父節點可能有很多個子節點，但子節點只會有一個父節點。
在 document 之上其實還有一個 window 物件，它不屬於 DOM 但實作時經常會使用到 window 提供的屬性及方法，例如： setInterval, clearIntervel, confirm, alert, location

# DOM 事件傳遞機制

事件在 DOM 裡面有既定的傳遞順序，假設你有一個 ul 元素，底下有很多 li ，代表不同的 item，當你點擊任何一個 li 的時候，其實也點擊了 ul，因為 ul 把所有的 li 都包住了。假如我在兩個元素上面都加了eventListener，哪一個會先執行？這時候知道事件的執行順序就很重要。

DOM 事件觸發有三個階段，例如：當點擊事件發生時，會先從 window 開始往下傳遞，一直傳到被點擊的該物件為止，到這邊就叫做 CAPTURING_PHASE 捕獲階段，接著事件傳遞到物件本身，這時候叫做 AT_TARGET，最後事件會從物件一路傳回去 window，這時候叫做 BUBBLING_PHASE 冒泡階段。

- 先捕獲(capturing)再冒泡(bubbling)，當事件傳到 target 本身，沒有分捕獲跟冒泡:
  一般而言是先捕獲再冒泡，但是當事件傳遞到點擊的真正對象，也就是 event.target 的時候，無論你使用 addEventListener 的第三個參數是 true 還是 false，這邊的 event.eventPhase 都會變成 AT_TARGET，既然這邊已經變成 AT_TARGET ，自然就沒有什麼捕獲跟冒泡之分，所以執行順序就會根據 addEventListener 的順序而定，先添加的先執行，後添加的後執行。

- 事件註冊，加上監聽機制的方法：`target.addEventListener(type, listener[, useCapture]);`
  addEventListener 函數的第三個參數是 boolean 值，可以決定事先採取被觸發事件的動作反應，還是其外部元素的觸發動作。設定為 true 代表把這個 listener 添加到捕獲階段，false 代表把這個 listener 添加到冒泡階段，預設為false、在冒泡階段 listen 事件觸發。一個 element可以有多個 eventlistener，監聽不同事件(event)，設置指定動作(function)。

# stopPropagation & preventDefault 

- stopPropagation：取消事件繼續往下傳遞。加在哪邊，事件的傳遞就斷在哪裡，不會繼續往下傳遞給下一個節點。
  用法如下：
  ```
  $list.addEventListener('click', (e) => {
    console.log('list capturing');
    e.stopPropagation();
  }, true)
  ```
  儘管已經用e.stopPropagation，但對於同一個層級，剩下的 listener 還是會被執行到，若是你想要讓其他同一層級的 listener 也不要被執行，可以改用 e.stopImmediatePropagation();

- preventDefault：取消瀏覽器的預設行為。最常見的例子是阻止點擊`<a>`時新開分頁、跳轉或`<form>` submit action。
  preventDefault 跟事件傳遞「一點關係都沒有」，事件還是會繼續往下傳遞。有一個特別值得注意的地方是 W3C 的文件裡面有寫到：Once preventDefault has been called it will remain in effect throughout the remainder of the event’s propagation. 意思就是說一旦 call 了 preventDefault，在之後傳遞下去的事件裡面也會有效果。
  用法舉例：
  ```
  form.addEventListener('submit',e=>{
    e.preventDefault(); // 把原生的 Form submit 跳轉行為停掉
  })
  ```

# Event Delegation

事件代理機制是將許多事件綁定在同一節點，透過該節點使下層的子節點都可以觸發事件，因為只要綁定一個地方不需要每個子節點或後來新增的節點都綁定所以叫做事件代理。

# Reference

[事件 (Event) 的註冊、觸發與傳遞](https://medium.com/@realdennis/javascript-%E4%BA%8B%E4%BB%B6-event-da8104c5c98c)
[DOM 的事件傳遞機制：捕獲與冒泡](https://blog.techbridge.cc/2017/07/15/javascript-event-propagation/)
