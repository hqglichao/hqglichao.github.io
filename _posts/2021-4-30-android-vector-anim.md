---
layout: post
title: Android AnimatedVectorDrawable实现简单动画
date: 2021-4-30 21:30:00 +0800
categories: 技术文章
tag: [Android vector Animation AnimatedVectorDrawable]
---


## 动画效果简介  
我们在Android App开发过程中，动画是经常接触的，实现动画的方式也多种多样。比如AnimationSet/Animation/Lottie等。
用AnimationSet可以实现一些简单组合动画，也可以自定义View在onDraw函数里面画各种花样。但自定义View的代码量和复杂度都不足以让一个想偷懒的Coder有任何偷懒的机会。写代码久了后，给一个动画后常常会想有没有更简洁的方式来实现一个设计要求的动画，而且又不让动画过于侵入我们的业务代码。  
下面要介绍的是用**AnimatedVectorDrawable**来实现的，圆形渐变矩形的一种矢量动画。
先来看下实现的动画效果，如下所示  

<img src="https://github.com/hqglichao/hqglichao.github.io/raw/master/styles/gif/vector_anim.gif" height="400" width="200"/>  

动画本身不难，就是一个从小圆变成大圆透明度逐渐增大，到一定程度后拉伸成矩形的一个过程。  
我们可以用AnimatedVectorDrawable来实现这个矢量动画，但并不需要亲自写这个xml文件，有个网站可以实现这个矢量动画的在线编辑和导出xml。  
网站地址：https://shapeshifter.design/  

### 1. 画svg矢量图 
首先简单介绍一下这个怎么画一个矢量图，如下表所述
|  指令                  |  详情  |
|  ----                 | ----  |
|  M x,y                | 开始一个新路径，并移动到起点 x,y |
|  L x,y                | 画直线，终点（x,y） |
|  C x1,y1 x2,y2 x,y    | 画贝塞尔曲线，终点（x,y），控制点（x1,y1）和（x2,y2）
|  Z                    | 画一条线到起点，闭合当前线路| 
可以用贝塞尔曲线来拟合圆，可以看下图用贝塞尔曲线来拟合一个圆：  
<img src="https://github.com/hqglichao/hqglichao.github.io/raw/master/styles/images/svg_round.png"/>  

### 2. shapeshifter网站生成xml
简单介绍一下[shapeshifter](https://shapeshifter.design/)的用法  
<img src="https://github.com/hqglichao/hqglichao.github.io/raw/master/styles/images/shapeshifter.png"/>    
shapeshifter的界面上图所示，就我们这个动画而言，左边的层级结构里有一个group，group下面有个path，path里面有两个属性`pathData`和`fillAlpha`。选中pathData可以在右上属性编辑区，编辑开始和结束的svg路径，再指定一个过渡动画，就可以实现开始到结束的动画。给`fillAlpha`指定属性，在动画编辑区，通过调整轨道时间，就可以实现形状透明度的混合叠加动画。  
此外path还支持strokeColoe等其他属性。  
我们这个动画可以分成两个部分
* 第一个部分从0到一个圆的变化  
    ```bash
    fromValue: M 19 19 C 19 19 19 19 19 19 C 19 19 19 19 19 19 L 19 19 C 19 19 19 19 19 19 C 19 19 19 19 19 19
    toValue: M 19 0 C 8.55 0 0 8.55 0 19 C 0 29.45 8.55 38 19 38 L 19 38 C 29.45 38 38 29.45 38 19 C 38 8.55 29.45 0 19 0
    ```
* 第二个部分从圆到矩形带倒角的形状
    ```bash
    fromValue: M 19 0 C 8.55 0 0 8.55 0 19 C 0 29.45 8.55 38 19 38 L 19 38 C 29.45 38 38 29.45 38 19 C 38 8.55 29.45 0 19 0
    toValue: M 19 0 C 8.55 0 0 8.55 0 19 C 0 29.45 8.55 38 19 38 L 97 38 C 107.45 38 116 29.45 116 19 C 116 8.5 107.45 0 97 0
    ```
编辑完成直接导出`Animated Vector Drawable`

### 3. Android接入动画
接入动画这块就比较简单了，直接把上一步导出的xml，当成一个drawable来用就可以了  
* 布局
    ```xml
    <FrameLayout
        android:id="@+id/flScaleContainer"
        android:layout_marginLeft="200dp"
        android:layout_marginTop="100dp"
        android:layout_centerInParent="true"
        android:background="@drawable/round_to_rectangle_entrance_bg"
        android:layout_width="200dp"
        android:layout_height="50dp">
    </FrameLayout>
    ```
* 使用
    ```kotlin
    (view.background as AnimatedVectorDrawable).start()
    ```
至此，我们用比较简单的业务代码，就实现了一个形状变化动画。后续的东动画的编辑也不会对现有代码结构有影响，而且后续迭代微调的代价也比较小。

## 附录：
完整动画xml文件
```xml
<animated-vector xmlns:tools="http://schemas.android.com/tools"
  xmlns:android="http://schemas.android.com/apk/res/android"
  xmlns:aapt="http://schemas.android.com/aapt"
  tools:ignore="NewApi">
  <aapt:attr name="android:drawable">
    <vector
      android:name="vector"
      android:width="116dp"
      android:height="38dp"
      android:viewportWidth="116"
      android:viewportHeight="38">
      <group android:name="group"/>
      <path
        android:name="path"
        android:pathData="M 19 0 C 8.55 0 0 8.55 0 19 C 0 29.45 8.55 38 19 38 L 19 38 C 29.45 38 38 29.45 38 19 C 38 8.55 29.45 0 19 0"
        android:fillColor="#80242628" />
    </vector>
  </aapt:attr>
  <target android:name="path">
    <aapt:attr name="android:animation">
      <set>
        <objectAnimator
          android:propertyName="pathData"
          android:duration="240"
          android:valueFrom="M 19 19 C 19 19 19 19 19 19 C 19 19 19 19 19 19 L 19 19 C 19 19 19 19 19 19 C 19 19 19 19 19 19"
          android:valueTo="M 19 0 C 8.55 0 0 8.55 0 19 C 0 29.45 8.55 38 19 38 L 19 38 C 29.45 38 38 29.45 38 19 C 38 8.55 29.45 0 19 0"
          android:valueType="pathType"
          android:interpolator="@android:anim/accelerate_interpolator"/>
        <objectAnimator
          android:propertyName="fillAlpha"
          android:duration="240"
          android:valueFrom="0"
          android:valueTo="0.1"
          android:valueType="floatType"
          android:interpolator="@android:anim/accelerate_interpolator"/>
        <objectAnimator
          android:propertyName="pathData"
          android:startOffset="240"
          android:duration="240"
          android:valueFrom="M 19 0 C 8.55 0 0 8.55 0 19 C 0 29.45 8.55 38 19 38 L 19 38 C 29.45 38 38 29.45 38 19 C 38 8.55 29.45 0 19 0"
          android:valueTo="M 19 0 C 8.55 0 0 8.55 0 19 C 0 29.45 8.55 38 19 38 L 97 38 C 107.45 38 116 29.45 116 19 C 116 8.5 107.45 0 97 0"
          android:valueType="pathType"
          android:interpolator="@android:interpolator/fast_out_slow_in"/>
        <objectAnimator
          android:propertyName="fillAlpha"
          android:startOffset="240"
          android:duration="240"
          android:valueFrom="0.1"
          android:valueTo="1"
          android:valueType="floatType"
          android:interpolator="@android:anim/decelerate_interpolator"/>
      </set>
    </aapt:attr>
  </target>
</animated-vector>

```

> 参考文献：  
> 1. https://www.androiddesignpatterns.com/2016/11/introduction-to-icon-animation-techniques.html#drawing-paths