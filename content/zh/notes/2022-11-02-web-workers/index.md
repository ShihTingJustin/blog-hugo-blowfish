---
title: "Web Workers"
summary: "Web Workers 是 JavaScript 的多執行緒解決方案"
categories: ["note"]
tags: ["JavaScript", "Web Workers", "Multi-Threads", "多執行緒"]
#externalUrl: ""
# showSummary: true
date: 2022-11-02
draft: false
---

<img 
src="https://darkthread.github.io/js-worker/js-benchmark.gif" />

左邊的 setTimeout 版本，質數計算期間動晝完全凍結，算完才繼續，且因執行間隔錯亂，粒子原本應隨機亂跑，一度出現整群同步移動。而 Web Wroker 版，全程動畫順暢未受干擾，證明質數計算是用另一個執行緒在跑，不中斷網頁的 JavaScript 執行，真正實踐了多執行緒。

最後再補充另一個實驗，瀏覽器本身是多執行緒環境，受單一執行緒限制的是網頁的 JavaScript 程式，其他如 Render、CSS 等運算等作業，瀏覽器會安排不同執行緒處理。因此，如果今天是用純 CSS 製作的動晝(我找到一個雪花飄效果當範例)，用 setTimeout 或 Web Worker 的差異不大。(註：測量結果未包含一秒延遲，故比之前少一秒)

### Reference

[MDN](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API)

[阮一峰的网络日志 Web Worker 使用教程](https://www.ruanyifeng.com/blog/2018/07/web-worker.html)

[發揮 JavaScript 多執行緒威力 - Web Worker](https://blog.darkthread.net/blog/web-worker/)
