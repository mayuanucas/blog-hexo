---
title: JAVA解惑-找零时刻
date: 2018-10-07 21:04:36
tags: [Java]
categories: [JAVA解惑]
---

JAVA解惑-找零时刻

<!-- more -->

## 找零时刻

Tom用两美元在商店购买了一个价值$1.10的火花塞,那么该找零多少呢?
以下代码,你觉得会输出什么?

```java
public static void main(String[] args){
    // 输出 0.8999999999999999
    System.out.println(2.00 - 1.10);
}
```

**很遗憾,该程序的输出并不是0.90.**如何才能输出准确的正确的数值?

问题在于1.1这个数字不能被精确表示成为一个double,因此它被表示成为最接近它的double值.该程序从2中减去的就是这个值.

## 解决方案

### 使用某种整数类型

例如 int 或 long,并且以分为单位来执行计算.如果采用此路线,请确保该整数类型大道足够表示在程序中你将要用到的所有值.

```java
System.out.println((200 - 110) + "cents);
```

### 使用执行小数运算的 BigDecimal

**一定要用BigDecimal(String)构造器,而千万不要用BigDecimal(double)!**.
后一个构造器将用它的参数的"精确"值来创建一个实例: new BigDecimal(0.1) 将返回一个表示 0.1000000000000000055511151231257827021181583404541015625 的 BigDecimal,程序就可以打印出我们所期望的结果0.9:

```java
public static void main(String[] args){
    System.out.println(new BigDecimal("2.00")).subtract(new BigDecimal("1.10"));
}
```

这个版本并不十分完美,因为 Java 并没有为 BigDecimal 提供任何语言上的支持.使用 BigDecimal 的计算很有可能比那些使用原始类型的计算要慢一些,对某些大量使用小数计算的程序来说,这可能成为问题.

总之,在需要精确答案的地方,要避免使用 float 和 double;对于货币结算,要使用 int long BigDecimal.