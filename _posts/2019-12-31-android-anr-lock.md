---
layout: post
title: Android ANR 的实例分析
date: 2019-12-31 20:12:00 +0800
categories: 技术文章
tag: [Android ANR]
---

* content
{:toc}

# trace文件获取
标准做法是`adb pull /data/anr/traces.txt`。  
如果碰到没有权限的情况，用`adb bugreport`生成，然后adb pull生成目录下的文件出来，解压会得到一个`bugreport-***.txt`的文件，这个文件里面就有trace信息，文件里面搜`VM TRACES AT LAST ANR`定位最后一次anr的trace信息。
# 主线程忙等
看第一个`ANR`的trace，如下图  

![ANR的trace](https://picgo-1307686581.cos.ap-shanghai.myqcloud.com/github/hqglichao/imagesandroid-anr.png)    

上图可以看出UI线程`lock 0x0799e5b8`然后又在`wait 0x0799e5b8`，是一个Object调用wait后等待notfiy。`ANR`发生的时候，UI线程正在等这个notify，但是没有等到，这个时候在运行的是`at com.a.b.c.d(c.java:746)`，可以得出结论是这个里面执行了一个耗时操作。  
# 死锁
下面看一个死锁的trace  

![ANR的trace2](https://picgo-1307686581.cos.ap-shanghai.myqcloud.com/github/hqglichao/imagesandroid-anr2.png)   

上图可以比较容易看出线程`Thread-0`锁住了`<0x00000007d56540a0>`在等`<0x00000007d5656180>`。而`Thread-1`锁住了`<0x00000007d5656180>`在等`<0x00000007d56540a0>`，所以这两个线程就造成了死锁。