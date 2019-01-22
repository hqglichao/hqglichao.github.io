---
layout: post
title: Android Architecture Components Room
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


