---
layout: post
category : linux
tags : [linux, centos, ssh]
---
{% include JB/setup %}

公司搬到MS大樓後所有開發服務器要做IP遷移，但是平時大家討論問題時仍然使用原來的對服務器的稱呼，比如31（以前的IP是192.168.1.31）.所以想在ssh登錄到某台服務器上後能顯示出這臺機器以前的IP。

第一種方法是使用Banner功能，但它的缺陷是當執行git fetch這類命令的時候也會顯示出來。第二種是用MOTD，符合我的要求。

# Banner方式 #
- edit： `/etc/ssh/sshd_config`
- uncomment： `Banner /etc/ssh/my_banner` 
- add msg to： `echo 'CC' > /etc/ssh/my_banner`

# MOTD方式 #
- edit： `/etc/ssh/sshd_config`
- uncomment： `PrintMotd yes`
- add msg to： `/etc/motd`

記得最後__service sshd reload__重啓sshd服務套用更新
