---
published: false
layout: post
category: linux
tags: 
  - redis
  - centos
---

## 按照官方安装指导
```
$ wget http://download.redis.io/releases/redis-2.6.16.tar.gz
$ tar xzf redis-2.6.16.tar.gz
$ cd redis-2.6.16
$ make
```
执行会遇到错误

```
[cc@ntdev2 redis-2.6.16]$ make -j8
cd src && make all
make[1]: Entering directory `/root/redis-2.6.16/src'
    CC adlist.o
    CC ae.o
    CC anet.o
    CC dict.o
In file included from adlist.c:34:
zmalloc.h:50:31: error: jemalloc/jemalloc.h: No such file or directory
zmalloc.h:55:2: error: #error "Newer version of jemalloc required"
make[1]: *** [adlist.o] Error 1
make[1]: *** Waiting for unfinished jobs....
In file included from ae.c:44:
zmalloc.h:50:31: error: jemalloc/jemalloc.h: No such file or directory
zmalloc.h:55:2: error: #error "Newer version of jemalloc required"
In file included from dict.c:47:
zmalloc.h:50:31: error: jemalloc/jemalloc.h: No such file or directory
zmalloc.h:55:2: error: #error "Newer version of jemalloc required"
make[1]: *** [ae.o] Error 1
make[1]: *** [dict.o] Error 1
make[1]: Leaving directory `/root/redis-2.6.16/src'
make: *** [all] Error 2
[cc@ntdev2 redis-2.6.16]$ 
```
看错误是`jemalloc`没有所以导致中断，解决办法是到redis的`deps`里装上就可以了，还有其他几个依赖：

`cd deps && make hiredis lua jemalloc linenoise`

然后再回到`src`目录执行`make`就可以了，当然`gcc`是必须的，如果要`make test`记得`yum install tcl`