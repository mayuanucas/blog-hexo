---
title: 快学Scala(第二版)_第三章习题
date: 2017-11-14 20:27:27
tags: Scala
categories: 快学Scala(第二版)
---

快学Scala(第二版) 第三章习题
<!-- more -->

### 编写一段代码，将a设置为一个n个随机整数的数组，要求随机数介于0（包含）和 n （不包含）之间。
```scala
def func(n: Int): Array[Int] = {
    val random = new Random()
    val randomArr = new Array[Int](n)
    for (i <- 0 until n) randomArr(i) = random.nextInt(n)
    randomArr
  }
```

### 编写一个循环，将整数数组中相邻的元素置换。例如，Array(1, 2, 3, 4, 5)经过置换后变为Array(2, 1, 4, 3, 5)。
```scala
def fun2(arr: Array[Int]) = {
    for (i <- 0 until arr.length by 2) {
      if (i + 1 < arr.length) {
        val temp = arr(i)
        arr(i) = arr(i+1)
        arr(i+1) = temp
      }
    }
    arr
  }
```

### 重复前一个练习，不过这一次生成一个新的值交换过的数组。使用 for/yield 。
```scala
def func(arr: Array[Int]): Array[Int] = {
    val newArr = for (i <- Range(0, arr.length)) yield {
      if (1 == arr.length % 2 && arr.length - 1 == i) arr(i)
      else if (0 == i % 2) arr(i + 1)
      else arr(i - 1)
    }
    newArr.toArray
  }
```

### 给定一个整数数组，产生一个新的数组，包含元数组中的所有正值，以原有顺序排列；之后的元素是所有零或负值，以原有顺序排序。
```scala
def func2(arr: Array[Int]): Array[Int] = {
    val posArr = new ArrayBuffer[Int]
    val negArr = new ArrayBuffer[Int]

    for (item <- arr) {
      if (item > 0) posArr += item
      else negArr += item
    }

    negArr.copyToBuffer(posArr)
    posArr.toArray
  }
```

### 如何计算Array[Double]的平均值？
```scala
def func3(arr: Array[Double]): Double = {
    arr.sum / arr.length
  }
```

### 如何重新组织Array[Int]的元素将它们以反序排序？对于ArrayBuffer[Int] 你又会怎么做呢？
```scala
def func4(arr: Array[Int]) = {
    arr.reverse
  }
```

```scala
def func5(arr: ArrayBuffer[Int]) = {
    arr.reverse
  }
```

### 编写一段代码，产出数组中的所有值，去掉重复项。
```scala
def func6(arr: Array[Int]) = {
    arr.toBuffer.distinct.toArray
  }
```

### 假定你拿到一个整数的数组缓冲，想要移除第一个负数外的所有负数。以下是一个顺序解决方案，在第一个负数被叫到时设置标记，然后移除所有该标记之后的负数。
```scala
var n = a.length
var i = 0
while (i < n){
  if (a(i) >= 0) i += 1
  else {
    if (first){ first = false; i += 1}
    else {a.remove(i); n -= 1}
  }
}
```
这是一个复杂而低效的方案。用Scala重写，采集负数元素的位置，丢弃第一个(位置)元素，反转该序列，然后对每个位置下标调用a.remove(i)。
```scala
def func7(arr: ArrayBuffer[Int]) = {
    val negArr = new ArrayBuffer[Int]()

    for (i <- 0 until arr.length) {
      if (0 > arr(i)) negArr += i
    }

    val temp = negArr.reverse
    temp.trimEnd(1)

    for (item <- temp) {
      arr.remove(item)
    }
    arr.toArray
  }
```

### 改进前一个练习的方案，采集应该被移动的位置和目标位置。执行这些移动并截断缓冲。不要复制第一个不需要的元素之前的任何元素。
```scala

```


### 创建一个由 java.util.TimeZone.getAvailableIDs返回的时区集合，判断条件是它们在美洲。去掉“American/”前缀并排序。
```scala
def func8() = {
    val arraySet = java.util.TimeZone.getAvailableIDs.filter(
      _.startsWith("America/")).map(_.replaceFirst("America/", "")).sorted
    arraySet
  }
```


### 引入java.awt.datatransfer._ 并构建一个类型为 SystemFlavorMap 类型的对象：
```scala
val flavors = SystemFlavorMap.getDefaultFlavorMap().asInstanceOf(SystemFlavorMap)
```
然后以DataFlavor.imageFlavor 为参数调用 getNativesForFlavor 方法，以Scala 缓冲保存返回值。

```scala
import java.awt.datatransfer._
import scala.collection.JavaConversions._
import scala.collection.mutable.Buffer
val flavors = SystemFlavorMap.getDefaultFlavorMap().asInstanceOf[SystemFlavorMap]
val flavor: Buffer[String] = flavors.getNativesForFlavor(DataFlavor.imageFlavor)
```
