---
layout: post
category : kvm
tags : [kvm]
---
{% include JB/setup %}

vm本身是需要開啓acpid服務來接受電源相關管理的，如果vm本身沒有這個服務，那麼reboot、shutdown等操作都是無效的。

處理方式很簡單：

	yum -y install acpid
	/etc/init.d/acpid start
	chkconfig --level 235 acpid on