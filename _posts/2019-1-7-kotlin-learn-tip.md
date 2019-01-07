---
layout: post
title: Kotlin学习笔记
date: 2018-1-7 18:25:00 +0800
categories: Kotlin
tag: [Kotlin]
---

* content
{:toc}


init和constructor的不同
====================================
`Kotlin`中的数据初始化有好几种方式，其中调用的方式是不同的。  
### Initializer
* Property initializers
  ```bash
  var name: String = "David"
  ```
* Initializer blocks
  ```bash
  init {
    /* do some setup here */
  }
  ```
### Constructors
如下代码中constructor。
```bash
open class Parent {
    private val a = println("Parent.a")

    constructor(arg: Unit=println("Parent primary constructor default argument")) {
        println("Parent primary constructor")
    }

    constructor(arg: Int, arg2:Unit= println("Parent secondary constructor default argument")): this() {
        println("Parent secondary constructor")
    }

    init {
        println("Parent.init")
    }

    private val b = println("Parent.b")
}
```
### 执行顺序
先Initializer（从上到下）后constructor内部。如上例子，如果调用Parent(1),则输出如下：  
```bash
Parent secondary constructor default argument  
Parent primary constructor default argument  
Parent.a  
Parent.init  
Parent.b   
Parent primary constructor  
Parent secondary constructor  
```
>引用：[https://medium.com/keepsafe-engineering/an-in-depth-look-at-kotlins-initializers-a0420fcbf546](https://medium.com/keepsafe-engineering/an-in-depth-look-at-kotlins-initializers-a0420fcbf546)


Kotlin中的companion object
====================================
`companion object`可以有名字，也可以省略。如果省略，就默认`Companion`。  
有点像`java`里面的`static`，但底层实现不一样。  
>有用资料：[https://www.programiz.com/kotlin-programming/companion-objects](https://www.programiz.com/kotlin-programming/companion-objects)



