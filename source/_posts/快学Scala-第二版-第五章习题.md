---
title: 快学Scala(第二版)_第五章习题
date: 2017-11-15 15:06:58
tags: 
- scala
categories: 
- 快学Scala(第二版)
---

快学Scala(第二版) 第五章习题
<!-- more -->

### 改进5.1节的Counter类，让它不要在 Int.MaxValue 时变成负数。
```scala
class Counter{
  private var value = 0

  def increment(): Unit = {
    if(value < Int.MaxValue){
      value += 1
    }
  }

  def current() = value
}
```

### 编写一个 BankAccount 类，加入 deposit 和 withdraw 方法，以及一个只读的 balance 属性。
```scala
class BankAccount {
  private var balance: Double = 0.0

  def getBalance(): Double = {
    balance
  }

  def deposit(num: Double) = {
    balance += num
  }

  def withdorw(num: Double) = {
    if (num < balance) {
      balance -= num
    } else {
      throw new Exception("withdraw money over total money")
    }
  }
}
```

### 编写一个 Time 类，加入只读属性 hours 和 minutes, 以及一个检查某一时刻是否早于另一个时刻的方法 before(other: Time): Boolean。Time 对象应该以 new Time(hrs, min) 方式构建，其中 hrs 小时数以军用时间格式呈现(介于 0 和 23 之间)。
```scala
class Time(var h: Int = 0, var m: Int = 0) {
  private var hours: Int = 0
  private var minutes: Int = 0

  if (0 <= h && h <= 23){
    hours = h
  } else {
    throw new Exception("construct Time error")
  }
  if (0 <= m && m <= 59){
    minutes = m
  } else {
    throw new Exception("construct Time error")
  }

  def getHours() = hours

  def getMinutes() = minutes

  def before(other: Time): Boolean = {
    if (hours < other.getHours() || hours == other.getHours() && minutes < other.getMinutes()) {
      true
    } else {
      false
    }
  }
}
```

### 重新实现前一个练习中的Time类，将内部呈现改成自午夜起的分钟数(介于 0 到 24*60-1之间)。不要改变公有接口。也就是说，客户端代码不应因为你的修改而受到影响。
```scala
class Time4(var h: Int = 0, var m: Int = 0) {
  var minutes: Int = 0

  if (!(0 <= h && h <= 23)){
    throw new Exception("construct Time error")
  }
  if (!(0 <= m && m <= 59)){
    throw new Exception("construct Time error")
  }

  minutes = h * 60 + m

  def getHours() = {
    minutes / 60
  }

  def getMinutes() = {
    minutes % 60
  }

  def before(other: Time): Boolean = {
    if (minutes < other.getHours() * 60 + other.getMinutes()) {
      true
    } else {
      false
    }
  }
}
```

### 创建一个Student类，加入可读写的JavaBean属性name(类型为String)和id(类型为Long).有哪些方法被生成？(用javap查看)你可以在Scala中调用JavaBeans版的getter和setter方法吗？应该这样做吗？
{% asset_img img01.jpg %}

### 在5.1节的Person类中提供一个主构造器，将负年龄转换为0.
```scala
class Student {
  @BeanProperty var name: String = ""
  @BeanProperty var id: Long = 0L
}

class Person(myAge: Int) {
  var age: Int = 0

  if (myAge < 0) {
    age = 0
  } else {
    age = myAge
  }

  def getAge() = age
}
```

### 编写一个Person类，其主构造器接受一个字符串，该字符串包含名字、空格和姓，如new Person(“Fred Smith”)。提供只读属性firstName和LastName。
```scala
class Person(val name : String) {
  val firstName = name.split(" ")(0)
  val lastName = name.split(" ")(1)
}
```

### 编写一个Car类，以只读属性对应制造商、设备型号、型号年份以及一个可读写的属性用于车牌。提供四组构造器。每一个构造器都要求制造商和型号名称为必填。型号年份以及车牌为可选，如果未填，则型号年份设置为 -1，车牌设置为空字符串。你会选择哪一个作为你的主构造器？为什么？
```scala
class Car(val maker: String, val typeName: String) {
  private var typeYear: Int = -1
  var numberOfCar = ""

  def this(maker: String, typeName: String, typeYear: Int) {
    this(maker, typeName)
    this.typeYear = typeYear
  }

  def this(maker: String, typeName: String, numberOfCar: String) {
    this(maker, typeName)
    this.numberOfCar = numberOfCar
  }

  def this(maker: String, typeName: String, typeYear: Int, numberOfCar: String) {
    this(maker, typeName)
    this.typeYear = typeYear
    this.numberOfCar = numberOfCar
  }
}
```

### 在Java、C#或C++中重做前一个练习。相比之下Scala精简了多少？



### 考虑如下类：
```scala
class Employee(var name: String, var salary: Double) {
  def this() {this("John Q. Public", 0.0)}
}
```
重写该类，使用显式的字段定义和一个默认主构造器。你更倾向于使用哪一种形式？为什么？

```scala
class Employee {
  private var name: String = "John Q. Public"
  private var salary: Double = 0.0

  def this(name: String, salary: Double) {
    this()
    this.name = name
    this.salary = salary
  }

  def getName() = this.name
  def getSalary() = this.salary
}
```
