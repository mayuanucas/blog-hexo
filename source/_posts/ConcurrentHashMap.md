---
title: ConcurrentHashMap
date: 2018-08-28 20:12:26
tags: [Java]
categories: [Java基础知识]
---

本文主要记录 ConcurrentHashMap 相关知识点。
<!-- more -->

## 安全的几种使用方式
在了解了 HashMap 为什么不安全之后，现在看看如何安全地使用 HashMap。有以下几种方式：

* HashTable
* ConcurrentHashMap
* SynchronizedMap

## HashTable
HashTable 源码中是使用 synchronized 关键字修饰方法来保证线程安全的。所以当一个线程访问 HashTable 的同步方法时，其他线程如果也要访问同步方法，会被阻塞住。举个例子，当一个线程使用 put 方法时，另一个线程不但不可以使用 put 方法，连 get 方法都不可以！so~~，效率很低，现在基本不会选择它了。

## SynchronizedMap

```java
// synchronizedMap方法
public static <K,V> Map<K,V> synchronizedMap(Map<K,V> m) {
       return new SynchronizedMap<>(m);
   }
// SynchronizedMap类
private static class SynchronizedMap<K,V>
       implements Map<K,V>, Serializable {
       private static final long serialVersionUID = 1978198479659022715L;
       private final Map<K,V> m;     // Backing Map
       final Object      mutex;        // Object on which to synchronize
       SynchronizedMap(Map<K,V> m) {
           this.m = Objects.requireNonNull(m);
           mutex = this;
       }
       SynchronizedMap(Map<K,V> m, Object mutex) {
           this.m = m;
           this.mutex = mutex;
       }
       public int size() {
           synchronized (mutex) {return m.size();}
       }
       public boolean isEmpty() {
           synchronized (mutex) {return m.isEmpty();}
       }
       public boolean containsKey(Object key) {
           synchronized (mutex) {return m.containsKey(key);}
       }
       public boolean containsValue(Object value) {
           synchronized (mutex) {return m.containsValue(value);}
       }
       public V get(Object key) {
           synchronized (mutex) {return m.get(key);}
       }
       public V put(K key, V value) {
           synchronized (mutex) {return m.put(key, value);}
       }
       public V remove(Object key) {
           synchronized (mutex) {return m.remove(key);}
       }
       // 省略其他方法
   }
```

从源码中可以看出调用 synchronizedMap() 方法后会返回一个 SynchronizedMap 类的对象，而在 SynchronizedMap 类中使用了 synchronized 同步关键字来保证对 Map 的操作是线程安全的。

## ConcurrentHashMap

### JDK1.7

先来看看 1.7 的实现，下面是它的结构图：
{% asset_img img01.jpg%}
如图所示，是由 Segment 数组、HashEntry 组成，和 HashMap 一样，仍然是数组加链表。
和 HashMap 非常类似，唯一的区别就是其中的核心数据如 value ，以及链表都是 volatile 修饰的，保证了获取时的可见性。

原理上来说：ConcurrentHashMap 采用了分段锁技术，其中 Segment 继承于 ReentrantLock。不会像 HashTable 那样不管是 put 还是 get 操作都需要做同步处理，理论上 ConcurrentHashMap 支持 CurrencyLevel (Segment 数组数量)的线程并发。每当一个线程占用锁访问一个 Segment 时，不会影响到其他的 Segment。

#### put 方法

```java
public V put(K key, V value) {
    Segment<K,V> s;
    if (value == null)
        throw new NullPointerException();
    int hash = hash(key);
    int j = (hash >>> segmentShift) & segmentMask;
    if ((s = (Segment<K,V>)UNSAFE.getObject          // nonvolatile; recheck
         (segments, (j << SSHIFT) + SBASE)) == null) //  in ensureSegment
        s = ensureSegment(j);
    return s.put(key, hash, value, false);
}
```
首先是通过 key 定位到 Segment，之后在对应的 Segment 中进行具体的 put。虽然 HashEntry 中的 value 是用 volatile 关键词修饰的，但是并不能保证并发的原子性，所以 put 操作时仍然需要加锁处理。

#### get 方法

```java
public V get(Object key) {
    Segment<K,V> s; // manually integrate access methods to reduce overhead
    HashEntry<K,V>[] tab;
    int h = hash(key);
    long u = (((h >>> segmentShift) & segmentMask) << SSHIFT) + SBASE;
    if ((s = (Segment<K,V>)UNSAFE.getObjectVolatile(segments, u)) != null &&
        (tab = s.table) != null) {
        for (HashEntry<K,V> e = (HashEntry<K,V>) UNSAFE.getObjectVolatile
                 (tab, ((long)(((tab.length - 1) & h)) << TSHIFT) + TBASE);
             e != null; e = e.next) {
            K k;
            if ((k = e.key) == key || (e.hash == h && key.equals(k)))
                return e.value;
        }
    }
    return null;
}
```

get 逻辑比较简单：
只需要将 key 通过 Hash 之后定位到具体的 Segment ，再通过一次 Hash 定位到具体的元素上。
由于 HashEntry 中的 value 属性是用 volatile 关键词修饰的，保证了内存可见性，所以每次获取时都是最新值。
ConcurrentHashMap 的 get 方法是非常高效的，因为整个过程都不需要加锁。

### JDK1.8

再来看看jdk1.8的实现:
底层的组成结构:
{% asset_img img02.jpg %}

在JDK1.8中，**抛弃了原有的 Segment 分段锁，而采用了 CAS + synchronized 来保证并发安全性**
也将 1.7 中存放数据的 HashEntry 改为 Node，

#### put 方法

```java
public V put(K key, V value) {
        return putVal(key, value, false);
    }

final V putVal(K key, V value, boolean onlyIfAbsent) {
        if (key == null || value == null) throw new NullPointerException();
        int hash = spread(key.hashCode());
        int binCount = 0;
        for (Node<K,V>[] tab = table;;) {
            Node<K,V> f; int n, i, fh;
            if (tab == null || (n = tab.length) == 0)
                tab = initTable();
            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
                if (casTabAt(tab, i, null,
                             new Node<K,V>(hash, key, value, null)))
                    break;                   // no lock when adding to empty bin
            }
            else if ((fh = f.hash) == MOVED)
                tab = helpTransfer(tab, f);
            else {
                V oldVal = null;
                synchronized (f) {
                    if (tabAt(tab, i) == f) {
                        if (fh >= 0) {
                            binCount = 1;
                            for (Node<K,V> e = f;; ++binCount) {
                                K ek;
                                if (e.hash == hash &&
                                    ((ek = e.key) == key ||
                                     (ek != null && key.equals(ek)))) {
                                    oldVal = e.val;
                                    if (!onlyIfAbsent)
                                        e.val = value;
                                    break;
                                }
                                Node<K,V> pred = e;
                                if ((e = e.next) == null) {
                                    pred.next = new Node<K,V>(hash, key,
                                                              value, null);
                                    break;
                                }
                            }
                        }
                        else if (f instanceof TreeBin) {
                            Node<K,V> p;
                            binCount = 2;
                            if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,
                                                           value)) != null) {
                                oldVal = p.val;
                                if (!onlyIfAbsent)
                                    p.val = value;
                            }
                        }
                    }
                }
                if (binCount != 0) {
                    if (binCount >= TREEIFY_THRESHOLD)
                        treeifyBin(tab, i);
                    if (oldVal != null)
                        return oldVal;
                    break;
                }
            }
        }
        addCount(1L, binCount);
        return null;
    }
```

* 根据 key 计算出 hashcode
* 判断是否需要进行初始化
* f 即为当前 key 定位出的 Node，如果为空表示当前位置可以写入数据，利用 CAS 尝试写入，失败则自旋保证成功
* 如果当前位置的 hashcode == MOVED == -1,则需要进行扩容
* 如果都不满足，则利用 synchronized 锁写入数据
* 如果数量大于 TREEIFY_THRESHOLD 则要转换为红黑树

#### get 方法

```java
public V get(Object key) {
        Node<K,V>[] tab; Node<K,V> e, p; int n, eh; K ek;
        int h = spread(key.hashCode());
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (e = tabAt(tab, (n - 1) & h)) != null) {
            if ((eh = e.hash) == h) {
                if ((ek = e.key) == key || (ek != null && key.equals(ek)))
                    return e.val;
            }
            else if (eh < 0)
                return (p = e.find(h, key)) != null ? p.val : null;
            while ((e = e.next) != null) {
                if (e.hash == h &&
                    ((ek = e.key) == key || (ek != null && key.equals(ek))))
                    return e.val;
            }
        }
        return null;
    }
```

* 根据计算出来的 hashcode 寻址，如果就在桶上那么直接返回值
* 如果是红黑树那就按照树的方式获取值
* 就不满足那就按照链表的方式遍历获取值

>1.8 在 1.7 的数据结构上做了大的改动，采用红黑树之后可以保证查询效率（O(logn)），甚至取消了 ReentrantLock 改为了 synchronized，这样可以看出在新版的 JDK 中对 synchronized 优化是很到位的。