---
published: true
layout: post
category: linux
tags: 
  - shell
  - bash
  - osx
---

## 前言

我的工作環境是Ubuntu，自己家裏的環境是Mac，我使用自己的Dot Files。其中對於`ls`命令，我都設置了幾個alias，例如`alias ll='ls -ashlF --color'`。但是在Mac下的`ls`命令是沒有`--color`這個選項的。這是因爲OSX的`ls`命令是BSD版本的，所以這裏要做的是讓Mac和Ubuntu下的表現是一樣的。

## Mac下使用GNU版本的ls

![GNU Image](http://www.gnu.org/graphics/gnu-head-sm.jpg)

`ls`在[GNU的coreutils裏](http://www.gnu.org/software/coreutils/)，mac下很容易用homebrew來安裝：`brew install coreutils`。

安裝完後，coreutils裏的命令都可以在前面加一個`g`來使用，比如`gls`來使用GNU的`ls`命令，當然，可以設置`PATH`來實現全部替換。

但是如果不想替換（像我），那麼就只改動alias腳本就可以了：

	unamestr=`uname`
    if [[ "$unamestr" == "Darwin" ]]; then
      alias ll='gls -aslhF --color'
      alias la='gls -A --color'
      alias l='gls -CF --color'
    else
      alias ll='ls -aslhF --color'
      alias la='ls -A --color' 
     alias l='ls -CF --color' 
    fi

最後，[要注意Mac下的.bash_profile哦](http://www.joshstaiger.org/archives/2005/07/bash_profile_vs.html)
