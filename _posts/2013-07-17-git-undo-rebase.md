---
published: true
layout: post
category: Git
tags: 
  - Git
---

## 产生原因

两个branch，一个是`master`，一个叫`fea-xx`，在`fea-xx`上完成了功能后，想整理一下commits然后再merge到`master`上，结果`master`上有些问题，执行`git checkout fea-xx`的时候并没有成功，正好当时同事喊我，自己也大意了忘了这错误的事儿了，直接一个`git rebase HEAD~5`，就悲剧了。

## 解决方法

超级简单：执行`git reflog`，找到`master`之前在的内个commit的标识，比如`HEAD@{15}`这样，然后直接`git reset --hard HEAD@{15}`，就ok了。
