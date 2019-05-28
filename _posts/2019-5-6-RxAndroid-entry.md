---
layout: post
title: RxAndroid 入门简介
date: 2019-5-6 17:06:00 +0800
categories: RxAndroid
tag: [RxAndroid]
---

* content
{:toc}


RxAndroid 使用准备
=======================================
所需依赖：  
// RxJava  
implementation ‘io.reactivex.rxjava2:rxjava:2.1.13’  
// RxAndroid  
implementation ‘io.reactivex.rxjava2:rxandroid:2.0.2’  
// Retrofit  
implementation ‘com.squareup.retrofit2:retrofit:2.3.0’  
implementation ‘com.squareup.retrofit2:converter-gson:2.3.0’  
implementation ‘com.squareup.okhttp3:logging-interceptor:3.4.1’  
implementation ‘com.squareup.retrofit2:converter-scalars:2.1.0’   
// RxAndroid + Retrofit
implementation ‘com.squareup.retrofit2:adapter-rxjava2:2.4.0’

基本知识
=======================================
RxAndroid像是一种`RxJava`的Android实现方式。所以了解RxAndroid要从了解RxJava下手。  
RxJava有两个比较关键的概念：Observable和Observer。    
`Observable`：处理一些耗时操作，释放一些处理结果的value。  
`Observer`：获取Observable释放的value。  
  
Observables的类型有：  
* Observable  
* Flowable  
* Single 
* Maybe
* Completable 
 
Observers的类型有：
* Observer
* SingleObserver
* MaybeObserver
* CompletableObserver
  
简单来说，Observable可以发出多个value，Flowable是当Observable发出大量无法被Observer消耗的value时采用。两种都可以使用普通的Observer。Single只能发出一个value，像一个网络请求的返回。Maybe可以发出一个或不发出value。Completable用于不发出值的任务。详情见[这里](https://blog.mindorks.com/understanding-types-of-observables-in-rxjava-6c3a2d0819c8)。  

RxJava中的线程是在依赖于Schedulers的。Scheduler可以想象成控制一个或多个线程的线程池。当需要执行任务的时候，Scheduler就从对应的线程池中取出线程来执行任务。  
`Schedulers`的种类及通常用法：  
1. Schedulers.io()没有线程数量限制，用于处理非cpu密集型操作，如网络请求IO和数据库操作等。
2. Schedulers.computation() 线程数量由处理器数量限制   
3. Schedulers.newThread() 每个任务都会有单独的线程，没有线程的重复利用，开销大。
4. Schedulers.from(Executor executor) 用自定义的线程池。
5. AndroidSchedulers.mainThread() android中的所谓UI线程，勿做耗时操作。
6. Schedulers.single() 单独一个线程顺序处理请求。
7. Schedulers.trampoline()

小心使用非限制线程数量的Scheduler，Schedulers.io()和Schedulers.newThread(),使用不当，线程泛滥。  

Let's Code
=======================================


subscribeOn()提交运行的线程，多个只有第一个有效，除非是在flatmap里面。  
map优化返回结果[https://medium.com/@mtrax/rxandroid-2-with-retrofit-2-and-gson-3f08d4c2627d](https://medium.com/@mtrax/rxandroid-2-with-retrofit-2-and-gson-3f08d4c2627d)
常用Schedulers.io()等