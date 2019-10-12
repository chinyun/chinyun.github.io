---
title: UI variation and dynamic feedback
date: 2019-10-12 14:47:37
tags:
---

# UI/UX 與互動設計

- 靜態的網頁介面與具有互動性的網站不同之處在於，互動能夠有效地引導使用者進行期待的操作。

- 互動設計與 UX 設計關係緊密，UX 關注網頁所規劃的操作流程是否足夠引導使用者表現出預期行為？使用者完成任務的效率是否良好？操作過程是否有挫折感？設計的操作提示，是否足夠引導使用者良好的方式吸收訊息？

- 為網頁設計具有互動性 UI 的可以幫助打造更友善的使用者介面，營造良好的使用者體驗。

在為應用程式做介面設計和互動設計的時候，將為使用者考慮的細節包含進去思考，會發現每一個操作的動作，有它可以設計得更貼心的細微處。
觀察日常生活中經常使用的應用程式，也可以發現在細節處講究的互動設計，這些微互動設計可以稱為 微互動。

根據 微互動 一書的作者 Dan Saffer，他說：
「Microinteractions are contained product moments that revolve around a single use case — they have one main task. Every time you change a setting, sync your data or devices, set an alarm, pick a password, log in, set a status message, or favorite or “like” something, you are engaging with a microinteraction.」

- 微互動是應用程式、設備等與使用者之間互動過程中，所有細節上的互動。
- 微互動可以幫助使用者更流暢的完成主要功能操作，也能讓使用者感到愉悅，影響使用者會不會繼續使用產品。

UI 基本上由 輸入 -> 內容改變 -> 輸出 這個架構構成，而微互動則是
觸發 Trigger -> 規則 Ruls -> 回饋 Feedback -> 迴圈和模式 Loops & Modes。

「 A Trigger initiates a microinteraction. The Rules determine what happens, while **Feedback lets people know what’s happening.** Loops and Modes determine the meta-rules of the microinteraction. 」-- Reference: MICROINTERACTIONS


# 為什麼需要回饋（feedbacks）

使用者操作時有預期心理，即使底層資料已改變，如果視覺上沒有給予足夠的回饋，會覺得操作行為尚未完成，適當的回饋可以傳遞更完整的訊息給使用者。

「 回饋：呼應微互動的規則。
使用數位裝置時，我們看到或聽到的一切都是一種抽象概念。只有少數使用者清楚軟體或裝置運作背後的原理。比如，我們不是真的把檔案放進資料夾內，電子郵件也不是真的送進收件夾裡，這些都是方便使用者理解互動如何進行的隱喻。任何可視、可聽、可感覺，可以幫助使用者了解系統運作規則，就是回饋。」 -- Reference:[筆記] 微互動 microinteractions


# 如何產生適當的回饋（feedbacks）

「 回饋可以採用多種形式：視覺、聽覺、觸覺（震動），甚至是**動畫**（有許多維度可以變化：時間點、速度、方向等）。重點是讓回饋契合執行中的動作，儘可能以最適當的方式傳達明確的訊息。
回饋可以展現產品的個性。
回饋也可以具備自己的運作規則，比方出現的時機？如何改變顏色？當使用者旋轉平板，畫面如何旋轉？這些規則可以變成回饋本身的微互動，開放使用者手動設定。」 -- Reference:[筆記] 微互動 microinteractions


# CSS Animation & React Render 機制

- 問題
React 在 Render 時會比較 Virtual DOM 與真實 DOM 的差異，沒有改變的地方便不會重新 Render，但是 CSS Animation 的機制是每次節點生成時產生動畫效果，而如果 React Component 以 props 的方式傳遞資料，父元件的 state 更新使子元件的 props 改變，由 props 傳遞的內容也隨之改變，然而節點本身沒有消失再生成，所以並不會產生動畫效果。

- 解決方式
給予 Html element 特定的 Key -> 每次 props 更新時，連動 Key 更新，使 react render 機制判斷為新節點，因而重新生成觸動 animation的 element。

for example:
```
render() {
  return (
    <div>
      <p key={this.props.name} className='title'>{this.props.name}</p>
    </div>
```

# Reference

[[筆記] 微互動 microinteractions](http://uirate.net/?p=1667)
[MICROINTERACTIONS](http://microinteractions.com/what-is-a-microinteraction/)
