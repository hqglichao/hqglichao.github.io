---
layout: post
title: Android 小Tip
date: 2018-12-11 17:16:00 +0800
categories: Android
tag: [View, TouchEvent]
---

* content
{:toc}


View 点击不穿透                       {#View}
====================================

如果有多层view叠加时，要使点击最上层view不穿透到下一层view。必需要让上层View消耗掉点击事件，也就是在`OnTouchEvent`中`return true`。  
因为这样所有`Touch`事件就都有点击的这个view处理了，不会上传到`parentVew`。所以点击就不会穿透到`parentView`了。  
`xml`代码如下：  
```bash
android:clickable="true"
```