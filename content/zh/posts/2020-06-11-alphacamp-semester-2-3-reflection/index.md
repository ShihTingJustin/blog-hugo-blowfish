---
title: ALPHA Camp 學期2–3 自學經驗回顧
summary: 學期 2–3 正式開始進入後端開發的領域，使用的語言依舊是 Javascript，但環境從 broswer 換到了 server ，也開始接觸 Javascript 的執行環境 Node.js 及網路框架 Express。
image: https://i.imgur.com/OsE7ovi.jpg
categories: ["blog"]
tags: ["ALPHA Camp", "Bootcamp", "Career", "SoftwareEngineer"]
#externalUrl: ""
# showSummary: true
date: 2020-06-11
draft: false
---

學期 2–3 正式開始進入後端開發的領域，使用的語言依舊是 Javascript，但環境從 broswer 換到了 server ，也開始接觸 Javascript 的執行環境 Node.js 及網路框架 Express。

在寫作業時開始感受到前後端的區別，不論在 server 端多麼用力的使用 BOM or DOM 的相關語法，除了噴紅字以外是不會有任何回應的 😂

![0_gTMTcGPVAuwZI8cX](https://i.imgur.com/IIa8RIX.jpg)

在寫老爸私房錢的作業時，想要操作兩個 Schema 來 create 消費的資料及種類的資料，想將`Category.find()`撈出來的資料拿到`Record.create()`裡面用

```js copy showLineNumbers
//recordSeeder.js
let category = []
Category.find()
  .lean()
  .then(cates => {
    console.log(cates) // 這邊有看到資料
    return category.concat(cates)
  })
取出 Category 的資料，放進之後的 record 種子資料

db.once('open', () => {
  console.log('mongodb connected!')
  for (let i = 0; i < 10; i++) {
    Record.create({
      category: 這邊要用,
      name: pickName(),
      date: pickDate(),
      amount: Math.floor(Math.random() * 500 + 50)
    })
  }
  console.log('done!')
})
```

結果發現不行，只能把`Record.create()`寫在`Category.find(`的 `.then()`裡面。如果還有要執行的動作，就要一直`.then()`下去...

```js copy showLineNumbers
db.once("open", () => {
  console.log("mongodb connected!");

  Category.find()
    .lean()
    .then((cates) => {
      console.log(cates); // 這邊有看到資料

      for (let i = 0; i < 10; i++) {
        Record.create({
          category: 這邊要用,
          name: pickName(),
          date: pickDate(),
          amount: Math.floor(Math.random() * 500 + 50),
        });
      }
    });

  console.log("done!");
});
```

原來這就是非同步操作著名的 callback hell...我寫兩層就覺得不太對勁了，下面這種到底...

![0_4MpoPOr7ZMxPmw4E](https://i.imgur.com/P95ookU.png)

參考了政治助教分享的 Promise 筆記好像從有懂一點？但來不及實作在老爸私房錢的作業上。

在最後驗收單元寫 URL Shortener 時，我的邏輯是這樣：

![0_7C2bk9PeG7mNgSJ3](https://i.imgur.com/8Cla5KS.jpg)

中間又遇到了非同步操作的問題，像是在 `urlCheck()` 檢查網址能否正常連線，可以連線才會執行`generateShortURL()`產生短網址，但程式執行的結果卻是不管網址能不能連線，都會產生短網址 😵 。

經過一連串的`console.log()`除錯，發現`generateShortURL()`沒有等待`urlCheck()`，這時候就需要使用 Promise 啦～

雖然最後的`async/await`寫得有點笨笨的，但還是成功使用 Promise 解決問題了，非常開心。

好在這個專案我已經逐步摸索出非同步的寫法，現在學期三的作業也需要用到非同步的寫法，使用起來就更加順手，不過還是要在學期三從頭學習 callback, Promise, async / await 演進的過程與精進的寫法。

```js copy showLineNumbers
async function shortenAsyncAwait() {
  const value = await urlCheck();
  const value1 = await generateShortURL(value, url);
  const value2 = await checkShortUrlDuplicated(value1);
  const value3 = await createData(value2);
  const previewData = await linkPreviewGenerator(url);
  const value4 = await saveThumbnail(value3, previewData);
}
shortenAsyncAwait();
```

首次嘗試暗色系的 UI 配置，有種神秘的感覺。

![1_WyKSIpuz5OdYgugeeRlEuQ](https://i.imgur.com/25VXBEw.png)

實作短網址網站常有的後台追蹤點擊次數功能

![1_PBsRq-ramY5rj64yCli5zQ](https://i.imgur.com/7bFd9tY.png)

這個專案有部署到 Heroku，功能可以正常運作，但是短網址管理介面的 網頁 preivew 功能因為套件與 Heroku 會衝突，所以無法正常顯示，目前只能在本地端看到網頁 preview 😢。

[**URL SHORTENER**
*Edit description*frozen-atoll-45486.herokuapp.com](https://frozen-atoll-45486.herokuapp.com/)

完整的程式碼可以在我的 GitHub 查看，喜歡的話歡迎 Fork 並幫我按個 Star 吧！

- [ShihTingJustin/url_shortener](https://github.com/ShihTingJustin/url_shortener)
