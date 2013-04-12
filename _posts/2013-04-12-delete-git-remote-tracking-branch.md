---
layout: post
category : git
tags : [git]
---

## 產生原因

公司都項目使用Git管理，有2個main repo，一個是公司局域網內的（命名爲origin），一個是公網的（命名爲upstream）。所以origin上的分支一般都是tracking的。在公司時git push操作也是只針對origin，然後由hook腳本做相應的upstream git repository的操作，比如創建新分支，提交更新，刪除分支，創建tag等操作。

經常開發機器需要執行git fetch upstream來同步查看upstream的情況，而當本地執行git branch -d XXX再push上去時，origin和upstream都會做相應刪除分支操作。但是本地都/remote/upstream/XXX確是依然存在的

## 解決辦法

`git remote prune XXX`或者`git fetch -p upstream`或者`git branch -rd XXX`