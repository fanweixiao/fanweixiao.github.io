---
layout: post
category : homebrew
tags : [homebrew, path]
---
{% include JB/setup %}

# $PATH相關問題

運行`brew doctor`的時候，如果提醒`/usr/local/bin`在`/usr/bin`之後，那麼會優先使用osx默認自帶的命令了，這時候修改該文件即可：`sudo vim /etc/paths`，將前者的位置調整到前面即可。

調整完後記得關掉該terminal的tab再重新打開就可以啓用了，可是使用`echo $PATH`檢查是否正確，當然也可以使用`brew doctor`檢查的。

# 一次性更新所有已經安裝的formula

	brew upgrade
	