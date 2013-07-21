---
published: true
layout: post
category: ecmascript
tags: 
  - ecmascript
---

[Draft ECMAScript 6 Specification](http://wiki.ecmascript.org/doku.php?id=harmony:direct_proxies)最近新增了對Proxies節點的定義，在之前的specification drafts裏這一節是空的。而這次改版也替換了之前的API。

### 使用方式

{% highlight javascript %}
var proxy = Proxy(target, handler);
{% endhighlight %}


要注意這裏的聲明方式，不再是`Proxy.create`了

### 舉個簡單的小例子

{% highlight javascript %}
var obj = { name: 'WoW' };
 
var hdl = {
  set: function (receiver, property, value) {
    console.log(property, '->', value);
    receiver[property] = value;
  }
};
 
obj = Proxy(obj, hdl);

obj.name = 'C.C.';

// will prints
name -> C.C.

{% endhighlight %}


`set`只是其中的一個方法，更多的可以[參考wiki](http://wiki.ecmascript.org/doku.php?id=harmony:direct_proxies)

