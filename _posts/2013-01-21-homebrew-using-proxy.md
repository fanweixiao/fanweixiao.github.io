---
layout: post
category : homebrew
tags : [homebrew, github]
---
{% include JB/setup %}

Github.com被ban後，homebrew的安裝也深受影響，這裏提供一個簡單的臨時解決方案

## Install homebrew

添加hosts，解決github的dns poison問題：

	sudo vim /etc/hosts
	
添加：

	207.97.227.243  raw.github.com
	207.97.227.252  nodeload.github.com
	207.97.227.239  github.com

然後執行這條命令快速安裝homebrew：

	ruby -e "$(curl -fsSkL https://raw.github.com/mxcl/homebrew/go)"
	
注意其中的**https://**

當然您也可以[使用Homebrew wiki裏提到的高級安裝辦法](https://github.com/mxcl/homebrew/wiki/Installation):

	mkdir homebrew && curl -L https://github.com/mxcl/homebrew/tarball/master | tar xz --strip 1 -C homebrew

## homebrew的formular安裝遇到網絡問題
homebrew[支持http_proxy這個env](https://github.com/mxcl/homebrew/wiki/Tips-N%27-Tricks)，如果您有http proxy請直接從**第4步**開始，如果只有socks proxy，操作如下：

假設您的socks5 proxy監聽端口是**127.0.0.1:7070**，然後想創建一個**127.0.0.1:8118**的http proxy：

1. 安裝polipo：`brew install polipo`
		
2. 按照如下方式創建文件：`vim ~/.polipo.conf`

		proxyAddress = "127.0.0.1"
		proxyPort = 8118
		socksParentProxy = "127.0.0.1:7080"
		socksProxyType = socks5
	
3. 啓動polipo：`polipo -c ~/.polipo.conf`（polip更多的配置可以[參考這裏](https://gitweb.torproject.org/torbrowser.git/blob_plain/1ffcd9dafb9dd76c3a29dd686e05a71a95599fb5:/build-scripts/config/polipo.conf)）
4. 開始安裝：`http_proxy=http://127.0.0.1:8118 brew install tmux`

## 我常用的formula
+ wget
+ tree
+ tmux
+ polipo
+ meld
