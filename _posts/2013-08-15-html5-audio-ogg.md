---
layout: post
category : html5
tags : 
  - ogg
  - X-Content-Duration
---
{% include JB/setup %}

## 問題

Firefox對HTML5 Audio不支持mpeg編碼，一般的做法是選擇[Ogg]格式。而如果server端要支持[Ogg]格式的內容，需要注意一個地方：[Ogg]格式本身是不包含media文件的duration信息的，所以當firefox加載[Ogg]文件的時候，如果response裏沒有[X-Content-Duration]，那firefox需要多次訪問文件（當然前提是支持[Range Request](http://tools.ietf.org/html/draft-ietf-httpbis-p5-range-23)）來determine這個media文件的duration內容，如下圖所示：

![w/o X-Content-Duration](http://niting.qiniudn.com/Screen%20Shot%202013-08-15%20at%203.42.45%20PM.png)


## X-Content-Duration

Response headers裏如果有了它之後，請求狀況如下圖所示：

![w/ X-Content-Duration](http://niting.qiniudn.com/Screen%20Shot%202013-08-15%20at%203.42.16%20PM.png)

firefox的整個步驟在這裏有一個詳細的解釋：
[Bug 502894 - HTTP byte range requests for Ogg/Theora video streams make no sense](https://bugzilla.mozilla.org/show_bug.cgi?id=502894#c1)

## 七牛

七牛CDN在國內現在算是最讚的了，比如支持很酷的轉碼方案：

對於一個MP3文件：`http://niting.qiniudn.com/Jules%20Massenet%20-%20Meditation.mp3`，如果需要使用[Ogg]格式的，只需要在後面添加：`?avthumb/ogg/acodec/libvorbis/ab/192k`即可。

但是缺點就是Respones Headers是不包含[X-Content-Duration]的… what a pitty :(


[Ogg]: http://en.wikipedia.org/wiki/Ogg "Ogg"
[X-Content-Duration]: https://developer.mozilla.org/en-US/docs/Configuring_servers_for_Ogg_media "Configuring Servers for Ogg media"