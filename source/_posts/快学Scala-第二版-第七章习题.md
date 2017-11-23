---
title: 快学Scala(第二版)_第七章习题
date: 2017-11-15 15:07:10
tags: Scala
categories: 快学Scala(第二版)
---

快学Scala(第二版) 第七章习题
<!-- more -->

### 编写示例程序，展示为什么
```scala
package com.horstmann.impatient
```
不同于
```scala
package com
package horstmann
package impatient
```

```scala

```

### 编写一段让你的Scala朋友们感到困惑的代码，使用一个不在顶部的com包。
```scala

```

### 编写一个包random, 加入函数nextInt(): Int 、 nextDouble(): Double 和 setSeed(seed: Int):Unit。生成随机数的算法采用线性同余生成器：
```scala
后值 = （前值 * a + b）mod 2 ^n
其中， a = 1664525, b = 1013904223, n = 32, 前值的初始值为seed
```

```scala

```

### 在你看来，Scala的设计者为什么要提供 package object 语法，而不是简单地让你将函数和变量添加到包中呢?
```scala

```

### private[com] def giveRaise(rate: Double)的含义是什么？有用吗？
```scala

```

### 编写一段程序，将 Java 哈希映射中的所有元素复制到 Scala 哈希映射。用引入语句重命名这两个类。
```scala

```

### 在前一个练习中，将所有引入语句移动到尽可能小的作用域里。
```scala

```

### 以下代码的作用是什么？这是一个好主意吗？
```scala
import java._
import javax._
```

### 编写一段程序，引入 java.lang.System类，从 user.name 系统属性读取用户名，从 StdIn 对象读取一个密码。如果密码不是“secret”,则在标准输出流中打印一个问候消息。不要使用任何其他引入，也不要使用任何限定词（带句点的那种）。
```scala

```

### 除了StringBuilder 还有哪些 java.lang 的成员是被 scala 包覆盖的？
```scala

```
