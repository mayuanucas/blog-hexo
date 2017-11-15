---
title: scala字符串变量替换
date: 2017-11-15 12:17:19
tags: Scala
categories: 快学Scala(第二版)
---

Scala字符串变量替换
<!-- more -->

### s 前缀
**s** 前缀的作用是用来表示变量的替换。
```scala
def test() = {
  val word = "hello"
  println(s"$word, scala!")
}
```

输出结果为：
{% asset_img img01.jpg %}


### f 前缀
**f** 前缀在表示变量替换的同时，还可以在变量名后面添加格式化参数。
```scala
def test() = {
  val year = 2017
  println(f"hello, $year%.2f")
  val year2 = "2017"
  println(f"hello, $year2%.3s")
}
```

输出结果为：
{% asset_img img02.jpg %}


### raw 前缀
**raw** 前缀在表示变量替换的同时，不对特殊字符转义。
```scala
def test() = {
  val year = 2017
  println(raw"hello\n$year")
}
```

输出结果为：
{% asset_img img03.jpg %}
