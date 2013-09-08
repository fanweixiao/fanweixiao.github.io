---
published: true
layout: post
category: homebrew
tags: 
  - socks5
  - github
---

這兩天Github貌似被功夫牆了，所以導致`brew update`這樣的命令會失效，如果homebrew不能用，那可損失大了。

通過`brew update -v`能看到是卡在了`git pull`的操作上，好在`git`現在的版本支持proxy了，操作方式也很簡單：

`cd /usr/local && git config http.proxy 'socks5://127.0.0.1:7070'`

然後執行`brew update`，順利通過。

