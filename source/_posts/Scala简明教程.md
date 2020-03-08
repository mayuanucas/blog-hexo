---
title: Scala简明教程
date: 2020-03-07 15:03:32
tags: 
- scala
- 编程语言
categories: 
- 快学Scala(第二版)
---

Scala是一门主要以Java虚拟机(JVM)为目标运行环境并将面向对象和函数式编程语言的最佳特性结合在一起的编程语言。 我们可以使用 Scala编写出更加精简的程序，同时充分利用并发的威力。由于Scala默认运行于JVM之上，因此它可以访问任何Java类库并且与Java框架进行互操作，Scala还可以被编译成JavaScript代码，让我们更便捷、高效地开发Web应用。Scala既有动态语言那样的灵活简沽，同时又保留了静态类型检查带来的安全保障和执行效率， 加上其强大的抽象能力，既能处理脚本化的临时任务，又能处理高并发场景下的分布式互联网大数据应用，可谓能缩能伸。本文从实用角度出发， 给出了一份快速的、基于代码的人门指南，让你可以快速地掌握和应用。

<!-- more -->

# 概述

Scala 是一门多范式(multi-paradigm)的编程语言，设计初衷是要集成面向对象编程和函数式编程的各种特性。Scala 运行在Java虚拟机上，并兼容现有的Java程序。Scala 源代码被编译成Java字节码，所以它可以运行于JVM之上，并可以调用现有的Java类库。下面是一个简单的代码例子(HelloWorld.scala)：

```scala
object HelloWorld {
    def main(args: Array[String]): Unit = {
        println("Hello, world!")
    }
}

$ scalac HelloWorld.scala // 把源码编译为字节码
$ scala HelloWorld  // 把字节码放到虚拟机中解释运行
```

# 基础语法

## 基本概念

- 类：外部事物的抽象，具有方法和属性。
- 对象：对象是类的具体实例，类是对象的抽象。
- 方法：方法是类的某个行为动作，一个类可以包含多个方法。
- 属性：类的若干属性，又称为字段。
- 语句：语句是执行代码的单元。如果一行只有一个语句，那么结尾不需要加 `;` 分号。如果一行之内有多个语句的话，需要加分号 `;` 。
- 注释：支持多行注释 `/* */` 和单行注释 `//`。
- 包：使用 `package` 定义包，类似于命名空间。
- 引用：使用 `import` 引用包，需要注意 Scala 的包引用可以出现在任何地方，而不仅仅只是文件的顶部。
- 标识符：标识符可以由字母和下划线开头，后面可以连接数字、字母和下划线。区分大小写。
- 类名：类名采用驼峰写法，每个单词首字母大写。
- 方法名：方法名称的第一个字母用小写，其余每个单词的第一个字母大写。
- 程序文件名：程序文件名称和对象名称要完全匹配。虽然新版本不再要求，但为了保证代码能通过旧版本编译器，建议保持一致。
- 程序入口：程序入口为 main 方法。

## 数据类型

### Scala类型层次结构

`Any`是所有类型的超类型，也称为顶级类型。它定义了一些通用的方法如`equals`、`hashCode`和`toString`。`Any`有两个直接子类：`AnyVal`和`AnyRef`。

`AnyVal`代表值类型。有9个预定义的非空的值类型分别是：`Double`、`Float`、`Long`、`Int`、`Short`、`Byte`、`Char`、`Unit`和`Boolean`。`Unit`是不带任何意义的值类型，它仅有一个实例可以像这样声明：`()`。所有的函数必须有返回，所以说有时候`Unit`也是有用的返回类型。

`AnyRef`代表引用类型。所有非值类型都被定义为引用类型。在Scala中，每个用户自定义的类型都是`AnyRef`的子类型。如果Scala被应用在Java的运行环境中，`AnyRef`相当于`java.lang.Object`。

### Nothing和Null

`Nothing`是所有类型的子类型，也称为底部类型。没有一个值是`Nothing`类型的。它的用途之一是给出非正常终止的信号，如抛出异常、程序退出或者一个无限循环（可以理解为它是一个不对值进行定义的表达式的类型，或者是一个不能正常返回的方法）。

`Null`是所有引用类型的子类型（即`AnyRef`的任意子类型）。它有一个单例值由关键字`null`所定义。`Null`主要是使得Scala满足和其他JVM语言的互操作性，但是几乎不应该在Scala代码中使用。

### 整数

| 类型  | 说明                                                         |
| :---- | :----------------------------------------------------------- |
| Byte  | 8位有符号补码整数。数值区间为 -128 到 127                    |
| Short | 16位有符号补码整数。数值区间为 -32768 到 32767               |
| Int   | 32位有符号补码整数。数值区间为 -2147483648 到 2147483647     |
| Long  | 64位有符号补码整数。数值区间为 -9223372036854775808 到 9223372036854775807 |

### 浮点数

| 类型   | 说明                               |
| :----- | :--------------------------------- |
| Float  | 32 位, IEEE 754 标准的单精度浮点数 |
| Double | 64 位 IEEE 754 标准的双精度浮点数  |

### 字符

| 类型   | 说明                                             |
| :----- | :----------------------------------------------- |
| Char   | 16位无符号Unicode字符, 区间值为 U+0000 到 U+FFFF |
| String | 字符序列                                         |

### 布尔

| 类型    | 说明        |
| :------ | :---------- |
| Boolean | true或false |

### 其他类型

| 类型    | 说明                                                         |
| :------ | :----------------------------------------------------------- |
| Unit    | 表示无值，和其他语言中void等同。用作不返回任何结果的方法的结果类型。Unit只有一个实例值，写成()。 |
| Null    | null 或空引用                                                |
| Nothing | Nothing类型在Scala的类层级的最低端；它是任何其他类型的子类型。 |
| Any     | Any是所有其他类的超类                                        |
| AnyRef  | AnyRef类是Scala里所有引用类(reference class)的基类           |

### Scala 基础字面量

Scala 非常简单且直观,接下来详细介绍 Scala 字面量。

#### 整型字面量

整型字面量用于 Int 类型，如果表示 Long，可以在数字后面添加 L 或者小写 l 作为后缀。

```scala
0
035
21 
0xFFFFFFFF 
0777L
```

#### 浮点型字面量

如果浮点数后面有 f 或者 F 后缀时，表示这是一个Float类型，否则就是一个Double类型的，实例如下：

```scala
0.0 
1e30f 
3.14159f 
1.0e100
.1
```

#### 布尔型字面量

布尔型字面量有 true 和 false。

#### 符号字面量

符号字面量被写成： **'<标识符>** ，这里 **<标识符>** 可以是任何字母或数字的标识（注意：不能以数字开头）。这种字面量被映射成预定义类 scala.Symbol 的实例。

如： 符号字面量 **'x** 是表达式 **scala.Symbol("x")** 的简写，符号字面量定义如下：

```scala
package scala
final case class Symbol private (name: String) {
   override def toString: String = "'" + name
}
```

#### 字符字面量

在 Scala 字符变量使用单引号 **'** 来定义，如下：

```scala
'a' 
'\u0041'
'\n'
'\t'
```

其中 **\\** 表示转义字符，其后可以跟 **u0041** 数字或者 **\r\n** 等固定的转义字符。

#### 字符串字面量

在 Scala 字符串字面量使用双引号 **"** 来定义，如下：

```scala
"Hello,\nWorld!"
"Scala官网：www.scala-lang.org"
```

#### 多行字符串的表示方法

多行字符串用三个双引号来表示分隔符，格式为：**""" ... """**。

实例如下：

```
val foo = """scala官方网址
www.scala-lang.org
docs.scala-lang.org
www.scala-lang.org/community
以上三个地址都能访问"""
```

### Null 值

空值是 scala.Null 类型。

cala.Null和scala.Nothing是用统一的方式处理Scala面向对象类型系统的某些"边界情况"的特殊类型。

Null类是null引用对象的类型，它是每个引用类（继承自AnyRef的类）的子类。Null不兼容值类型。

## 变量与常量

变量在程序运行过程中，其值可能发生改变，采用 `var` 声明变量；

常量在程序运行过程中，其值不会发生改变，采用 `val` 声明常量。

声明变量和常量的语法如下：

```scala
var VariableName : DataType [=  Initial Value]

或

val VariableName : DataType [=  Initial Value]
```

另外 DataType 是可选的。如果不指定数据类型，编译器会自动根据初始值推断出数据类型。

示例代码：

```scala
// 声明其值可变的变量用var
var counter = 0
// 下面这条语句可以编译运行,我们可以改变一个var
counter = 1

// 以val定义的值实际上是一个常量,无法改变它的内容
val answer = 21
// 下面这条语句无法编译运行, Compilation Failed
answer = 22

// 可以将多个值或变量放在一起, 将xmax和ymax设为100
val xmax, ymax = 100 

// 指定类型
val name: String = "abc"
// greeting和mess age都是字符串， 被初始化为 null
var greeting, message: String = null
```

## 引用

Scala 使用 import 关键字引用包。在Scala中，import语句可以出现在任何地方， 而并不仅限于文件顶部， import语句的效果一直延伸到包含该语句的块末尾。

```scala
// 在Scala中， -字符表示通配符
// 下面的语句导入 math 包中的成员（类，特质，函数等）
import scala.math._

// 输出 1.4142135623730951
sqrt(2)
// 输出 16.0
pow(2, 4)
// 输出 3.0
min(3, Pi)

// 如果你不引人scala.math包，就添加包名来使用
// 输出 1.4142135623730951
scala.math.sqrt(2)
```

如果想要引入包中的几个成员，可以使用selector（选取器）：

```scala
// 想要引人包中的几个成员 ，可以像这样使用选取器
import java.awt.{Color,  Font}  
// 重命名成员,这样一来，JavaHashMap就是java.util.HashMap
import java.util.{HashMap  =>  JavaHashMap}  
// 隐藏成员
// 引入了util包的所有成员，但是HashMap被隐藏了。
// 选取器 HashMap =>_ 将隐藏某个成员而不是重命名它。这仅在你需要引人其他成员时有用。
import java.util.{HashMap  => _, _}
```

## 语句终止

在Scala中，与Python、JavaScript和其他脚本语言类似，行尾的位置上不需要分号。 同样，在 }、else 以及类似的位置也不必写分号，只要能够从上下文明确地判断出这里是语句的终止即可。不过，如果想在单行中写下多个语句， 就需要将它们以分号隔开。 例如：

```scala
if (n > 0) {r = r*n; n -= 1}
```

这里需要用分号 ; 将 r = r*n; n -= 1 隔开。由于有 ｝， 因此在第二个语句之后并不需要写分号。

## 块表达式和赋值

在Scala中，{} 块包含一系列表达式， 其结果也是一个表达式。 块中最后一个表达式的值就是块的值。这个特性对于那种对某个val 的初始化需要分多步完成的情况很有用 。 例如：

```scala
val distance = {val dx = x - xO; val dy = y - yO; sqrt(dx * dx + dy * dy)}
```

{}块的值取其最后一个表达式， 变量dx也和dy仅作为计算所需要的中间值， 很干净地对程序其他部分而言不可见了 。

在Scala中，赋值动作本身是没有值的，或者更严格地说， 它们的值是Unit类型的。一个以赋值语句结束的块， 比如：

```scala
{r = r*n; n -= 1}
```

的值是Unit类型的，当我们定义函数时需要意识到这一点。由于赋值语句的值是Unit类型的， 因此别把它们串接在一起。

```scala
// 别这样做, y=1 的值是（），你几乎不太可能想把一个Unit类型的值赋给x。
x = y = 1
```

## 输入输出

如果要打印一个值， 我们用print或println 函数。 后者在打印完内容后会追加一个换行符。例如：

```scala
print("Answer: ")
println(42)

// 上面两行代码与下面这一行代码输出的内容相同
println("Answer 42")
```

需要使用格式化字符串时，比较好的做法是使用字符串插值。使用f插值器比使用printf方法更好， 因为它是类型安全的不是数值的表达式使用了 %f， 编译器会报错。被格式化的字符串以字母 f 打头 。 它包含以 $ 打头并且可能带有C风格的格式化字符串的表达式。：

```scala
val name = "Scala"
val number = 7
//
println(f"Hello $name, the number is ${number + 0.8}%7.2f , Welcome.")
```

上面代码示例中表达式 $name 被替换成 Scala。

表达式 ${number + 0.8}%7.2f 则被替换成 number + 0.8的值，并以宽度为7，精度为2的浮点数格式化。 

需要注意的是，用 ${...} 将不是简单的变量名的表达式括起来。

可以用 scala.io.Stdin 的 readLine 方法从控制台读取一行输入，如果要读取数字、Boolean或者是字符，可以用 readByte、readShort、readInt、readLong、readFloat、readDouble、readBoolean或者readChar。与其他方法不同，readLine带有一个参数作为提示字符串：

```scala
import scala.io._

val name = StdIn.readLine("Your name:")
println("Your age:")
val age = StdIn.readInt()
println(f"Hello, ${name}! Next year, you will be ${age + 1}.")
```

## 流程控制逻辑

Scala 的流程控制主要包括条件控制和循环控制。

### 条件控制

其使用方法和其它语言类似，示例如下：

```scala
if(布尔表达式 1){
   // 如果布尔表达式 1 为 true 则执行该语句块
}else if(布尔表达式 2){
   // 如果布尔表达式 2 为 true 则执行该语句块
}else if(布尔表达式 3){
   // 如果布尔表达式 3 为 true 则执行该语句块
}else {
   // 如果以上条件都为 false 执行该语句块
}
```

在Scala 中 if/else表达式有值，这个值就是跟在 if或else之后的表达式的值。例如 ：

```scala
if (x > 0) 1 else -1
```

上述表达式的值是1或－ 1 ，具体是哪一个取决于x的值。 可以将if/else表达式的值赋给变量 ：

```scala
val s ＝ if (x > 0) 1 else -1
// 这与如下语句的效果一样：
if (x > 0) s = 1 else s = -1
```

不过第一种写法更好，因为它可以用来初始化一个val 。 而在第二种写法中，s必须是var。

### 循环控制

循环支持三种循环类型：while 循环、do while 循环和 for 循环。

1. while 循环

   ```scala
   while(condition) { //语句块 }
   ```

2. do while 循环

   ```scala
   do { //语句块 } while( condition );
   ```

3. for 循环

   ```scala
   for( var x <- Range ){ //语句块 }
   ```

### 循环控制的 break 和 continue

Scala 的 break 的语法和其它语言不同，它完全是基于类的一种实现。

文件：BreakDemo.scala

```scala
import scala.util.control.Breaks;

object BreakDemo {
   def main(args: Array[String]): Unit = {
      var i = 0;
      val numList = List(1,2,3,4);

      val loop = new Breaks;
      loop.breakable {
         for( i <- numList){
            println( "第" + i + "次" );
            if( i == 3 ){
               loop.break;
            }
         }
      }
      println( "事不过三" );
   }
}
```

输出结果：

```scala
第1次
第2次
第3次
事不过三
```

遗憾的是，Scala 里并没有 continue 的语法，即跳过本次循环。但想一下 continue 的实质是什么？跳过满足特定条件的循环中的元素，即过滤掉某些元素，让其不参与循环。Scala 提供了循环过滤的机制，来看一下示例。

我们循环 10 次，每次取一个 100 以内的随机数，要求只保留偶数，所有的奇数都过滤掉。实现上可以用 for + if 的循环过滤机制来实现。文件：ForFilterDemo.scala

```scala
import java.util.Random;

object ForFilterDemo {
   def main(args: Array[String]): Unit = {
        var r = new Random()
        var rand = r.nextInt(100)
        var i = 0
     
     		//只保留所有的偶数
        for(i <- 0 to 10 if rand % 2 == 0) {
            println(rand)
            rand = r.nextInt(100)
        }
   }
}
```

输出结果（每次都不相同）：

```scala
6
90
12
```

## 函数

函数是带有参数的表达式。可以定义一个匿名函数（即没有名字），来返回一个给定整数加一的结果。

```scala
(x: Int) => x + 1
```

`=>` 的左边是参数列表，右边是一个包含参数的表达式。也可以给函数命名：

```scala
val addOne = (x: Int) => x + 1
// 输出 2
println(addOne(1))
```

函数可带有多个参数：

```scala
val add = (x: Int, y: Int) => x + y
// 输出 3
println(add(1, 2))
```

或者不带参数：

```scala
val getTheAnswer = () => 42
// 输出 42
println(getTheAnswer())
```

## 方法

方法的表现和行为和函数非常类似，但是它们之间有一些关键的差别。方法由`def`关键字定义。`def`后面跟着一个名字、参数列表、返回类型和方法体。例如：

```scala
def abs(x: Double): Double = if (x >= 0) x else -x
```

方法可以接受多个参数列表。

```scala
def addThenMultiply(x: Int, y: Int)(multiplier: Int): Int = (x + y) * multiplier
// 9
println(addThenMultiply(1, 2)(3)) 
```

或者没有参数列表。

```scala
def name: String = System.getProperty("user.name")
println("Hello, " + name + "!")
```

方法也可以有多行的表达式。

```scala
def getSquareString(input: Double): String = {
  val square = input * input
  square.toString
}
// 6.25
println(getSquareString(2.5)) 
```

方法体的最后一个表达式就是方法的返回值。(Scala中也有一个 `return` 关键字，但是很少使用)

## 默认参数

我们在调用某些方法时并不显式地给出所有参数值，对于这些方法可以使用默认参数。例如：

```scala
def decorate(str: String, left: String = "[", right: String = "]") = {
  left + str + right
}
```

这个方法有两个参数，left 和 right，带有默认值"["和"]"。

如果调用decorate("Hello")，将会得到"[Hello]"。如果不喜欢默认的值，则可以给出自己的版本，例如：decorate("Hello", "<<<", ">>>")。带名参数可以让函数更加可读。它们对于那些有很多默认参数的方法来说也很有用。还可以混用未命名参数和带名参数，只要那些未命名的参数是排在前面的即可：

```scala
// 运行结果为: [Hello<<<
decorate("Hello", right = "<<<")
```

## 主方法

主方法是一个程序的入口点。JVM要求一个名为`main`的主方法，接受一个字符串数组的参数。

通过使用对象，你可以如下所示来定义一个主方法。

```scala
object Main {
  def main(args: Array[String]): Unit =
    println("Hello, Scala developer!")
}
```

# class、object、trait

## class

在Scala中，类名可以和对象名为同一个名字，该对象称为该类的伴生对象，类和伴生对象可以相互访问他们的私有属性，但是他们必须在同一个源文件内。类只会被编译，不能直接被执行，类的声明和主构造器在一起被声明，在一个类中，主构造器只有一个所有必须在内部声明主构造器或者是其他声明主构造器的辅构造器，主构造器会执行类定义中的所有语句。Scala对每个字段都会提供getter和setter方法，同时也可以显示的申明，但是针对val类型，只提供getter方法，默认情况下，字段为公有类型，可以在setter方法中增加限制条件来限定变量的变化范围，在Scala中方法可以访问该类所有对象的私有字段。

Scala 类的特征有如下几个：

- 单继承
- 无 static 静态属性和方法，但提供了单例对象（singleton objects）的概念
- 支持抽象类和抽象方法
- 支持重载（override）

可以使用`class`关键字定义一个类，后面跟着它的名字和构造参数。

```scala
class Greeter(prefix: String, suffix: String) {
  def greet(name: String): Unit =
    println(prefix + name + suffix)
}
```

`greet`方法的返回类型是`Unit`，表明没有什么有意义的需要返回。它有点像Java和C语言中的`void`。（不同点在于每个Scala表达式都必须有值，事实上有个`Unit`类型的单例值，写作`()`，它不携带任何信息）

可以使用`new`关键字创建一个类的实例。

```scala
val greeter = new Greeter("Hello, ", "!")
// Hello, Scala developer!
greeter.greet("Scala developer") 
```

## case class

Scala有一种特殊的类叫做样例类（case class）。默认情况下，样例类一般用于不可变对象，并且可作值比较。你可以使用`case class`关键字来定义样例类。

```scala
case class Point(x: Int, y: Int)
```

可以不用`new`关键字来实例化样例类。

```scala
val point = Point(1, 2)
val anotherPoint = Point(1, 2)
val yetAnotherPoint = Point(2, 2)
```

并且它们的值可以进行比较。

```scala
if (point == anotherPoint) {
  println(point + " and " + anotherPoint + " are the same.")
} else {
  println(point + " and " + anotherPoint + " are different.")
} // Point(1,2) and Point(1,2) are the same.

if (point == yetAnotherPoint) {
  println(point + " and " + yetAnotherPoint + " are the same.")
} else {
  println(point + " and " + yetAnotherPoint + " are different.")
} // Point(1,2) and Point(2,2) are different.
```

## object

object(对象)是它们自己定义的单实例，可以把它看作它自己的类的单例，可以使用`object`关键字定义对象。例如：

```scala
object IdFactory {
  private var counter = 0
  
  def create(): Int = {
    counter += 1
    counter
  }
}
```

在Scala中没有静态方法和静态字段，所以在Scala中可以用object来实现这些功能，直接用对象名调用的方法都是采用这种实现方式，例如Array.toString。对象的构造器在第一次使用的时候会被调用，如果一个对象从未被使用，那么他的构造器也不会被执行。对象本质上拥有类（Scala中）的所有特性，除此之外，object还可以扩展类以及一个或者多个特质：例如，

```scala
abstract class ClassName（val parameter）{}
object Test extends ClassName(val parameter){}
```

注意：object不能提供构造器参数，也就是说object必须是无参的。

## trait

在Java中可以通过interface实现多重继承，在Scala中可以通过特征（trait）实现多重继承，不过与Java不同的是，它可以定义自己的属性和实现方法体，在没有自己的实现方法体时可以认为它与Java interface是等价的，在Scala中也是一般只能继承一个父类，可以通过多个with进行多重继承。

```scala
trait TraitA{}
trait TraitB{}
trait TraitC{}
object Test1 extends TraitA with TraitB with TraitC{}
```

trait(特质)是包含某些字段和方法的类型，可以组合多个特质。可以使用`trait`关键字定义特质。

```scala
trait Greeter {
  def greet(name: String): Unit
}
```

特质也可以有默认的实现。

```scala
trait Greeter {
  def greet(name: String): Unit =
    println("Hello, " + name + "!")
}
```

可以使用`extends`关键字来继承特质，使用`override`关键字来覆盖默认的实现。

```scala
class DefaultGreeter extends Greeter

class CustomizableGreeter(prefix: String, postfix: String) extends Greeter {
  override def greet(name: String): Unit = {
    println(prefix + name + postfix)
  }
}

val greeter = new DefaultGreeter()
greeter.greet("Scala developer") // Hello, Scala developer!

val customGreeter = new CustomizableGreeter("How are you, ", "?")
customGreeter.greet("Scala developer") // How are you, Scala developer?
```



## companion object

在Java或C++中，你通常会用到既有实例方法又有静态方法的类。 在Scala中，可以通过类和与类同名的“伴生（ companion ）”对象来达到同样的目的。 例如 ：

```scala
class Account {
  val id = Account.newUniqueNumber()
  private var balance = 0.0
  def deposit(amount: Double) {balance += amount}
  ...
}

// 伴生对象
object Account {
  private var lastNumber = 0
  private def newUniqueNumber() = {lastNumber += 1; lastNumber}
}
```

类和它的伴生对象可以相互访问私有特性，它们必须存在于同一个源文件中。注意 ，类的伴生对象的功能特性并不在类的作用域内。举例来说， Account类必须通过Account.newUniqueNumber() 而不是直接用newUniqueNumber()来调用伴生对象的方法。

# 继承

Scala 是单继承的语言，子类只有一个父类。

##抽象类和抽象方法

定义一个抽象类，需要使用 `abstract` 关键字。例如：

```scala
abstract class Animal(name: String)
{
   //class body
   def eat 
}
```

定义抽象方法，只需保持方法的函数体为空即可。子类继承抽象父类，必须实现父类的所有的抽象方法。

## override

`override` 关键字有以下特点：

- 子类重写父类的抽象方法，不需要加 `override` 关键字。
- 子类重写父类的普通方法，需要加 `override` 关键字。
- 只有主构造函数才可以往父类的构造函数里写参数。

# Scala 访问修饰符

Scala 访问修饰符基本和Java的一样，分别有：private、protected、public。
如果没有指定访问修饰符，默认情况下，Scala 对象的访问级别都是 public。
Scala 中的 private 限定符，比 Java 更严格，在嵌套类情况下，外层类甚至不能访问被嵌套类的私有成员。

## 私有(Private)成员

用 private 关键字修饰，带有此标记的成员仅在包含了成员定义的类或对象内部可见，同样的规则还适用内部类。

```scala
class  Outer
{  
  class  Inner{  
    private  def f()
    {
        println("f")
    }  
    class  InnerMost
    { 
      f()  // 正确  
    }  
  }  
  (new  Inner).f()  //错误 
}
```

(new Inner).f( ) 访问不合法是因为 **f** 在 Inner 中被声明为 private，而访问不在类 Inner 之内。但在 InnerMost 里访问 **f** 就没有问题的，因为这个访问包含在 Inner 类之内。

## 保护(Protected)成员

在 scala 中，对保护（Protected）成员的访问比 java 更严格一些。因为它只允许保护成员在定义了该成员的类的子类中被访问。而在java中，用protected关键字修饰的成员，除了定义了该成员的类的子类可以访问，同一个包里的其他类也可以进行访问。

```scala
package p
{  
    class  Super
    {  
        protected  def f()  
        {println("f")}  
    } 
    class  Sub  extends  Super
    { 
        f() 
    } 
    class  Other
    { 
        (new  Super).f()  //错误 
    }  
}
```

上例中，Sub 类对 f 的访问没有问题，因为 f 在 Super 中被声明为 protected，而 Sub 是 Super 的子类。相反，Other 对 f 的访问不被允许，因为 other 没有继承自 Super。

## 公共(Public)成员

Scala中，如果没有指定任何的修饰符，则默认为 public。这样的成员在任何地方都可以被访问。

```scala
class  Outer  
{  
    class  Inner  
    {  
        def f()  
        {
            println("f")  
        }  
        class  InnerMost  
        { 
            f()  // 正确  
        }  
    }  
    (new  Inner).f()  // 正确因为 f() 是 public  
}
```

## 作用域保护

Scala中，访问修饰符可以通过使用限定词强调。格式为:

private[x] 或 protected[x]

这里的x指代某个所属的包、类或单例对象。如果写成private[x],读作：这个成员除了对[…]中的类或[…]中的包中的类及它们的伴生对像可见外，对其它所有类都是private。

这种技巧在横跨了若干包的大型项目中非常有用，它允许定义一些在项目的若干子包中可见但对于项目外部的客户却始终不可见的东西。

```scala
package bobsrocckets
{
    package navigation
    {
        private[bobsrockets] class Navigator
        {
             protected[navigation] def useStarChart(){}
             class LegOfJourney
             {
                 private[Navigator] val distance = 100
             }
              private[this] var speed = 200
         }
    }
    package launch
    {
        import navigation._
        object Vehicle
        {
            private[launch] val guide = new Navigator
        }
     }
}
```

上述例子中，类Navigator被标记为private[bobsrockets]就是说这个类对包含在bobsrockets包里的所有的类和对象可见。

比如说，从Vehicle对象里对Navigator的访问是被允许的，因为对象Vehicle包含在包launch中，而launch包在bobsrockets中，相反，所有在包bobsrockets之外的代码都不能访问类Navigator。

# 模式匹配

Scala 使用 `case` 关键字实现多条件逻辑匹配的功能。每个条件是一个备选项，称为模式。每个备选项包含一个模式和一到多个表达式。语法如下：

```scala
case 模式 => 语句
```

看几个简单例子（整型数值匹配、温度判断）学习模式匹配：

例子1，文件：Temperature.scala

```scala
object Temperature {
   def main(args: Array[String]): Unit = {
        println(display(37))
        println(display(100))
        println(display(50))
   }
  
   def display(x: Int): String = x match {
      case 0 => "freezing point" 
      case 100 => "boiling point" 
      case x if(x >= 36.5 && x <= 37.5) => "body point"
      case _ => "other point"
   }
}
```

输出结果：

```scala
body point
boiling point
other point
```

match 表达式通过以代码编写的先后次序尝试每个模式来完成计算，只要发现有一个匹配的case，剩下的case不会继续匹配。

例子2，文件：CaseClassDemo.scala

```scala
object CaseClassDemo {
   def main(args: Array[String]): Unit = {
       val alice = new Person("Alice", 25)
    	 val bob = new Person("Bob", 32)
       val charlie = new Person("Charlie", 32)
   
    for (person <- List(alice, bob, charlie)) {
        person match {
            case Person("Alice", 25) => println("Hi Alice!")
            case Person("Bob", 32) => println("Hi Bob!")
            case Person(name, age) =>
               println("Age: " + age + " year, name: " + name + "?")
         }
      }
   }
  
   // 样例类
   case class Person(name: String, age: Int)
}
```

输出结果：

```scala
Hi Alice!
Hi Bob!
Age: 32 year, name: Charlie?
```

在声明样例类时，下面的过程自动发生了：

- 构造器的每个参数都成为val，除非显式被声明为var，但是并不推荐这么做；
- 在伴生对象中提供了apply方法，所以可以不使用new关键字就可构建对象；
- 提供unapply方法使模式匹配可以工作；
- 生成toString、equals、hashCode和copy方法，除非显示给出这些方法的定义。

# apply

将对象以函数的方式进行调用时，scala会隐式地将调用改为在该对象上调用apply方法。例如XXX(“hello”)实际调用的是XXX.apply(“hello”), 因此apply方法又被称为注入方法。apply方法常用于创建类实例的工厂方法。示例如下：

```scala
object Greeting{
  def apply(name: String) = "Hello " + name
}

//下面两条语句的作用等价 
// 结果都为 Hello Lucy
Greeting.apply(“Lucy”) 
Greeting(“Lucy”) 
```

# unapply

与 apply 相对的是 unapply 方法，它的用法与 apply 类似，但其作用是用来抽取部分参数，它也称为抽取方法，主要用于模式匹配时抽取某些参数 case XXX(str) => println(str)

# Scala 闭包

闭包是一个函数，返回值依赖于声明在函数外部的一个或多个变量。这里我们引入一个自由变量 factor，这个变量定义在函数外面。这样定义的函数变量 multiplier 成为一个"闭包"，因为它引用到函数外面定义的变量，定义这个函数的过程是将这个自由变量捕获而构成一个封闭的函数。

```scala
object Test 
{  
   def main(args: Array[String]): Unit = {  
      println( "muliplier(1) value = " +  multiplier(1) )  
      println( "muliplier(2) value = " +  multiplier(2) )  
   }  
  
   var factor = 3  
   val multiplier = (i: Int) => i * factor  
}  


$ scalac Test.scala  
$  scala Test  
// 输出结果为：
muliplier(1) value = 3  
muliplier(2) value = 6
```

# Scala 字符串

String 对象是不可变的，如果需要创建一个可以修改的字符串，可以使用 StringBuilder 类，如下示例:

```scala
object Test
{
   def main(args: Array[String]): Unit = {
      val buf = new StringBuilder;
      buf += 'a'
      buf ++= "bcdef"
      println( "buf is : " + buf.toString );
   }
}
```

# Scala 数组

## 定长数组

如果你需要一个长度不变的数组， 可以用Scala中的Array。 例如：

```scala
// 10个整数的数组，所有元素初始化为0
val nums = new Array[Int](10)

// 10个元素的字符串数组，所有元素初始化为null
val a = new Array[String](10)

// 长度为2的Array[String],类型是推断出来的
// 说明：当已提供初始值时，就不需要new了
val s = Array("Hello", "World")

s(0) = "Goodbye"
```

## 变长数组：数组缓冲

对于那种长度按需要变化的数组，Scala中对应的数据结构为ArrayBuffer 。

```scala
import scala.collection.mutable.ArrayBuffer

// 一个空的数组缓冲，准备存放整数
val b = ArrayBuffer[Int]()
// 用 += 在尾端添加元素: ArrayBuffer(1)
b += 1
// 在尾端添加多个元素，以括号包起来: ArrayBuffer(1, 1, 2, 3, 5)
b += (1, 2, 3, 5)
// 可以用 ++= 操作符追加任何集合: ArrayBuffer(1, 1, 2, 3, 5, 8, 13, 21)
b ++= Array(8, 13, 21)
// 移除最后5个元素: ArrayBuffer(1, 1, 2)
b.trimEnd(5)
```

在数组缓冲的尾端添加或移除元素是一个高效的操作，也可以在任意位置插入或移除元素， 但这样的操作并不那么高效，因为所有在那个位置之后的元素都必须被平移 。 举例如下 ：

```scala
// 在下标2之前插入6: ArrayBuffer(1, 1, 6, 2)
b.insert(2, 6)
// 移除下标1位置的元素: ArrayBuffer(1, 6, 2)
b.remove(1)
// 第2个参数的含义是要移除多少个元素: ArrayBuffer(1)
b.remove(1, 2)
```

有时你需要构建一个Array但不知道最终需要装多少元素。在这种情况下，先构建一个数组缓冲，然后调用`toArray` ，例如：

```scala
// 得到: Array(1)
b.toArray
```

## 遍历数组和数组缓冲

使用 for 循环遍历数组和数组缓冲的语法：

```scala
val a = "Hello"

for (i <- 0 until a.length)
  println(f"$i: ${a(i)}")
```

输出结果为：

```scala
0: H
1: e
2: l
3: l
4: o
```

until方法跟to方法很像，只不过它排除了最后一个元素。因此，变量i 的取值为从0到a.length-1。

如果想要每两个元素一跳，可以让i这样来进行遍历：

```scala
0 until a.length by 2
```

如果想要从数组的尾端开始，遍历的写法为：

```scala
0 until a.length by -1
```

除了上述方法外，还可以使用 `a.indices` 或者 `a.indices.reverse`。如果在循环体重不需要用到下标，也可以直接访问数组元素，就像这样：

```scala
val a = "Hello"

for (elem <- a)
  println(elem)
```

# 参考文献

https://docs.scala-lang.org/zh-cn/

https://www.tutorialspoint.com/scala/index.htm

http://horstmann.com/scala/