---
layout: post
title: Android NinePatch
date: 2020-1-3 10:58:00 +0800
categories: 技术文章
tag: [Android 9-patch]
---

* content
{:toc}

# NinePatch 四周黑线含义  
9图四周有中间一个图+上下左右四条黑线构成，`通过左侧和顶部的线定义可拉伸区域，通过底部和右侧的线定义可绘制区域`。可拉伸区域会通过拉伸来确保绘制区域能放下需要绘制的内容。  

下图是一个比较典型的9图示例：  
<img src="https://picgo-1307686581.cos.ap-shanghai.myqcloud.com/github/hqglichao/imagesninepatch_raw.png" style="zoom:60%;"/> 


填充内容后：  

![9图填充内容后](https://picgo-1307686581.cos.ap-shanghai.myqcloud.com/github/hqglichao/imagesninepatch_examples.png)     

-------------  
>参考文章：[https://developer.android.com/guide/topics/graphics/drawables#nine-patch](https://developer.android.com/guide/topics/graphics/drawables#nine-patch)