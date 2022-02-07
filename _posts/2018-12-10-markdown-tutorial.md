---
layout: post
title: Markdown 入门笔记
date: 2018-12-10 20:28:00 +0800
categories: Markdown
tag: 教程
---
* content
{:toc}


这是一篇关于[Markdown Tutorial](https://www.markdowntutorial.com/)的笔记。  
本文所有示例源码，都可以在这个[项目源码](https://github.com/hqglichao/hqglichao.github.io)找到。  

初级                       {#Mardow-junior}
------------------------------------------

* 前后加_ _斜体_ 
* 前后加\**前后 **加粗**
* \# 到 \###### 表示标题等级1-6级，极端级用的较少。注意后面要跟一个空格
* 标题1、2级还有另一种表现方式（用多个==和--），[这里](https://wastemobile.gitbooks.io/gitbook-chinese/content/format/markdown.html)有详细解释
* \[]() 给一个文件加链接，前面的\[]写文字，后面的（）写链接。例子：[GitHub](https:\\www.github.com)
* \![]() 插入图片， \[]中写图片没加载出来的时候的提示词，后面的（）中写图片链接。
* \<img src="xxx" height="400" width="200"/\>  图片宽高限制方法
* \<img src="xxx" style="zoom:60%;"/\> 图片缩放到60%
* \[]\[]这种方式也可以给文字加链接，第二个\[]中加tag。其他地方声明\[tag]: link。例子：[GitHub][linkName]
* 换行的两种方式
  * 空行换行
  * 行尾两个空格自动换行
* 列表用\*，子列表换行在行前加空格
* 有序列表：数字加.加空格

[linkName]: https:\\\\www.github.com



中级                     {#Mardow-senior}
------------------------------------------

* \`包裹，代表提示。例子：`引用`
* \>引用。例子：
  >引用
* \~~~base，并且以```结尾，外加边框。例子：
  ~~~bash
  内容
  ~~~