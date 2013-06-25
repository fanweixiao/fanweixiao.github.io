---
published: true
layout: post
category: centos
tags: 
  - linux
  - performance
---

## 服務器內容

+ MariaDB, serve for [WPDang.com](http://wpdang.com) and Zabbix Server
+ Zabbix_Server
+ Zabbix_Client
+ Nginx
+ 1 PHP Zabbix Website
+ 1 Sinatra Application
+ 2 Nodejs Services

## 症狀

+ Low CPU
+ SSH login too slow
+ [WPDang.com](http://wpdang.com) can not open
+ Zabbix Web can't not open

## 問題

`ulimit -n`看了一下，結果是`1024`，才想起這臺機器沒有做配置優化，`ulimit -n 100000`先解決掉當前問題

## 最好的解決辦法

`/etc/sysctl.conf`添加`fs.max-files=xxx`，然後`sysctl -p`使之生效

`/etc/security/limits.conf`添加點內容，google可搜到，這樣重啓後就沒問題了

## tips

如果ssh login不上去，可以`ssh SERVER '/etc/init.d/xxxxx stop'`把服務都停掉，`kill`掉也可以
