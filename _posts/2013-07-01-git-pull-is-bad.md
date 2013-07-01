---
published: true
layout: post
category: git
tags: 
  - git
---

## Why git pull sometimes bad

在更新本地的Git repository的時候，使用git-pull方式，比如：`git pull origin master`，那麼如果不是fast-forword的，那一定會自動創建一個類似下面的commit：

	*   f15ee07 - Merge remote-tracking branch 'origin/master'

這樣的commit還是挺不好的，把Branch搞的很難看，而且還有很多負面作用（比如git-blame）

`git pull --rebase`也不是很好。這裏推薦下面這種方式：

## A better way

分三步：

	## 1）更新remote tracking branches（比如origin/master）
    ## 如果有多個remote，都會執行。如果只想從origin獲取更新，可以git fetch origin -p
	git remote update -p
    
    ## 2）嘗試使用fast-forward方式更新本地分支
    ## - 如果成功，那麼整個過程結束
    ## - 如果失敗，執行第三步
    git merge --ff-only @{u}
        
    ## 3）rebase
    git rebase -p @{u}
    
    ## 4）Review
    git log --graph --oneline --decorate --date-order --color --boundary @{u}..
    
這樣就舒服多了。
    

