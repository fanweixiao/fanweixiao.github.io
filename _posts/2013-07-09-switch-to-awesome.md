---
published: false
layout: post
category: awesome
tags: 
  - linux
---

# Using Awesome as your Windows Manager

這個倒是不難，直接`sudo apt-get install awesome`後就能使用了，當然，必須要Logout出現在的WindowsManager環境，Login時選擇使用Awesome才行。

進來後，左上角有菜單可以點擊（是的，支持鼠標的）。當然，我是有癖好的人，很多都需要自定義

### Dual Monitor

我一直是雙顯使用者，Laptop + Extension Monitor的方式已經習慣了，切換到awesome後，默認是Mirror模式使用兩個Monitor，而且分辨率也很可恥，動手：

1. 運行`xrandr -q`，輸出結果會告訴我們現在顯示器的相關狀況，比如，我公司的工作環境，會告訴我有`VGA1`和`LVDS1`，前者是外接顯示器。此外，還會告訴我可以用哪些分辨率，根據這些內容，開始設置分辨率
1. 那麼先設置Laptop的分辨率：`xrandr --output LVDS1 --mode 1366x768`（恩！公司的筆記本是挺讓人崩潰的，半個屁股大的屏幕。。。）
1. 我的外接顯示器在Laptop的左側，也就是說`VGA1 left of LVDS1`，那麼可以這樣設置`xrandr --output VGA1 --mode 1440x900 --left-of LVDS1`（恩！外接顯示器也很崩潰的說。。）

OK，雙顯基本OK了，[參考](http://awesome.naquadah.org/wiki/Using_Multiple_Screens)

### Keyremap
