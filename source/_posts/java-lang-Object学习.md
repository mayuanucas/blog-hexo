---
title: java.lang.Object学习
date: 2018-07-20 19:29:24
tags: [Java, 序列化]
categories: Java基础知识
---

Java中的Object类是所有类的父类,在编译时会自动导入，位于java.lang包中，而Object中具有的属性和行为，是Java语言设计背后的思维体现,本文将对 Object 类进行探究详细.

<!-- more -->

### 环境说明
java version "1.8.0_152"
Java(TM) SE Runtime Environment (build 1.8.0_152-b16)
Java HotSpot(TM) 64-Bit Server VM (build 25.152-b16, mixed mode)

### 构造函数摘要
```
public Object(){}
```

### 方法摘要
Object类提供了以下11个方法:
```
protected Object clone()
boolean equals(Object obj)
protected void finalize()
Class<?> getClass()
int hashCode()
void notify()
void notifyAll()
String toString()
void wait()
void wait(long timeout)
void wait(long timeout, int nanos)
```

### 构造函数细节
```
public Object()
```

### 方法细节
#### getClass
```
// getClass方法是一个final方法，不允许子类重写，并且也是一个native方法.
// 返回当前运行时对象的Class对象.
public final native Class<?> getClass()

"abc".getClass()
// java.lang.String
```