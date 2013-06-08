---
published: false
layout: post
category: centos
tags: 
  - linux
  - performance
---

## 第一次在Linux服務器上遇到這種Low CPU Hang的問題

這是我第一次在CentOS上遇到這種Low CPU Hang的問題，之前我的服務器端的經驗都是在Windows Server + .NET平臺上。不過反過來也說實話，Windows Server的Performance Counter還真是超級好用的東東。跑題了，把這次故障的解決思路記錄下來：

## 狀況表現

[WPDang](http://www.wpdang.com/)的服務器最近兩天極度不穩定，今晚正好有時間，就來找找問題。

ssh鏈接服務器，發現速度超級慢，來回嘗試了幾次，大概要4分鐘左右，就卡在了`Last Login`，之後再也不顯示內容了。

`ssh -vvv`後看了一下，排除了網絡的問題，打開[zabbix](http://www.zabbix.org/)搭建的監控系統之後也查看了eth0是沒問題。

說實話這個問題我沒遇到過，在無法login進服務器時，先是把服務器上的nginx、php-fpm和MySQL都停掉了，然後再login，這下沒問題了，進去了。上去之後開top，然後把逐個服務器打開，盯着top看了一會，發現CPU的User time不高，而Wait的消耗確是很高的，平均在70%~99%，這時候Nginx的平均請求約65/s，是Wordpress系統和2個NodeJS的服務。

## bug repro

雖然注意到了`%wait`的值有些高，但是我並沒有太多CentOS上的經驗，此前也沒有太多的關注過這個值，加上服務器本身還是泰Virtual Machine，所以慣性思路還是從服務上去找的，來回的關閉掉業務，看top，然後再開啓，看着top，幸好有[tmux]()，vertical排列兩個pane，左側是top，右側操作，方便了很多。

反覆幾次之後發現，停了所有業務進程後，`%wait`就在0%~3%之間，開啓後就平均在70%以上。抄起Google，[看看它是什麼意思](http://blog.scoutapp.com/articles/2011/02/10/understanding-disk-i-o-when-should-you-be-worried)。

## 應該就是I/O的問題了

看看I/O的情況，執行`vmstat 2`，每2s刷新一次結果，看到pre-last col（也就是倒數第2列的值）的值還是挺高的

然後就找[誰消耗了%wait](http://stackoverflow.com/questions/666783/how-to-find-out-which-process-is-consuming-wait-cpu-i-e-i-o-blocked)：

`while true; do date; ps auxf | awk '{if($8=="D") print $0;}'; sleep 1; done`

