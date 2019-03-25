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

编译阶段调试
=========================================
`android studio`编译阶段的调试。
一般调试都是编译结束程序运行后的调试，如果想调试一些编译时的过程，比如`Annotation Processor`
```bash
./gradlew --no-daemon -Dorg.gradle.debug=true :app:clean :app:compileDebugJavaWithJavac
```
然后创建一个`Remote`的编译调试，然后点击`debug`按钮，attach调试。
>参考：[https://blog.xmartlabs.com/2016/03/28/Debugging-an-Annotator-Processor-in-your-project/](https://blog.xmartlabs.com/2016/03/28/Debugging-an-Annotator-Processor-in-your-project/)