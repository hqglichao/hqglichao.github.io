---
layout: post
title: Kotlin学习笔记
date: 2019-1-7 18:25:00 +0800
categories: Kotlin
tag: [Kotlin]
---

* content
{:toc}


init和constructor的不同
====================================
`Kotlin`中的数据初始化有好几种方式，其中调用的方式是不同的。  
### Initializer
构造函数外的数据初始化
* **Property initializers**
  ```bash
  var name: String = "David"
  ```
* **Initializer blocks**  
  逻辑比较多时，可以这样初始化，用init包裹。
  ```bash
  init {
    /* do some setup here */
  }
  ```  


### Constructors
构造函数中的数据初始化，如下代码中constructor。
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

by lazy
====================================
第一次调用的时候会执行里面的所有代码，然后就会记录返回值。  
第二次调用就不执行代码了，直接用上次的返回值，节省重复执行时间消耗

-------------------------------------------------------------------------------  

run, with, let, also 和 apply
=====================================
>这篇文章写的非常好：[https://medium.com/@elye.project/mastering-kotlin-standard-functions-run-with-let-also-and-apply-9cd334b0ef84](https://medium.com/@elye.project/mastering-kotlin-standard-functions-run-with-let-also-and-apply-9cd334b0ef84)  
这篇文章关于用`this`还是`it`：[https://proandroiddev.com/the-difference-between-kotlins-functions-let-apply-with-run-and-else-ca51a4c696b8](https://proandroiddev.com/the-difference-between-kotlins-functions-let-apply-with-run-and-else-ca51a4c696b8)

以下这是对上面文章的理解总结归纳。  
* **run**  
  这个函数可以理解为，内部的作用域和外部不同，内部可以重新定义和外部一样的变量名。此外还有一个好处，多个对象调用一个方法可以写在一处。以下函数就不用写两个show():
  ```bash
    run {
        if (firstTimeView) introView else normalView
    }.show()
  ```
* **with**  
  `with`其实和`T.run()`很类似。
  类似的地方:  
  ```bash
  with(webview.settings) {
    javaScriptEnabled = true
    databaseEnabled = true
  }
  //与下面代码相似
  webview.settings.run {
    javaScriptEnabled = true
    databaseEnabled = true
  }
  ```
  不同的地方，如果对象为空：
  ```bash
  //麻烦
  with(webview.settings) {
      this?.javaScriptEnabled = true
      this?.databaseEnabled = true
  }
  //简洁
  webview.settings?.run {
    javaScriptEnabled = true
    databaseEnabled = true
  }
  ```
* **let**  
  `let`传入的是參数要用`it`来使用。可以对`it`进行重名名。返回的可以是改造过的`it`，或者之前返回另一个类型的数据。
* **also**  
  `also`和`let`有所不一样，返回的和传入的是一样的。`also`可以用于代码分段。  
  如以下例子很好解释了两者的区别：  
  ```bash
  //正确的
  original.let {
    println("The original String is $it") // "abc"
    it.reversed() // evolve it as parameter to send to next let
  }.let {
    println("The reverse String is $it") // "cba"
    it.length  // can be evolve to other type
  }.let {
    println("The length of the String is $it") // 3
  }
  //不对的
  original.also {
    println("The original String is $it") // "abc"
    it.reversed() // even if we evolve it, it is useless
  }.also {
    println("The reverse String is ${it}") // "abc"
    it.length  // even if we evolve it, it is useless
  }.also {
    println("The length of the String is ${it}") // "abc"
  }

  //also的正确使用
  original.also {
    println("The original String is $it") // "abc"
  }.also {
    println("The reverse String is ${it.reversed()}") // "cba"
  }.also {
    println("The length of the String is ${it.length}") // 3
  }
  ```
* **apply**  
  `T.apply`传入`this`作为参数，返回修改后的（比如添加了属性值）的`this`。还可以链式调用。  
  ```bash
  fun createIntent(intentData: String, intentAction: String) =
        Intent().apply { action = intentAction }
                .apply { data = Uri.parse(intentData) }
  ```