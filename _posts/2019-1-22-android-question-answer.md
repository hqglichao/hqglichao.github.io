---
layout: post
title: Answer Of Android Related Question
date: 2019-1-22 20:15:00 +0800
categories: Android
tag: [Answer, Question]
---

* content
{:toc}

transformDexArchiveWithDexMergerForRelease
==============================================
编译时报包合并出错，一般是有类名的路径重复了。比如在两个地方同时出现类`a.b.Utils`，且包名是一样的。  
可以在这行内容下面找到重复路径类名，`Task :app:transformDexArchiveWithDexMergerForRelease FAILED`。  
报错信息如下：
```bash
java.lang.RuntimeException: com.android.builder.dexing.DexArchiveMergerException: Error while merging dex archives:  
at java.util.concurrent.ForkJoinTask$AdaptedCallable.exec(ForkJoinTask.java:1431)  
at java.util.concurrent.ForkJoinTask.doExec(ForkJoinTask.java:289)
at java.util.concurrent.ForkJoinTask.externalAwaitDone(ForkJoinTask.java:326)
at java.util.concurrent.ForkJoinTask.doJoin(ForkJoinTask.java:391)
at java.util.concurrent.ForkJoinTask.join(ForkJoinTask.java:719)
at com.google.common.collect.ImmutableList.forEach(ImmutableList.java:397)
at com.android.build.gradle.internal.transforms.DexMergerTransform.transform(DexMergerTransform.java:221)
at com.android.build.gradle.internal.pipeline.TransformTask$2.call(TransformTask.java:221)
at com.android.build.gradle.internal.pipeline.TransformTask$2.call(TransformTask.java:217)
at com.android.builder.profile.ThreadRecorder.record(ThreadRecorder.java:102)
......
```

UnsatisfiedLinkError
==============================================
调用`System.loadLibrary("***");`时，如果找不到相应的`lib***.so`文件，就会报这个错误。  
首先要确保自己的`so`文件正确的放在`libs`文件夹下面的`armeabi`等目录下。  
其次要确保自己的`so`文件打包进自己的`apk`（把`apk`解压了看看）
如果`apk`里面没有打包进`so`文件，需要在`build.gradle`中添加：
```bash
android {
    compileSdkVersion 28
    ...
    //添加如下代码
    sourceSets {
        main {
            jniLibs.srcDirs = ['libs']
        }
    }
}
```

这个错误报错代码如下：
```bash
java.lang.UnsatisfiedLinkError: dalvik.system.PathClassLoader[DexPathList[[zip file...
at java.lang.Runtime.loadLibrary(Runtime.java:379)
at java.lang.System.loadLibrary(System.java:1086)
...
```


google()找不到
===========================================
Could not find method google() for arguments [] on repository container
这个是因为`gradle-wrapper.properties`这个文件夹里面设置的Gradle版本太低了，这个google库简单支持是从Gradle4.1开始的，可以升级Gradle版本，或者替换成：
```bash
maven {
    url 'https://maven.google.com'
}
```

Minimum supported Gradle version
===============================================
Minimum supported Gradle version is 3.3. Current version is 2.14.1.
一定是Gradle的版本和classpath里面的tools.build版本不匹配了，2.14.1最高支持2.2.0版的工具。  
对应关系如下链接：[https://developer.android.com/studio/releases/gradle-plugin](https://developer.android.com/studio/releases/gradle-plugin)