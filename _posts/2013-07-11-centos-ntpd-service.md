---
published: true
layout: post
category: CentOS
tags: 
  - linux
  - date
---

## 服務器時間不準確的處理步驟

1. 先檢查是否有`ntpd`服務和`ntpdate`，如果米有，就`yum -y install ntpd ntpdate`
2. 設置成正確的`timezone`，比如我的服務器`ln -s /usr/share/zoneinfo/Asia/Taipei /etc/localtime`
3. `ntpdate ntp.tuna.tsinghua.edu.cn`矯正時間
3. 運行`date`檢查一下時間是否正確
3. `chkconfig ntpd on`開啓ntpd服務在系統啓動時自動加載
3. `/etc/init.d/ntpd start`立刻啓動該服務
3. `hwclock --systohc`寫到硬件時鐘里


