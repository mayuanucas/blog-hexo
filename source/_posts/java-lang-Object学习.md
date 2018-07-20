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

#### hashCode
```
// 该方法返回对象的哈希码，主要使用在哈希表中
public native int hashCode();
```

#### equals
```
// 比较两个对象是否相等。Object类的默认实现，即比较2个对象的内存地址是否相等
// 如果重写了equals方法，通常有必要重写hashCode方法
public boolean equals(Object obj) {
    return (this == obj);
}
```

#### clone
```
// 创建并返回当前对象的一份拷贝
// 一般情况下，对于任何对象 x，表达式 x.clone() != x 为true，
// x.clone().getClass() == x.getClass() 为true
protected native Object clone() throws CloneNotSupportedException;
```

#### toString
```
// Object对象的默认实现，即输出类的名字@实例的哈希码的16进制
public String toString() {
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
```

#### notify
```
// 唤醒正在此对象监视器上等待的单个线程。 如果所有线程都正在等待此对象，则选择其中一个线程被唤醒。 
// 选择是任意的，由实施决定。 线程通过调用其中一个wait方法等待对象的监视器。
public final native void notify();
```

#### notifyAll
```
// 唤醒在此对象监视器上等待的所有线程。
public final native void notifyAll();
```

#### wait
```
// wait方法会让当前线程等待直到另外一个线程调用对象的notify或notifyAll方法，或者超过参数设置的timeout超时时间
public final native void wait(long timeout) throws InterruptedException;
```

#### wait
```
// wait方法会让当前线程等待直到另外一个线程调用对象的notify或notifyAll方法，或者超过参数设置的timeout超时时间
// nanos参数表示额外时间（以毫微秒为单位，范围是 0-999999)
public final native void wait(long timeout,
                            int nanos) throws InterruptedException;
```

#### wait
```
// 跟之前的2个wait方法一样，只不过该方法一直等待，没有超时时间这个概念
// wait方法和notify方法会一起使用的，wait方法阻塞当前线程，notify方法唤醒当前线程
public final void wait()
                throws InterruptedException
```

#### finalize
```
// 该方法的作用是实例被垃圾回收器回收的时候触发的操作 (最后的挣扎)
// 当垃圾收集确定没有对该对象的更多引用时，由对象上的垃圾收集器调用。 
// 子类重写finalize方法以处置系统资源或执行其他清理。
protected void finalize() throws Throwable { }
```