---
title: 快学Scala(第二版)_第四章习题
date: 2017-11-15 15:06:50
tags: Scala
categories: 快学Scala(第二版)
---

快学Scala(第二版) 第四章习题
<!-- more -->

### 设置一个映射，其中包含你想要的一些装备及其价格，然后构建另一个映射采用同一组键，但在价格上打9折。
```scala
def func() = {
    val origin = Map("Mac" -> 11000, "ipad" -> 2600, "iphone" -> 8800)

    for ((k, v) <- origin) yield (k, v * 0.9)
  }
```

### 编写一段程序，从文件中读取单词。用一个可变映射来清点每一个单词出现的频率。读取这些单词的操作可以使用 java.util.Scanner:
```scala
val in = new java.util.Scanner(new java.io.File("myfile.txt"))
while (in.hasNext()) 处理 in.next()
```
或者翻到第9章看看更Scala的做法。
```scala
import scala.io.Source  
import scala.collection.mutable.HashMap  

def func2() = {
    val source = Source.fromFile("myfile.txt").mkString
    val tokens = source.split("\\s+")

    val map = new mutable.HashMap[String, Int]()

    for(key <- tokens){
      map(key) = map.getOrElse(key, 0) + 1
    }

    println(map.mkString(","))
  }
```

### 重复前一个练习，这次用不可变映射。
```scala
import scala.io.Source

def func3() = {
    val source = Source.fromFile("myfile.txt").mkString
    val tokens = source.split("\\s+")

    var map = Map[String, Int]()
    for (key <- tokens) {
      map += (key -> (map.getOrElse(key, 0) + 1))
    }
    println(map.mkString(","))
  }
```

### 重复前一个练习，这次用已排序的映射，以便单词可以按顺序打印出来。
```scala
import scala.io.Source  
import scala.collection.SortedMap

def func4() = {
    val source = Source.fromFile("myfile.txt").mkString
    val tokens = source.split("\\s+")

    var map = SortedMap[String, Int]()
    for (key <- tokens) {
      map += (key -> (map.getOrElse(key, 0) + 1))
    }
    println(map.mkString(","))
  }
```

### 重复前一个练习，这次用 java.util.TreeMap 并使之使用于 Scala API。
```scala
import scala.io.Source  
import scala.collection.mutable.Map  
import scala.collection.JavaConversions.mapAsScalaMap  
import java.util.TreeMap  

def func5() = {
    val source = Source.fromFile("myfile.txt").mkString
    val tokens = source.split("\\s+")

    val map: Map[String, Int] = new TreeMap[String, Int]
    for (key <- tokens) {
      map(key) = map.getOrElse(key, 0) + 1
    }
    println(map.mkString(","))
  }
```

### 定义一个链式哈希映射，将 “Monday” 映射到 java.util.Calendar.MONDAY ，依次类推加入其他日期。展示元素是以插入的顺序被访问的。



### 打印出所有Java 系统熟悉的表格，类似下面这样：



### 编写一个函数 minmax(values: Array[Int]) , 返回数组中最小值和最大值的对偶。



### 编写一个函数 lteqgt(valuse: Array[Int], v: Int), 返回数组中小于 v、等于 v 和大于 v 的数量，要求三个值一起返回。



### 当你将两个字符串拉链在一起，比如 "Hello".zip("World"), 会是什么结果？想出一个讲的通的用例。
