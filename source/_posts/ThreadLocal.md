---
title: ThreadLocal
date: 2018-08-29 21:25:15
tags: [Java]
categories: [Java基础知识]
---

本文记录 Java 中 ThreadLocal 相关知识点。

<!-- more -->

## ThreadLocal

ThreadLocal 用于提供线程局部变量，在多线程环境可以保证各个线程里的变量独立于其它线程里的变量。ThreadLocal 为每个线程创建一个【单独的变量副本】，相当于线程的 private static 类型变量。

ThreadLocal 的作用和同步机制有些相反：同步机制是为了保证多线程环境下数据的一致性；而 ThreadLocal 是保证了多线程环境下数据的独立性。

对于 ThreadLocal 类型的变量，在一个线程中设置值，不影响其在其它线程中的值。也就是说 ThreadLocal 类型的变量的值在每个线程中是独立的。

## set(T value) 方法

```java
public void set(T value) {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null)
            map.set(this, value);
        else
            createMap(t, value);
    }
```

ThreadLocalMap 相当于一个 HashMap，是真正保存值的地方。

## get() 方法

```java
public T get() {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null) {
            ThreadLocalMap.Entry e = map.getEntry(this);
            if (e != null) {
                @SuppressWarnings("unchecked")
                T result = (T)e.value;
                return result;
            }
        }
        return setInitialValue();
    }
```

在 get() 方法中也会获取到当前线程的 ThreadLocalMap，如果 ThreadLocalMap 不为 null，则获取 map.getEntry 为当前 ThreadLocal 的值；否则调用 setInitialValue() 方法返回初始值，并保存到新创建的 ThreadLocalMap 中。

## setInitialValue() 方法

```java
private T setInitialValue() {
        T value = initialValue();
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null)
            map.set(this, value);
        else
            createMap(t, value);
        return value;
    }

protected T initialValue() {
        return null;
    }
```

initialValue() 是 ThreadLocal 的初始值，默认返回 null，子类可以重写改方法，用于设置 ThreadLocal 的初始值。

## remove() 方法

```java
public void remove() {
         ThreadLocalMap m = getMap(Thread.currentThread());
         if (m != null)
             m.remove(this);
     }
```

在使用 ThreadLocal 类型变量进行相关操作时，都会通过当前线程获取到 ThreadLocalMap 来完成操作。每个线程的 ThreadLocalMap 是属于线程自己的，ThreadLocalMap 中维护的值也是属于线程自己的。这就保证了 ThreadLocal 类型的变量在每个线程中是独立的，在多线程环境下不会相互影响。
