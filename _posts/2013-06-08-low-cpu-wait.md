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

看看I/O的情況，執行`vmstat 2`，每2s刷新一次結果，看到pre-last col（也就是倒數第2列的值）的值還是挺高的。但是`vmstat`命令並不能夠完全說明問題，因爲他包含了所有foreground和background的進程的I/O消耗，如果前臺現在一些消耗I/O的東東，那它是不會block整個linux server的。但是這個命令至少能確定問題是出在這裏了。

## 找到消耗I/O的進程

[參考這裏](http://www.chileoffshore.com/en/interesting-articles/126-linux-wait-io-problem)用`ps auxf`看`STAT`列，有下面幾個字段：
	
    D    Uninterruptible sleep (usually IO)
    R    Running or runnable (on run queue)
    S    Interruptible sleep (waiting for an event to complete)
    T    Stopped, either by a job control signal or because it is being traced.
    W    paging (not valid since the 2.6.xx kernel)
    X    dead (should never be seen)
    Z    Defunct ("zombie") process, terminated but not reaped by its parent.
    
if a process with its stat with "D", it means it is actually taking all CPU resource with no any possible interruption. This means your Linux Box will wait on IO and does not responding any other commands if such process is always there.

然後用`while true; do date; ps auxf | awk '{if($8=="D") print $0;}'; sleep 1; done`把所有STAT列爲D的都打印出來，看看都有哪些process，像我，找到的是jbd2/dm-0-8（jbd is the "journaling block device". d-0-08 indicates a device mapped by device mapper）。

那麼直接就盯着它看吧：`while true; do ps auxf | grep "jbd2\/dm-0-8" |grep "D"; sleep 1; done`

## 磁盤健康狀況

`hdparm -Tt /dev/sda`看看，這裏我還是不透漏了當時的結果了...數太低了，估計是虛擬機背後挂的盤I/O不足了，要不就是盤要快報廢了，趕緊跑備份。。。
