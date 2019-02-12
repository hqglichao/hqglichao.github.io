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

简单使用介绍
=======================================
`databinding`的好处是数据和`view`绑定在一起的，做好前期关联工作后，我们只需要关心对数据的更新即可。数据更新后，底层代码会主动调用`notify`通知`view`数据变化了（_观察者模式，此时View相当于数据的一个观察者_），`view`的显示也就更新了。  
一般是一个`xml`布局关联一个特定的`ViewModel`对象，`ViewModel`对象控制着数据的改变。  
`ViewModel`可以是继承`BaseObservable`，在数据更新时调用`notifyPropertyChanged(BR.specificName)`。或者不需要继承类，通过使用`ObservableField`之类的数据，在数据变化时，调用`.set`之类的方法实现。这是两种方式，第二种方式比较灵活，因为不需要继承任何父类，这样可以有继承其他类的可能性。