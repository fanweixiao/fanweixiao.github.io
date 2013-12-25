---
published: false
layout: post
category: linux
tags: 
  - redis
  - centos
---

在ArchLinux上，我使用`Ubuntu Mono`字体，但是效果不好，字体发虚，要解决这个其实很简单：

创建`~/.fonts.conf`，内容如下：

{% highlight xml %}
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
<match target="font">
   <edit name="hinting" mode="assign">
      <bool>true</bool>
   </edit>
</match>

<match target="font">
   <edit name="hintstyle" mode="assign">
         <const>hintslight</const>
    </edit>
</match>

</fontconfig>
{% endhighlight %}


