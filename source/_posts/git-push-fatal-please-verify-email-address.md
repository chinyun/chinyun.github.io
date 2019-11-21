---
title: git push fatal-please verify email address
date: 2019-10-31 16:48:41
tags:
---

Git 向遠端儲存庫傳輸資料有兩種方式：SSH 金鑰 或 HTTPS 通訊協定。

# 問題
這次發生 git push 失敗時，是打算透過 HTTPS 遠端傳輸檔案，將完成的檔案 git commit 到已經創建好的 Github Repository，但 iTerm 顯示下面資訊：

```
remote: You must verify your email address.
remote: See https://github.com/settings/emails. 
fatal: unable to access ‘https://github.com/username/repo.git/’: The requested URL returned error: 403
```
>P.S. 這裡 username 代表自己的使用者名稱；repo 代表 repository名稱。

參考 [Github 官方](https://help.github.com/en/github/getting-started-with-github/verifying-your-email-address)的說明 ，先前使用 Github 時已 verify 過 email 無法重寄認證信，而且確認設定的遠端地址正確。

# 原因：Git credential storage

之前使用的 Github 帳戶因為 email 刪除已經不存在，
即便已經重設過帳密，但是因為這台電腦儲存了我之前的帳戶的帳密，每次要 git push 的時候就會因為和遠地帳戶帳密不符而失敗。

類似的問題像是更改 username 或 密碼等也會遇到相同錯誤。
在 Stackoverflow 也有查詢到解法：改用 SSH 金鑰傳輸就不用認證帳密，如果還是想用 HTTPS 傳輸的話，就要研究一下要怎麼修改電腦設定。

要探討這個問題，須先了解 Git 的認證機制：
通過 HTTPS 傳輸時，預設需要輸入帳號密碼才可以訪問，而 Git 內建 credential storage 提供了開發者將帳密儲存於自己電腦中的功能，就不用每次遠端傳輸資料時都要輸入 帳號密碼；儲存可分為 osxkeychain、store、chache 三種方式。

# 解決辦法

先用這個指令查詢電腦系統支援的 git credentials 形式：
```
git help -a | grep credential
```

在不同的作業系統平台，Git credential storage 有不同的預設值，如果是 mac OS X 用 Homebrew 安裝 Git ，會得到如下的資訊：
```
credential                remote-ext
credential-cache          remote-fd
credential-cache--daemon  remote-ftp
credential-osxkeychain    remote-ftps
credential-osxkeychain    remote-http
```

Homebrew 安裝過程預設 HTTPS 通訊協定情況使用 osxkeychain 輔助工具來儲存認證訊息，表示透過 HTTPS 連接 Repository 時，系統用 osxkeychain 儲存的帳密向其要求傳輸權限，所以如果把 HTTPS 的 credential 設定修改，就能重新輸入新的、正確的帳號密碼了。

修改方式如下：

1. 用這行命令找出所有 credential.helper 在電腦中的位置：
```
git config --show-origin --get credential.helper
```
> 通常可能是在…/usr/share/git-core/gitconfig

2. 找到 .gitconfig 文件後，將下面這段文字修改後儲存：
```
原始預設：
[credential]   helper = osxkeychain  
刪除 osxkeychain 變成：
[credential]   helper =
```
> 此時沒有設置儲存方式，之後每次 push 都會詢問帳密。

3. 輸入以下指令來檢查是否已經成功取消osxkeychain設置，如果還是出現 osxkeychain 表示沒有完成，可能一台電腦上有多個 .gitconfig 文件需要處理
```
git config credential.helper
```

4. 檢查完成後，進行 git push ， 此時 git 會詢問帳密，便可輸入新的、正確的帳號密碼了。

5. 最後可以直接在 iTerm 命令列恢復 osxkeychain 設定：
```
git  config --global  credential.helper  osxkeychain
```
或開啟.gitconfig 文件修改 `[credential] helper = osxkeychain` ，由於使用 store 方式會產生一個明碼的檔案在電腦中，比較不建議，為了安全起見，還是使用 osxkeychain 方式來儲存帳密。而如果是用 cache 方式的話，會將帳密暫存在硬碟中，一定時間內可以快取，但時間過了就要再重新輸入。
這樣就完成了 git credentials 的重新設置，另外如果想要查看 git config 的相關設定，可以用這個指令 `git config --list` 。
