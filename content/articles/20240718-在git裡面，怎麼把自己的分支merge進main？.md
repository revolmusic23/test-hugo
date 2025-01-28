---
title: 在 Git 裡面，怎麼把自己的分支 merge 進 main？
date: 2024-07-18
tags: ["Git"]
description: "每次多人協作，都有一堆分支衝突嗎？"
slug: how-to-merge-git-branch-to-main
draft: true
---

簡單來說，分成三個階段：

1. 將自己分支的本地端和遠端同步。
2. 將主分支合併到自己的主分支，並讓本地端和遠端同步。將主分支合併到自己的主分支，並讓本地端和遠端同步。
3. 將自己分支合併到主分支。

假設公司的主要分支叫做 `main`，而我現在在開發的分支叫做 `feature/login`，那我要怎麼把我現在的分支合併到 `main`？
首先，要先讓自己的分支是乾淨的，也就是要在 `feature/login` 執行：

```shell
git pull
git push
```

執行完之後，現在本地端和遠端的 `feature/login` 是同步的。

接著，要把遠端最新的 `main` 給拉到本地端，執行：

```sh
git fetch origin main
```

此時，我的本地端就有最新的 `feature/login` 和 `origin/main`。

接著就是要把這兩個分支合併起來。在 `feature/login` 分支上執行：

```sh
git merge origin/main
```

這個指令是要將剛剛從遠端啦過來的最新的 `main` 合併到我的 `feature/login`。
此時，可能會發生所謂的「衝突」（Conflict），這是因為你跟別人可能有改到同一個檔案。因此，所謂的「解衝突」就是要決定說，在這個檔案裡面，兩個檔案不一樣的地方要選擇哪一個。

當解完衝突之後，再執行：

```sh
git push
```

這時候，你是將你的 `feature/login` 分支（其中也包含 `main` 的部分）給推到遠端程式碼上面。
此時可以檢查 Commit 記錄。所謂的 Commit 記錄就是每一次程式碼提交的「截圖」。
若 `main` 最新的 Commit 記錄有包含在 `feature/login` 的 Commit 記錄裡面，那就代表你的 `feature/login` 已經包含了 `main`，這時就可以發出合併請求（Merge Request），等待主管同意。
