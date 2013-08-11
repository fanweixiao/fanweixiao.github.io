---
layout: post
category : osx
tags : 
  - keybinding
---
{% include JB/setup %}

習慣了使用鍵盤進行光標控制，使用emacs的keybindings。在osx的Terminal.app裏，是直接可以在Preferences裏強制將osx的`alt/option`mapping成`meta`，這樣就可以享受`m-f`和`m-b`等快捷鍵了。

但是在osx的其他地方，這兩個binding就不一定好用了，對於已經習慣了的我來講很痛苦。剛好上週Runo告訴我了一個辦法：

分三步：

1. `mkdir ~/Library/KeyBindings`
2. `touch ~/Library/KeyBindings/DefaultKeyBinding.dict`
3. 編輯該文件，內容如下：
{% highlight json %}
    {
      "~b" = moveWordBackward:;
      "~f" = moveWordForward:;
    }
{% endhighlight %}
當然，您也可以繼續mapping像`m-v`、`c-v`等快捷鍵的。
