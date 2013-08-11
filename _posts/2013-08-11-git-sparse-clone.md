---
layout: post
category : git
tags : 
  - git
---
{% include JB/setup %}

## 問題

看到一個Git Repository裏的部分代碼，想clone到本地慢慢學習，可是整個Repo很大，要執行`git clone repo`的時間卻很長

## Sparse checkout

我們以只獲取Github的[Awesome-Themes-3.5](https://github.com/Morley93/awesome-themes-3.5)中的`arch`目錄下的`titlebar`和`tasklist`兩個子目錄裏的內容爲例：

1. `git init awesome-themes-3.5.git`
2. `cd awesome-themes-3.5.git`
3. `git remote add -f origin git@github.com:Morley93/awesome-themes-3.5.git`
4. `git config core.sparsecheckout true`（或者手工編輯`.git/config`文件，在`[core]`節點裏加上`sparsecheckout = true`）
5. `echo 'arch/titlebar' >> .git/info/sparse-checkout`
6. `echo 'arch/tasklist' >> .git/info/sparse-checkout`
7. `git pull origin master`

## 注意

sparse checkout並不是說只取一部分內容，git相關的還是會取下來的，所以能節省多少，還是要看repo的情況。