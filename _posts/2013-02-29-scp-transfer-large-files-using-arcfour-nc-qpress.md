---
layout: post
category : linux
tags : [ssh, linux, scp, nc, qpress, arcfour]
---
{% include JB/setup %}

不追求複雜高效和數據安全，這裏列舉兩種簡單好記的有效辦法（下面的描述都是從server A傳輸文件到server B）

# 可以開端口的話，netcat+qpress方式 #

原理是在A上使用[qpress](http://www.quicklz.com/)壓縮源數據，再通過[netcat](http://en.wikipedia.org/wiki/Netcat)傳輸到B，然後再解壓縮。

qpress可以在這裏[下載](http://www.quicklz.com/qpress-11-linux-x64.tar)，記得在A和B上都要有它，比如放在`/usr/bin/`下。而netcat一般系統自帶了，叫nc。

操作可以都在A上完成，步驟如下：

	ssh fanweixiao@server_B 'nc -l 12345 | qpress -dio > /tmp/db.bak.sql' &

+ `ssh 用戶名@服務器IP '要執行的命令'` 可以在遠程機器上執行命令
+ 最後的`&`直接把該命令扔到後臺，這樣我們就可以立即執行後面的命令
+ `nc -l 12345 |` 在B上監聽port 12345，並把接受的數據pipe給後面的命令
+ `qpress -dio > /tmp/db.bak.sql` 以stdin方式接受到nc傳來的數據，然後解壓縮。`d`表示decompress，`i`表示read from stdin，`o`表示write to stdout， `> /tmp/db.bak.sql`表示將stdout重定向到該文件

該命令執行完成后，服務器B就會在等待接受了。現在開始壓縮、傳輸：

	time qpress -o db.bak.sql | nc server_B 12345

+ `time command` 是查看某個命令執行的時長
+ `qpress -o db.bak.sql |` 是用qpress壓縮db.bak.sql這個文件，`-o`表示將壓縮后的結果輸出到stdout，然後pipe給後面的命令
+ `nc server_B 12345` 表示從stdin讀入數據，傳輸到server_B的12345端口

可以在服務器B上執行`watch ls -alh /tmp/db.bak.sql`來查看傳輸情況

# 使用SSH通道傳輸 #

使用SSH通道的好處是不用開端口，尤其是在開發環境與生產環境的跳板機傳輸內容會方便很多。最常用的方式是scp命令，比如可能我們平時這樣用：

	scp db.tgz fanweixiao@server_B:/tmp/.

在傳輸大文件時可能會發現速度非常慢，[更換一下ssh過程中的cipher使用，比如選擇blowfish或者arc4](http://blog.famzah.net/2010/06/11/openssh-ciphers-performance-benchmark/)，速度就有很明顯的提升。綜合一下壓縮、解壓縮的步驟，一條命令完成：

	time tar -czf - db.bak.sql | ssh -c arcfour128 fanweixiao@server_B 'tar -xzf - -C /tmp/.'

不再詳細解釋命令，需要主要的地方就是`ssh的-c參數`表示指定選用哪種cipher，這裏我們選用了`arcfour128`。如果有時間，您也可以測試一下`arcfour`和`blowfish`的效果。

# 變態的做法 #

這個mission就分爲兩個步驟

- 壓縮、解壓縮
- 傳輸

極致的做法就是根據硬件環境選擇合適的搭配，讓整體時間降到最低。如果兩臺server之間的帶寬足夠高，那壓縮比差不多的情況下一定要選擇速度更快的。但是這裏一定要注意一點，切不可追求極致而擴大side effect：服務器A和B的計算和帶寬資源的損耗千萬不要影響了核心業務，那就得不償失了。
