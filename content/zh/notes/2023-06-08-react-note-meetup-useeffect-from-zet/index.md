---
title: "Reactjs.tw Meetup #14 | 都 2022 年了你可能還是不懂 useEffect | Zet"
summary: "Reactjs.tw Meetup #14 筆記"
categories: ["note"]
tags: ["React", "useEffect", "React Hooks"]
#externalUrl: ""
# showSummary: true
date: 2023-06-08
draft: false
---

https://www.youtube.com/watch?v=v9M20STEjgc
https://slides.com/tz5514/useeffect-guide

## useState

> 在 React，畫面是資料延伸的結果
> eg. event handler

每一次 render 都有自己的 event handlers

- 在每一次 render 之間的 props & state 都是獨立、不互相影響的
- 在每一次 render 中的 props & state 永遠都會保持不變，例如該次函式執行的常數
- event handlers 是以原始資料 (props & state) 延伸出來的另一種資料結果
  - 因此，每一次 render 都有自己的 event handlers

## useEffect

### 重要觀念

> 每一次 render 都有自己的 effects

> effect 是 render 結果的副產物，每個 effect 都只屬於特定一次的 render
>
> 當依賴的值從外部發生 mutate 時，closure 是不直覺，難以預測結果的。
> 當依賴的值，永遠不變時，closures 是直覺易懂的，因為他依賴的都是常數，執行的行為效果永遠固定

### Cleanup function

A cleanup function 會在 B 之前先執行，在執行 B

> 單向資料流，資料改變時，結果才會跟著改變

### 宣告式的同步化，而非生命週期

不關心過程跟方法，只關心目標與結果
中間跑了一百次也無所謂，只要 input 固定，output 是對應結果就好

FC 中是 mount 還是 update 其實不重要，對 FC 來說是同一件事 (CC 中 didMount 跟 didUpdate 不同)

### 用途

- 根據目前的 props & state 來同步 React elements 以外的東西，並且避免阻塞 UI 畫面的渲染
- 理想上 effect 無論跟著 render 執行了幾次，程式都應該保持同步且正常運作

是為了同步資料到畫面以外的地方

### 為什麼 effect & cleanup 要在每次 render 後都執行

避免受到上一次 render 的結果影響
不管執行幾次 都完美運行

### dependencies 是一種效能最佳化

為避免重複執行 effect
CC 要在 didUpdate 判斷 dependencies 是否相同
FC 不需要

把會改變的值放到 dependencies，如果沒改變，就會在 render 時跳過這個 effect

> dependencies 是「同步動作的資料依賴清單」，如果這個清單中記載的所有依賴，都跟上一次 render 時沒有差異，就代表沒有再次進行同步的需要，可以略過本次 effect 來節省效能。

### useEffect 的核心思考模型整理

- FC 沒有生命週期 API，只有 useEffect 用於「同步資料到 effect 行為」
- useEffect 讓你根據目前的資料來同步 React elements (畫面) 以外的任何事物
- 一般情況下，useEffect 會在每次 component render 然後瀏覽器完成 DOM 的更新 & 繪製畫面後才執行，以避免阻塞 component render 的過程 & 瀏覽器繪製畫面的過程
- useEffect 概念上不區分 mount 與 update 的情況，他們被視為是同一種情境
- 預設情況下，每一次 render 後都應該執行屬於該 render 的 useEffect，來確保同步的正確性與完整性
- 理想上這個 useEffect 無論隨著 render 重複執行了幾次，你的程式都應該保持同步且正常運作
- useEffect 的 dependencies 是一種「忽略某些非必要的執行」的效能最佳化，而不是控制 effect 發生在特定的 component 生命週期，或特定的商業邏輯時機

### 不要欺騙 hooks 的 dependencies chain

- 把 function 定義到 useEffect 中 (只有該 effect 用到，沒有共用需求)
- 跟資料流無關的流程抽到 component 外部
- 用 useCallback

> 函式在 FC 與 hooks 中是屬於資料流的一部分

### Dependencies chain

useCallback useMemo 讓由原始資料產生出來的延伸資料能夠完全的參與資料流，並以 dependencies chain 維持 useEffect 的同步可靠性

### useReducer 是 dependencies chain 的合法作弊手段

從動作分離更新...

### 以 dependencies 來控制 useEffect 執行邏輯的誤區

- useEffect 的用途是同步資料到 effect，不是生命週期
- FC & hooks 也沒有提供任生命週期的 API
- useEffect 的 dependencies 是一種「忽略某些非必要的執行」的效能最佳化，而不是控制 effect 發生在特定的 component 生命週期，或特定的商業邏輯時機

### Reusable state — React 18 的 useEffect 在 mount 時為何會執行兩次？

- component 必須設計得有足夠的彈性，多次 mount & unmount 也不會壞掉
- Offscreen API
  - 讓 React 可以在 UI 切換時保留 component 的 local state 及對應的真實 DOM elements，像是把他們暫時隱藏，而不是真的移除
  - 當這個 component 有再次顯示的需求時，就能以之前留下來的 state 再次 mount
- 為了確保 component 能支援上述特性，effect 必須要是可重複執行也不會壞掉的

![](https://i.imgur.com/edtn0uj.png)

### React 哲學

> 有依賴資料的延伸的任何東西都要成為資料流的一部分

過去在 CC 中寫了 class 的 method 使用了 this.props blablabla，實際上這個 function 不會因為 props 改變而改變，所以無法透過這個 function 辨識到資料流發生變化，因此後續的同步就很容易漏掉。

FC 想做的是只要 input props 固定，output 就固定。為了完美做到這件事，必須讓所有用到原始資料的東西都參與資料流，包含 effect 用到的 function
、function 用到的資料、function 要寫在 component 內就要用 useCallback 來保證整個資料流 chain 不會斷掉。
