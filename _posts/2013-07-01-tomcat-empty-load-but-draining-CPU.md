---
published: true
layout: post
category: java
tags: 
  - centos
  - java
  - performance tuning
---

{% include JB/setup %}

## 症狀

tomcat啓動之後，在沒有任何請求的情況下，發現一直在使用CPU，這一定存在問題，開始找。

## 尋找java進程的問題

	1. 找到java進程的pid
    ps aux|grep java
    
    2. 看哪消耗CPU的Thread Id
	top -H -p <PID>
    
    3. 看這個thread在幹什麼
	jstack <PID> |grep `printf %x <Thread Id>`

我的這例情況發現這些thread都是tomcat的，跟我的應用程序無關。所以轉爲google，後來找到了解決問題的辦法：[java leap second bug](http://blog.wpkg.org/2012/07/01/java-leap-second-bug-30-june-1-july-2012-fix/)，運行`dmesg`確實看到的了`Clock: inserting leap second 23:59:60 UTC`，按照文中所說的方法，解決。[問題詳細](http://www.wired.com/wiredenterprise/2012/07/leap-second-bug-wreaks-havoc-with-java-linux/)
