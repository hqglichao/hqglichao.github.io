---
layout: post
title: build.gradle 的断点调试
date: 2018-12-18 20:28:00 +0800
categories: Gradle
tag: Debug
---
* content
{:toc}


build.gradle debug                     
=========================================

`android studio`开发的`Android`项目，`build.gradle`不可或缺。
编写过程中和app代码一样进行断点调试该怎么做呢。

1、 Run->Edit Configrations  
2、 点击添加+  
![点击添加示意图](https://raw.githubusercontent.com/hqglichao/hqglichao.github.io/master/styles/images/gradle-debug-add.png)  
3、 点击确认OK  
4、 ./gradlew aR -Dorg.gradle.debug=true  --no-daemon （会进入等待模式）  
5、 点击debug按钮，attach调试  

gradle 的调试
=========================================

在代码左边带运行的`icon`上点击，填写参数，在`jvm`那栏写入  
```bash
-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=1044
```