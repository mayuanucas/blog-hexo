---
title: 快学Scala(第二版)_第二章习题
date: 2017-11-13 20:06:37
tags: Scala
categories: 快学Scala(第二版)
mathjax: true
---

快学Scala(第二版) 第二章习题
<!-- more -->

### 一个数字如果为正数，则它的signum为 1 ；如果是负数，则signum为 -1 ；如果是 0 则signum 为 0 。编写一个函数来计算这个值。
```scala
def signum(num: Int): Int = {
    if (1 < num) 1 else if (0 == num) 0 else -1
  }
```
Scala中的API中有对应的方法： BigInt(23).signum

### 一个空的块表达式{}的值是什么？类型是什么？
空的块表达式{}的值是 ()   类型是 Unit
{% asset_img img01.jpg %}

### 指出在Scala中任何情况下赋值语句 x = y = 1 是合法的。
在Scala中赋值语句是Unit类型，所以只要 x 是Unit类型就可以啦。
{% asset_img img02.jpg %}

### 针对下列Java循环编写一个Scala版：
```Java
for (int i=10; i>=0; i--){
  System.out.println(i);
}
```

```scala
for (i <- (0 to 10).reverse){
      println(i)
    }
```


### 编写一个过程countdown(n: Int)，打印从n到0的数字。
```scala
def countdown(n: Int): Unit ={
    for (i <- (0 to n).reverse){
      println(i)
    }
  }
```

### 编写一个for循环，计算子字符串中所有字母的Unicode代码的乘积。举例来说，“Hello”中所有字符的乘积为9415087488L。
```scala
def countUnicode(text: String) = {
    var ans: Long = 1
    for (s <- text){
      ans *= s.toLong
    }
    ans
  }
```

### 同样是解决前一个练习的问题，但这次不使用循环。
```scala
def countUnicode2(text: String) = {
    var ans: Long = 1
    text.foreach(ans *= _.toLong)
    ans
  }
```

### 编写一个函数product(s: String)，计算前面练习中提到的乘积。
```scala
def product(text: String) = {
    var ans: Long = 1
    text.foreach(ans *= _.toLong)
    ans
  }
```


### 把前一个练习中的函数改成递归函数。
```scala
def product(text: String): Long = {
    if (1 == text.length) {
      text(0).toLong
    } else {
      text(0).toLong * product(text.drop(1))
    }
  }
```


{% asset_img img00.jpg%}
```scala
def func(x: Double, n: Int):Double = {
    if (0 == n) 1
    else if (0 < n && 0 == n%2) {
      func(x, n/2) * func(x, n/2)
    } else if (0 < n && 0 != n%2) {
      x * func(x, n-1)
    } else {
      1 / func(x, -n)
    }
  }
```
