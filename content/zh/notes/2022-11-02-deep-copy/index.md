---
title: "Deep Copy in JavaScript"
summary: Some methods to deep copy an object in JavaScript
categories: ["note"]
tags: ["JavaScript", "DeepCopy", "深拷貝", "深層複製"]
#externalUrl: ""
# showSummary: true
date: 2022-11-02
draft: false
---

物件的深拷貝(深層複製)是指其屬性和複製的來源物件的屬性不共享相同的引用（指向相同的底層值）的副本。因此，當您更改來源或副本時，可以確保不會導致其他物件也發生更改；也就是說，您不會意外地對來源或副本造成預期之外的更改。這種行為與淺層複製的行為形成對比，在淺層複製中，對來源或副本的更改可能也會導致其他物件的更改（因為兩個物件共享相同的引用）。

### structureClone

#### 簡介

實際上，瀏覽器本身就有很多地方需要深度複製例如﹔將資料儲存在 IndexedDB 時序列化和反序列化。利用 postMessage() 將資料傳給 Web Worker 等情境都需要類似的處理。而內部其實是使用一種稱為結構複製的演算法，過去這功能並沒有提供給開發者，但現在我們可以使用 structuredClone() 了。

#### 功能與限制

結構複製解決了大部分 `JSON.stringify()` 的問題，可以使用遞迴資料結構，JS 內建型別，效能也不錯。

但還是有些限制

- Prototypes﹔如果您使用 structuredClone() 複製某類別物件實例 Class Instance 您只會取得單純的物件不會包涵 prototype 的部分
- Function﹔如果您的物件包涵了函式則會被移除
- 不可複製﹔有些值是不可複製的例如 Error 和 DOM 節點

#### 其他深層複製方法

1. Lodash
2. 轉 JSON 字串再轉回來

```js copy showLineNumbers=
const copy = JSON.parse(JSON.stringify(original));
```

但需要特別注意有些值經過 JSON.stringify/parse 處理後，會產生變化，導致非預期的結果發生：

- undefined : 會連同 key 一起消失。
- NaN : 會被轉成 null。
- Infinity :會被轉成 null。
- regExp /\*/: 會被轉成空 {}。
- Date : 型別會由 Data 轉成 string。
- function : 會連同 key 一起消失。

```js copy showLineNumbers
const originalData = {
  undefined: undefined, // undefined values will be completely lost, including the key containing the undefined value
  notANumber: NaN, // will be forced to null
  infinity: Infinity, // will be forced to null
  regExp: /.*/, // will be forced to an empty object {}
  date: new Date("1999-12-31T23:59:59"), // Date will get stringified
  function: () => {},
};
const faultyClonedData = JSON.parse(JSON.stringify(originalData));

console.log(faultyClonedData.undefined); // undefined
console.log(faultyClonedData.notANumber); // null
console.log(faultyClonedData.infinity); // null
console.log(faultyClonedData.regExp); // {}
console.log(faultyClonedData.date); // "1999-12-31T15:59:59.000Z"
console.log(faultyClonedData.function); // undefined
```

### Reference

- [JS 中的淺拷貝 (Shallow copy) 與深拷貝 (Deep copy) 原理與實作](https://www.programfarmer.com/articles/javaScript/javascript-shallow-copy-deep-copy)
- [[譯]在 JavaScript 使用 structuredClone 深度複製](https://andyyou.github.io/2021/12/19/javascript-structured-clone-2021/?hmsr=joyk.com&utm_source=joyk.com&utm_medium=referral)
- [最新 HTML 规范——structuredClone 深拷贝函数，能取代 JSON 或者 lodash 吗？](https://juejin.cn/post/7044934738431180830)

- [How to deep clone a JavaScript object](https://flaviocopes.com/how-to-clone-javascript-object/)
