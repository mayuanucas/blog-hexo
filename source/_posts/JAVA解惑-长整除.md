---
title: JAVA解惑-长整除
date: 2018-10-08 11:00:39
tags: [java]
categories: [JAVA解惑]
---

JAVA解惑-长整除
<!-- more -->

## 长整除

下面的程序是有关两个 long 型数值整除的.被除数表示的是一天里的微秒数;而除数表示的是一天里的毫秒数.这个程序的输出是什么?

```java
public static void main(String[] args) {
    final long micros = 24 * 60 * 60 * 1000 * 1000;
    final long millis = 24 * 60 * 60 * 1000;

    // 输出 5
    System.out.println(micros / millis);
}
```

出现这个问题的原因在于 micros 的计算确实溢出了.尽管计算的结果在 long 类型的可表示范围,但是计算的中间结果是以 int 运算来执行的,并且只有在计算完成之后,其结果才被提升到 long ,
但是这已经太迟了,计算已经溢出!

## 解决方案

通过使用 long 常量来替代 int 常量作为每一个乘积的第一个因子,就可以很容易地订正这个程序.这样可以强制表达式的所有后续计算都用 long 运算来完成.

```java
public static void main(String[] args) {
    final long micros = 24L * 60 * 60 * 1000 * 1000;
    final long millis = 24L * 60 * 60 * 1000;

    // 输出 5
    System.out.println(micros / millis);
}
```

总结:在操作很大的数字时,千万要提防溢出!