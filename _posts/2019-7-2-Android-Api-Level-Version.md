---
layout: post
title: Android 平台代号/版本/Api级别/库
date: 2019-7-2 17:23:00 +0800
categories: Android
tag: [Android]
---

* content
{:toc}

Android官方库版本查询
=======================================
[https://mvnrepository.com/artifact/com.android.support/support-compat](https://mvnrepository.com/artifact/com.android.support/support-compat)  
  


api级别官方描述地址
=======================================
[https://source.android.com/setup/start/build-numbers](https://source.android.com/setup/start/build-numbers) 

| 代号               | 版本          | API 级别/NDK 版本  |
|--------------------|---------------|-------------------|
| Android12L         | 12            | API 级别 32        |  
| Android12          | 12            | API 级别 31        |  
| Android11          | 11            | API 级别 30        |  
| Android10          | 10            | API 级别 29        |  
| Pie                | 9             | API 级别 28        |  
| Oreo               | 8.1.0         | API 级别 27        |  
| Oreo               | 8.0.0         | API 级别 26        |  
| Nougat             | 7.1           | API 级别 25        |  
| Nougat             | 7.0           | API 级别 24        |  
| Marshmallow        | 6.0           | API 级别 23        |  
| Lollipop           | 5.1           | API 级别 22        |  
| Lollipop           | 5.0           | API 级别 21        |  
| KitKat             | 4.4 - 4.4.4   | API 级别 19        |  
| Jelly Bean         | 4.3.x         | API 级别 18        |  
| Jelly Bean         | 4.2.x         | API 级别 17        |  
| Jelly Bean         | 4.1.x         | API 级别 16        |  
| Ice Cream Sandwich | 4.0.3 - 4.0.4 | API 级别 15，NDK 8 |  
| Ice Cream Sandwich | 4.0.1 - 4.0.2 | API 级别 14，NDK 7 |  
| Honeycomb          | 3.2.x         | API 级别 13        |  
| Honeycomb          | 3.1           | API 级别 12，NDK 6 |  
| Honeycomb          | 3.0           | API 级别 11        |  
| Gingerbread        | 2.3.3 - 2.3.7 | API 级别 10        |  
| Gingerbread        | 2.3 - 2.3.2   | API 级别 9，NDK 5  |  
| Froyo              | 2.2.x         | API 级别 8，NDK 4  |  
| Eclair             | 2.1           | API 级别 7，NDK 3  |  
| Eclair             | 2.0.1         | API 级别 6         |  
| Eclair             | 2.0           | API 级别 5         |  
| Donut              | 1.6           | API 级别 4，NDK 2  |  
| Cupcake            | 1.5           | API 级别 3，NDK 1  |  
| （无代号）         | 1.1           | API 级别 2         |  
| （无代号）         | 1.0           | API 级别 1         |  

gradle版本和插件版本的对应关系
=======================================

### gradle 下载的存放地址
```
/Users/hqglichao/.gradle/wrapper/dists
```

### 插件版本和gradle版本对应关系
[https://developer.android.com/studio/releases/gradle-plugin?hl=zh-cn](https://developer.android.com/studio/releases/gradle-plugin?hl=zh-cn)  

| 插件版本      | 所需的 Gradle 版本 |
| ------------- | ------------------ |
| 1.0.0 - 1.1.3 | 2.2.1 - 2.3        |
| 1.2.0 - 1.3.1 | 2.2.1 - 2.9        |
| 1.5.0         | 2.2.1 - 2.13       |
| 2.0.0 - 2.1.2 | 2.10 - 2.13        |
| 2.1.3 - 2.2.3 | 2.14.1 - 3.5       |
| 2.3.0+        | 3.3+               |
| 3.0.0+        | 4.1+               |
| 3.1.0+        | 4.4+               |
| 3.2.0 - 3.2.1 | 4.6+               |
| 3.3.0 - 3.3.3 | 4.10.1+            |
| 3.4.0 - 3.4.3 | 5.1.1+             |
| 3.5.0 - 3.5.4 | 5.4.1+             |
| 3.6.0 - 3.6.4 | 5.6.4+             |
| 4.0.0+        | 6.1.1+             |
| 4.1.0+        | 6.5+               |
| 4.2.0+        | 6.7.1+             |
| 7.0           | 7.0+               |
| 7.1           | 7.2+               |
| 7.2           | 7.3.3+             |
