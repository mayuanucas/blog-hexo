---
title: JAVA解惑-奇数性
date: 2018-10-06 23:07:24
tags: 
- java
categories: 
- java基础知识
---

JAVA解惑-奇数性

<!-- more -->

## 奇数性

下面的方法想要确定参数是否为奇数，该方法能够正常运行吗？

```java
public static boolean isOdd(int i){
    return 1 == i%2;
}
```

运行这段代码会发现，对于所有的负奇数，判断的结果都是错误的。>>>奇数可以被定义为被2整除余数为1的整数。
在所有的 int 数值中，有一半都是负数，而 isOdd 方法对于所有的负奇数的判断都会失败。
这是 Java 对取余运算符的定义所产生的后果。该操作符被定义为所有的 int 数值 a 和 所有的非零 int 数值 b, 都满足下面的恒等式:

```java
(a / b) * b + (a % b) == a
```

**当 i 是一个负奇数时， i%2 等于 -1 而不是1**,因此 isOdd方法将错误地返回 false.

## 解决方案

### 反转比较的含义

只需将 i%2 与 0 进行比较,并且反转比较的含义即可:

```java
public static boolean isOdd(int i){
    return 0 != i%2;
}
```

### 采用性能更好的与操作

```java
public static boolean isOdd(int i){
    return 0 != (i&1);
}
```