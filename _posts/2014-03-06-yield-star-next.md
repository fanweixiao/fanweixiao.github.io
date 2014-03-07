---
published: false
layout: post
category: koajs
tags: 
  - node
---

## Koa中yield next vs yield* next

delegate generator的一个作用就是解决`yield (function*(){yield 1;})`这样的问题，使用`yield *`后上面的例子就等价于`yield 1`了。而在[Koa](https://github.com/koajs)里[co](https://github.com/visionmedia/co)是灵魂，尽可能的使用`yield *`会避免overhead的co包装。

参考：
[harmony proposal](http://wiki.ecmascript.org/doku.php?id=harmony:generators#delegating_yield)
