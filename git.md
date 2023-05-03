[TOC]
#Git 常用指令
| 功能 | 指令 | 備註 |
|:--:|:--|:--|
| 更新到最新的提交 | git pull | 遠端的話就加上 ```origin/分支```
| 回到過去 | git rebase |
| 切換分支 | git checkout (branch) |
| 查看遠端分支 | git branch -r |
| 新增並切換 | git checkout -b (branch) |
| 遍歷過往 | git log |
| 起始回生 | git reflog | F鍵:往下</br>B鍵:往回</br>Q鍵:關閉 |
| 合併分支 | git merge (要合併的branch) |
| 切換分支 | git checkout (branch) |
| 刪除錯誤的推送 | git revert (某個紀錄) | ```:x!```為確認修改,修改完後push |

#rebase
使用```rebase```是將當前的提交來合併到目標分支
```git
git checkout feature
git rebase --squash master
# 以上會將 feature 分支上的提交合併成為一個提交，並且應用到master分支上，你需要手動輸入commit
```

##範圍選擇
`git rebase -i HEAD~3`
以上的`-i`是與git互動，`HEAD~3`是從當前最新提交到依序往前三個
此命令會打開編輯器(應該是vim):
```bash
# 會出現類似以下的訊息
pick SOME12345 測試一號
pick SOME34567 測試二號
pick SOME56789 測試三號
# 假設要把三個提交合併為第一個，就把前面的pick改為
pick SOME12345 測試一號
squash SOME34567 測試二號
squash SOME56789 測試三號
# 接著會顯示這三個的commit message
# 在手動把第二第三的commit刪掉
# 都修改完後再push上去
```
##rebase 與 merge 的不同
`git merge` 是將一個分支的歷史記錄合併到另一個分支，生成一個新的合併提交，將兩個分支的歷史記錄合併起來。合併提交有兩個父節點，分別指向合併前的兩個分支。合併的歷史記錄會保留下來，**<font color=Red>形成一個分叉的歷史記錄</font>**。
`git rebase` 是將一個分支的歷史記錄轉移到另一個分支上，讓這個分支看起來像是在新的基礎上開始開發的。在 rebase 過程中，Git 會將這些提交從原來的分支上拆下來，逐個應用到目標分支上。這樣可以使得目標分支的歷史記錄更加緊湊，**<font color=Red>沒有分叉</font>**。
>git merge 保留原來分支的歷史記錄，會產生分叉
git rebase 則重新整理歷史記錄，沒有分叉

#Stash 暫存
| 功能 | 指令 | 備註 |
| :--: | :-- | :-- |
| 暫時存檔 | git stash | 加上 save "自訂義紀錄訊息" |
| 查看暫時存檔 | git stash list |
| 讀取暫時存檔 | git stash pop (位置的key) | *會清除stash的紀錄 |
| 讀取暫時存檔 | git stash apply (位置的key)  | *不會清除stash的紀錄 |

#解決衝突
當兩個或多個開發者對同一個文件或代碼進行更改並提交時，就會發生提交衝突。Git 無法自動解決提交衝突，因此需要開發者手動解決
執行 git status 命令檢查當前的狀態，看是否有提交衝突。

1. 運行 `git pull` 命令從遠端存儲庫中拉取最新的更改，如果有衝突，Git 會提示你解決衝突。在解決衝突之前，你需要先確定當前分支與遠端分支的關係，以便選擇正確的解決策略。
   **`git config pull.rebase false` 設定 `merge`當作解決策略**
   **`git config pull.rebase true` 設定 `rebase`當作解決策略**


2. 手動解決提交衝突。打開發生衝突的文件，會看到類似以下的標記：

    ```bash
    <<<<<<< HEAD
    這是你的更改
    =======
    這是其他開發者的更改
    >>>>>>> branch_name
    ```
    左側的 <<<<<<< HEAD 標記表示當前分支的更改，右側的 >>>>>>> branch_name 表示另一個分支的更改，======= 則是當前分支和其他分支的分界線。

3. 你需要手動編輯這些衝突，將它們整合成一個正確的版本。例如，你可以刪除衝突標記和錯誤的代碼，保留正確的更改。解決衝突後，保存文件。

4. 運行 `git add` 命令將已解決的文件加入暫存區。如果你解決了多個文件的衝突，可以使用 `git add` . 命令將所有文件都加入暫存區。

5. 運行 `git commit` 命令提交已解決的衝突。

6. 如果需要，運行 `git push` 命令將本地提交推送到遠端存儲庫。

#如何採櫻桃
O-O-X-O-O
先用 `git reflog` 看一下紀錄
再用 `git reset --hard` 回到錯誤前也就是第二個存檔(第二個圈圈)
```git
git reset --hard (2-O的存檔紀錄)
# --hard 表示讀取存檔之前的檔案都不要留
# 回到第二個O之後

git cherry-pick (第四個O)
git cherry-pick (第五個O)
#這樣就會跳過第三個X,把四五的提交加進來
```
#起死回生 git reflog
先用 ```git reflog``` 顯示過往所有的動作紀錄
再用 ```git reset --hard``` 回到當初那一個紀錄
最後用 ```git checkout```回到原來的樣子

