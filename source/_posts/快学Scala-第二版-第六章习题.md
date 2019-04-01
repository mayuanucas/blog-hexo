---
title: 快学Scala(第二版)_第六章习题
date: 2017-11-15 15:07:03
tags: 
- scala
categories: 
- 快学Scala(第二版)
---

快学Scala(第二版) 第六章习题
<!-- more -->

### 编写一个 Conversions 对象，加入 inchesToCentimeters 、 gallonsToLiters 和 milesToKilometers 方法。
```scala
object Conversions {
  def inchesToCentimeters() = {
  }

  def gallonsToLiters()={
  }

  def milesToKilometers()={
  }
}
```

### 前一个练习不是很面向对象。提供一个通用的超类 UnitConversion 并定义扩展该超类的 InchesToCentimeters、GallonsToLiters和MilesToKilometers对象。
```scala
abstract class UnitConversion {
  def inchesToCentimeters() = {
  }

  def gallonsToLiters()={
  }

  def milesToKilometers()={
  }
}

object InchesToCentimeters extends UnitConversion {
  override def inchesToCentimeters() = {}
}

object GallonsToLiters extends UnitConversion {
  override def gallonsToLiters() = {}
}

object MilesToKilometers extends UnitConversion {
  override def milesToKilometers() = {}
}

```

### 定义一个扩展自 java.awt.Point 的 origin 对象。为什么说这实际上不是个好主意？(仔细看Point类的方法)
```scala
// Point类中的getLocation 方法返回的是Point对象，如果想返回Origin对象，需要Origin类才行。

object Origin extends java.awt.Point {
  override def getLocation:Point = super.getLocation
  Origin.move(2,3)
  println(Origin.toString)
}
```

### 定义一个 Point 类和一个伴生对象，使得我们可以不用 new 而直接用 Point(3, 4)来构造 Point 实例。
```scala
class Point(x: Int, y: Int) {
  override def toString: String = {
    "(" + x.toString + " , " + y.toString + ")"
  }
}

object Point {
  def apply(x: Int, y: Int): Point = new Point(x, y)
}
```

### 编写一个 Scala 应用程序，使用 App 特质，以反序打印命令行参数，用空格隔开。举例来说， scala Reverse Hello World 应该打印出 World Hello
```scala
object Reverse extends App {
  args.reverse.foreach(arg => print(arg + " "))
}
```

### 编写一个扑克牌4种花色的枚举，让其 toString 方法分别返回 ♣ ♦ ♥ 或 ♠
```scala
object Card extends Enumeration with App {
  val M = Value("♣")
  val T = Value("♠")
  val H = Value("♥")
  val F = Value("♦")

  println(Card.M)
  println(Card.T)
  println(Card.H)
  println(Card.F)
}
```

### 编写一个函数，检查某张牌的花色是否为红色。
```scala
def color(c: Card.Value) ={
    if (c == Card.M || c == Card.T){
      print("Black")
    } else {
      print("Red")
    }
  }
```

### 编写一个枚举，描述RGB 立方体的8个角。 ID 使用颜色值(例如，红色是0xff0000)
```scala
object RGB extends Enumeration with App{
  val RED = Value(0xff0000,"Red")
  val BLACK = Value(0x000000,"Black")
  val GREEN = Value(0x00ff00,"Green")
  val CYAN = Value(0x00ffff,"Cyan")
  val YELLOW = Value(0xffff00,"Yellow")
  val WHITE = Value(0xffffff,"White")
  val MAGENTA = Value(0xff0000,"Magenta")

}
```
