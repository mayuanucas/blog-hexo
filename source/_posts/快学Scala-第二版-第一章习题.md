---
title: 快学Scala(第二版)_第一章习题
date: 2017-11-13 18:39:59
tags: Scala
categories: 快学Scala(第二版)
---

快学Scala(第二版) 第一章习题
<!-- more -->

### 在Scala REPL中键入3，然后按住Tab建。有哪些方法可以被应用？
{% asset_img img01.jpg %}

### 在Scala REPL中，计算3的平方根，然后再对该值求平方。现在，这个结果与3相差多少？
{% asset_img img02.jpg %}

### res变量是val还是var？
res变量是val ,因为res变量不可再被赋值。

### Scala允许你用数字去乘字符串---去REPL中试一下 “crazy” * 3 。这个操作做什么？在Scaladoc中如何找到这个操作？
{% asset_img img03.jpg %}
从代码可以推断， "\*"是字符串所具有的方法，很明显此方法在StringOps中可以找到。

### 10 max 2 的含义是什么？max方法的定义在哪个类中？
{% asset_img img04.jpg %}
由上图可以看出，此方法返回两个数字中较大的那一个， max方法定义在RichInt 中。

### 用BigInt计算2的1024次方。
{% asset_img img05.jpg %}

### 为了在使用probablePrime(100, Random)获取随机素数时不在probablePrime 和 Random 之前使用任何限定符，你需要引入什么？
Random在scala.util中，而probablePrime是BigInt中的方法，引入即可
{% asset_img img06.jpg %}
{% asset_img img07.jpg %}
```scala
import scala.math.BigInt._
import scala.util.Random
```

### 创建随机文件的方式之一是生成一个随机的BigInt，然后将它转换成三十六进制，输出类似"qsnvbevtomcj38o06kul"这样的字符串。查阅Scaladoc, 找到在Scala中实现该逻辑的办法。
{% asset_img img08.jpg%}

### 在Scala中如何获取字符串的首字符和尾字符？
{% asset_img img09.jpg %}

### take、drop、takeRight和dropRight这些字符串函数是做什么用的？和substring相比，它们的优点和缺点有哪些？
take 从字符串首部开始获取字符串
drop 从字符串首部开始去除字符串
takeRight 从字符串尾部开始获取字符串
dropRight 从字符串尾部开始去除字符串
如果要获取字符串中间的 “子字符串”，那么需要同时调用 drop 和 dropRight，或者使用 substring 。
