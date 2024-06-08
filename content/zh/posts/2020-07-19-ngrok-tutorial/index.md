---
title: 5分鐘完成 ngrok 設定(Mac教學)
summary: ngrok 是一個可以快速讓外網連接本機 localhost 的工具，話不多說，六個步驟帶你快速完成設定！
image: https://i.imgur.com/OsE7ovi.jpg
categories: ["blog"]
tags: ["Ngrok", "SoftwareEngineer", "Frontend", "Backend", "Tutorial", "教學"]
#externalUrl: ""
# showSummary: true
date: 2020-07-19
draft: false
---

ngrok 是一個可以快速讓外網連接本機 localhost 的工具，話不多說，六個步驟帶你快速完成設定！

1. 到 ngrok 官網下載安裝檔
   ![](https://i.imgur.com/EV9qYL5.png)
2. 下載完成後解壓縮，解壓縮完會出現一個檔名為 ngrok 的執行檔
   ![](https://i.imgur.com/l5j6d0j.png)
3. 把執行檔放進 /usr/local/bin（可參考下圖 Finder 操作方式）
   ![](https://i.imgur.com/BACDAOs.png)
   P.S. 也可以放在你的任何一個專案目錄下，那 ngrok 指令也要在專案目錄下才能執行。
   ![](https://i.imgur.com/0UEDTep.png)
   ![](https://i.imgur.com/aRhIVKR.png)
4. 點兩下打開執行檔，完成會跳出終端機畫面
   ![](https://i.imgur.com/r0uEUSQ.png)
5. 回到 ngrok 官網創一個帳號，登入後選擇畫面左邊的 Setup & Installation，複製畫面右邊的 2. Connect your account 指令
   `ngrok authtoken ?????????????????????`
   （問號指的是你的 Authtoken，官網的指令有包含 /.要注意不要複製到）。
   ![](https://i.imgur.com/t0KRWx7.png)
   然後再開一個新的終端機，把指令貼上並執行，會出現這段訊息，代表 Authtoken 已經存到 configuration 了。
   `Authtoken saved to configuration file: /Users/YourName/.ngrok2/ngrok.yml`
6. 啟動你的專案後，在終端機輸入這行指令 `ngrok http 3000`（port 3000），終端機會出現下圖畫面。
   現在可以使用 ngrok 提供的 Forwarding 網址來 call API 囉！
   例如我有一支 API 是路由是 `/api/users`，那就會是這樣表示：`http://219e817c70a5.ngrok.io/api/users`
   ![](https://i.imgur.com/5lwSFBe.png)
