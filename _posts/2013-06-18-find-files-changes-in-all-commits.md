---
published: true
layout: post
category: git
tags: 
  - git
---

如果想查看項目中的某個文件都在哪些commits里被修改過，可以用這個命令：

`git log --pretty=oneline -n 20 --graph -- /path/to/file`

或者更好看一點的：

`git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr, %an)%Creset' --abbrev-commit --date=relative -20 -- /path/to/file`

當然，配置一下`~/.gitconfig`文件，就可以享受[git alias](http://git-scm.com/book/en/Git-Basics-Tips-and-Tricks#Git-Aliases)了

找到了都在哪些commits里後，就可以這樣來比較在任意兩個commits中間的diff：

`git difftool HEAD^^..543210 -- /path/to/file`
