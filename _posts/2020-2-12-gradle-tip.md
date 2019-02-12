---
layout: post
title: DataBinding 入门简介
date: 2019-1-31 20:25:00 +0800
categories: DataBinding
tag: [DataBinding]
---

* content
{:toc}


DataBinding使用准备
=======================================
在`app`的`build.gradle`里面添加
```
android {
    ...
    dataBinding {
        enabled = true
    }
}
```
某些版本`android studio`需要在`gradle.properties`文件中添加
```bash
android.databinding.enableV2=true
```
有了这些sync一下就可以开始使用了


