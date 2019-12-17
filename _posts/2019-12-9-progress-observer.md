---
layout: post
title: Android 平台代号/版本/Api级别/库
date: 2019-12-9 21:12:00 +0800
categories: 技术文章
tag: [观察者模式]
---

* content
{:toc}

# 下载UI和下载逻辑解耦
移动端编程中经常会用到ViewPager，容纳很多数据每个页面可能又是一个RecyclerView。如果RecyclerView的每个item想要有一个下载进度条，并且这个进度条要有记忆逻辑，就是item回收或者复用之后回来，状态能根据下载的进度实时更新显示，这个问题就有点烦了。
我的解决方案是把每个item当成一个observer，