---
layout: post
title: gradle 学习笔记
date: 2019-2-12 15:20:00 +0800
categories: gradle
tag: [Gradle, Tip]
---

* content
{:toc}


gradle学习
=======================================
先安装`gradle`，确保本地`JAVA_HOME`对应`java`版本高于8。  
>参考：[https://docs.gradle.org/current/userguide/installation.html#installing_gradle](https://docs.gradle.org/current/userguide/installation.html#installing_gradle)



## configure()
The [configure()](https://docs.gradle.org/5.2.1/userguide/multi_project_builds.html#ssub:filtering_by_name) method takes a list as an argument and applies the configuration to the projects in this list.  
### [Subproject configuration](https://docs.gradle.org/5.2.1/userguide/multi_project_builds.html#sec:subproject_configuration)


## dependency locking 
优先使用已经解析的版本，这个在使用动态依赖的时候有用处，不至于不能复现bug，频繁更新导致的不稳定。

## Customizing dependency
几种不同的实现更改`dependency`的方式  [https://docs.gradle.org/5.2.1/userguide/customizing_dependency_resolution_behavior.html](https://docs.gradle.org/5.2.1/userguide/customizing_dependency_resolution_behavior.html)