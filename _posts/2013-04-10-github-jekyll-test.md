---
layout: post
category : test
tagline: "只是一个测试"
tags : [test, jekyll]
---
{% include JB/setup %}

This Jekyll introduction will outline specifically  what Jekyll is and why you would want to use it.
Directly following the intro we'll learn exactly _how_ Jekyll does what it does.

## Moving

### Why github

Just because of github is awesome

### Why does Jekyll Do?

Generates my blog site

{% capture text %}...
<body>
  <div id="sidebar"> ... </div>
  <div id="main">
    |.{content}.|
  </div>
</body>
...{% endcapture %}
{% include JB/liquid_raw %}