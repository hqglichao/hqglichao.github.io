---
layout: post
title: Android 使用TransitionDrawable的组合动画
date: 2021-3-12 14:58:00 +0800
categories: 技术文章
tag: [Android Xml Animation TransitionDrawable]
---


## 动画效果简介
我们在Android App开发过程中，动画是经常接触的，实现动画的方式也多种多样。比如AnimationSet/Animation/Lottie等。
这篇文章主要是想介绍一个圆形（或其他形状）的layout在放大的过程中，背景颜色也逐渐变化的一个动画。
先来看下要实现的动画效果  
<img src="https://github.com/hqglichao/hqglichao.github.io/raw/master/styles/gif/transition_animation.gif"/>  

话不多说，直接上代码  
**布局文件**
```xml
<FrameLayout
android:id="@+id/mRubbishLayout"
android:layout_width="50dp"
android:layout_height="50dp"
android:layout_marginTop="1dp"
android:background="@drawable/edit_rubbish_bg_transition"
android:layout_centerHorizontal="true">

//这个里面可以放各种图形
    <ImageView
    android:id="@+id/iv_rubbish"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_gravity="center"
    android:src="@drawable/qqstory_rubbish_pressed" />
</FrameLayout>
```

`edit_rubbish_bg_transition.xml`文件代码
```xml
<?xml version="1.0" encoding="utf-8"?>
<transition xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/edit_rubbish_bg_normal"/>
    <item android:drawable="@drawable/edit_rubbish_bg_selected"/>
</transition>
```

`edit_rubbish_bg_normal.xml`文件代码
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
  android:shape="oval">
    <solid android:color="#991D1D1D" />
</shape>
```
`edit_rubbish_bg_selected.xml`文件代码
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="oval">
    <solid android:color="#FFFB6155" />
</shape>
```

**动画实现代码**
```java
private void startRubbishScaleAlphaAnim() {
    TransitionDrawable transitionDrawable = (TransitionDrawable) mRubbishLayout.getBackground();
    transitionDrawable.startTransition(SCALE_ALPHA_DURATION); //这个会实现背景颜色渐变

    //这里会实现放大的动画
    ScaleAnimation scaleAnimation = new ScaleAnimation(1.0f, 1.2f, 1.0f, 1.2f, Animation.RELATIVE_TO_SELF, 0.5f, Animation.RELATIVE_TO_SELF, 0.5f);
    scaleAnimation.setDuration(SCALE_ALPHA_DURATION);
    scaleAnimation.setFillAfter(true);
    scaleAnimation.setInterpolator(new AccelerateDecelerateInterpolator());
    mRubbishLayout.startAnimation(scaleAnimation);
}
```
这样就能比较简单的实现，圆形Layout整体放大的同时背景颜色渐变的动画了
