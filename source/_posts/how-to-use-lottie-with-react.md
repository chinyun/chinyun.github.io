---
title: How to use lottie with React?
date: 2020-04-26 15:01:00
tags:
---

最近開始幫公司重新設計官方網站，同時也負責開發，觀察近期網站 UI 趨勢，Micro Interactions & Animated Illustrations 成為現代網頁的特色，而且更能夠吸引瀏覽者的眼球，於是在規劃網站的時候打算使用由 Airbnb 開源的 [lottie-web](https://github.com/airbnb/lottie-web) Library 來產生 svg 動畫， svg 不但可以節省網路資源也比 gif 看起來更順暢。


# 使用 Adobe illustrator 和 Adobe Effect 製作動畫 

首先我用 illustrator 繪製好基礎向量素材，再 import 進 After Effect 去製作動畫效果，並不斷調整讓它看起來更順暢。
將 illustrator 檔案 import 進去 After Effect 的時候，有幾種方式，可以參考這個連結：[A Guide to Importing Adobe Illustrator Files into After Effects](https://www.schoolofmotion.com/blog/import-adobe-illustrator-files-into-after-effects)
但因為 Bodymovin 無法轉出 AI (.ai) 的圖層，會變成 missing image and vector ，所以必須將 AI (.ai) 圖層轉換成 shape layer，方法是選取要轉換的圖層，然後到 **Layer > Create > Create Shapes from Vector Layer**，他就會產生一個新的 shape layer。
-[Learn how to convert Illustrator layers into shape layers in a composition.](https://helpx.adobe.com/tw/after-effects/how-to/convert-illustrator-layers-to-shape-layers.html)

BTW 如果不使用 illustrator 畫，直接在 AE 裡面製作 shape layer 就沒有這個困擾了。

另外，版本與系統的相符也是一個需要注意的地方，這次在使用 AE 之前先花了一段時間升級 mac 系統 和重新安裝 AE XD

目前在 After Effect 中使用 Bodymovin 時需要注意 After Effect 版本須在 15.0 以上，如果使用 mac 系統的話目前至少 要 macOS versions 10.12 (Sierra), 10.13 (High Sierra), 10.14 (Mojave)等版本之系統才支援，並需要在 After Effect 的 Preference 設定中打勾這項：**Allow Scripts To Write Files And Access Network option** 否則 Bodymovin 無法運作。
- Bodymovin 官方網站有標示支援的 After Effect 版本：[Bodymovin Adobe exchange](https://exchange.adobe.com/creativecloud.details.12557.bodymovin.html)

To allow scripts to write files and communicate over a network, choose Edit > Preferences > General (Windows) or After Effects > Preferences > General (Mac OS), and select the Allow Scripts To Write Files And Access Network option.
- [Adobe After Effect](https://helpx.adobe.com/after-effects/using/scripts.html)

在 After Effect 2019 以後的版本，該選項被移到 scrtiping & expressions 的 pannel 裡了，可以參考以下連結說明：
The following existing preferences related to scripting and expressions have been moved from the General preferences panel to the new Scripting & Expressions panel.
-[adobe support community](https://community.adobe.com/t5/after-effects/can-t-see-quot-allow-scripts-to-write-file-and-access-network-quot-in-preferences/td-p/10421465?page=1)

# 透過 plug-in 'Bodymovin' 輸出動畫 JSON 檔，在專案中引用 lottie library 使動畫運行在 web 頁面上

把動畫修改的差不多後就可以透過 Bodymovin 來輸出 JSON 檔案，來運用到專案裡實際跑在 Browser 中看看，我用以下方法將 Bodymovin 轉出的 JSON 檔運用到專案中：

1. 開啟 Bodymovin (Navigate to Windows -> Extensions -> Bodymovin)，選好 export 設定與指定路徑資料夾
關於如何設定 Bodymovin：[How to Use Lottie ? | After Effects Plug-in ‘Bodymovin’](https://medium.com/@chenclaire/airbnb-lottie-after-effects-plug-in-bodymovin-d5473e4ab7cd)
2. 可以打開 demo.html 在瀏覽器中先看看 demo 是否沒有問題
3. 在專案資料夾路徑底下執行： `npm install lottie-web` 安裝運行動畫的 js script
4. 在動畫運行的 js 檔中（ex: app.js）`import lottie from 'lottie-web'`
5. 並將 json 檔也 import 進來： `import animationData from '../lottie/data.json'`
6. call `lottie.loadAnimation()` to start an animation
for example:
```
let animObj = lottie.loadAnimation({
  container: this.animBox, // the dom element that will contain the animation
  renderer: 'svg',
  loop: true,
  autoplay: true,
  animationData: animationData  //the path to the animation json
})
```
7. 在 react JSX 的 html 描述部分用 react 的 ref 屬性指定要產生動畫的 dom element：
```
<div style={{width: 300, margin: '0 auto'}} ref={ref => this.animBox = ref}></div>
```

關於除了用 npm install 來安裝 package 的使用方法，airbnb 的官方 github 有詳細的文件說明其他使用方法，例如 CDN 或透過 Bodymovin getPlayer 來取得 js script 後再引用到專案中，可以參考以下連結：
[lottie github documentation](https://github.com/airbnb/lottie-web)

# Reference
[Lottie animations for Web 2019](https://josephkhan.me/lottie-web/)
[How to export an animation with Bodymovin 2017](https://www.youtube.com/watch?v=5XMUJdjI0L8)

