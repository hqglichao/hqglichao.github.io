---
layout: post
title: View Layout Animation
date: 2019-4-9 17:08:00 +0800
categories: Android
tag: [View, Animation, Transition]
---

* content
{:toc}

>参考：  
1、[https://hencoder.com/ui-1-7/](https://hencoder.com/ui-1-7/)  
2、[https://developer.android.com/guide/topics/graphics/prop-animation#object-animator](https://developer.android.com/guide/topics/graphics/prop-animation#object-animator)   

ValueAnimator
==================================
下面代码原始值会从`startPropertyValue`到`endPropertyValue`间进行变化，变化时间为1秒。可以在`MyTypeEvaluator`返回改变后的属性值。
```bash
ValueAnimator animation = ValueAnimator.ofObject(new MyTypeEvaluator(), startPropertyValue, endPropertyValue);
animation.setDuration(1000);
animation.start();
```
此外还可以添加属性值监听：
```bash
animation.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
    @Override
    public void onAnimationUpdate(ValueAnimator updatedAnimation) {
        // You can use the animated value in a property that uses the
        // same type as the animation. In this case, you can use the
        // float value in the translationX property.
        float animatedValue = (float)updatedAnimation.getAnimatedValue();
        textView.setTranslationX(animatedValue);
    }
});
```

可以直接用连写的方式在同一个动画中改变多个属性值
```bash
view.animate()  
        .scaleX(1)
        .scaleY(1)
        .alpha(1);
```

ObjectAnimator  
===============================
直接指定要操作的`View`，和要操作的类型以及变化范围。自动完成动画，这个动画所需干预比较少。
```bash
ObjectAnimator animation = ObjectAnimator.ofFloat(textView, "translationX", 100f);
animation.setDuration(1000);
animation.start();
```
`ObjectAnimator`同样可以在一个动画中同时改变多个属性：
```bash
PropertyValuesHolder holder1 = PropertyValuesHolder.ofFloat("scaleX", 1);  
PropertyValuesHolder holder2 = PropertyValuesHolder.ofFloat("scaleY", 1);  
PropertyValuesHolder holder3 = PropertyValuesHolder.ofFloat("alpha", 1);

ObjectAnimator animator = ObjectAnimator.ofPropertyValuesHolder(view, holder1, holder2, holder3)  
animator.start();  
```

AnimatorSet
===============================
多个动画配合执行。之前的ObjectAnimator和ViewPropertyAnimator虽然可以同时执行多个动画，但是有些动画是有先后顺序配合执行的。这就要用到`AnimatorSet`了。
```bash
ObjectAnimator animator1 = ObjectAnimator.ofFloat(...);  
animator1.setInterpolator(new LinearInterpolator());  
ObjectAnimator animator2 = ObjectAnimator.ofInt(...);  
animator2.setInterpolator(new DecelerateInterpolator());

AnimatorSet animatorSet = new AnimatorSet();  
// 两个动画依次执行
animatorSet.playSequentially(animator1, animator2);  
animatorSet.start();  
```
当然，`AnimatorSet`也可以同时执行动画：
```bash
// 两个动画同时执行
animatorSet.playTogether(animator1, animator2);  
animatorSet.start();  
```
以及:
```bash
// 使用 AnimatorSet.play(animatorA).with/before/after(animatorB)
// 的方式来精确配置各个 Animator 之间的关系
animatorSet.play(animator1).with(animator2);  
animatorSet.play(animator1).before(animator2);  
animatorSet.play(animator1).after(animator2);  
animatorSet.start();  
```

PropertyValuesHolder
====================================
复杂动画详情见引用文章
```bash
// 在 0% 处开始
Keyframe keyframe1 = Keyframe.ofFloat(0, 0);  
// 时间经过 50% 的时候，动画完成度 100%
Keyframe keyframe2 = Keyframe.ofFloat(0.5f, 100);  
// 时间见过 100% 的时候，动画完成度倒退到 80%，即反弹 20%
Keyframe keyframe3 = Keyframe.ofFloat(1, 80);  
PropertyValuesHolder holder = PropertyValuesHolder.ofKeyframe("progress", keyframe1, keyframe2, keyframe3);

ObjectAnimator animator = ObjectAnimator.ofPropertyValuesHolder(view, holder);  
animator.start();  
```


Scene
====================================
不要把这个概念和`Transition`混淆，`Scene`其实可以理解为View树的快照。有两种方法进行Scene构建：  
1. Scene.getSceneForLayout(rootView, layoutId, context);
2. new Scene(rootView, childView);  

不同Scene可以通过`TransitionManager.go()`来进行切换，如果两个Scene中不同东西较多，可以`addTarget()`来指定要施加动画的对象。  
转场动画以之前动画的结尾帧为开始，如果没有指定可能就是当前的用户界面。  
`TransitionManager.beginDelayedTransition()`这个可以用于不与scene关联的转场动画，适合于当前view的设置visible等情景。  
可以在第二个参数传入的Transition中设置targetView施加动画，在view没有解析之前可以使用`transition.excludeChildren(layoutId, true)`，来排除指定id的view
在之后的变化中产生动画。

xml anim动效
====================================
可以在`.Project/res/anim/`下面编写动画效果，然后用loadAnimation加载，比如下面的xml：
```bash
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="300"
    android:startOffset="300">

    <translate
        android:fromYDelta="50%"
        android:toYDelta="0%" />
    <alpha android:fromAlpha="0"
        android:toAlpha="1"/>
</set>
```
`startOffset`为动画延迟开始时间。fromYDelta为开始动画位置起点为1/2*view.getHegiht() + view.getTop()。从完全透明的完全不透明。


xml transition/transitions动效
=============================================
res/transition/fade_transition.xml
```bash
<fade xmlns:android="http://schemas.android.com/apk/res/android" />
```
可以通过以下方式来解析：
```bash
Transition fadeTransition =
        TransitionInflater.from(this).
        inflateTransition(R.transition.fade_transition);
```
也可以同时设置一系列动效：
```bash
<transitionSet xmlns:android="http://schemas.android.com/apk/res/android"
    android:transitionOrdering="sequential">
    <fade android:fadingMode="fade_out" />
    <changeBounds />
    <fade android:fadingMode="fade_in" />
</transitionSet>
```
上面效果相当于`AutoTransition`，这个就是两个Scene切换过程中，后面少的view先`fade_out`，然后多的view先`changeBounds`(Moves and resizes views)，然后多的view再`fade_in`。