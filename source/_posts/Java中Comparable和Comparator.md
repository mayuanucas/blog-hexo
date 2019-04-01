---
title: Java中Comparable和Comparator
date: 2018-07-22 20:07:58
tags: 
- java
categories: 
- java基础知识
---

本文记录 Java 中 Comparable 和 Comparator 的知识点.

<!-- more -->

### Comparable 简介
Comparable 是排序接口。

若一个类实现了 Comparable 接口，就意味着 “该类支持排序”。  即然实现 Comparable 接口的类支持排序，假设现在存在 “实现 Comparable 接口的类的对象的 List 列表(或数组)”，则该 List 列表(或数组) 可以通过 Collections.sort（或 Arrays.sort）进行排序。

此外，“实现 Comparable 接口的类的对象”可以用作 “有序映射(如 TreeMap)” 中的键或 “有序集合(TreeSet)” 中的元素，而不需要指定比较器。

### Comparable 定义
```
public interface Comparable<T> {
    public int compareTo(T o);
}
```
说明：
假设我们通过 x.compareTo(y) 来 “比较 x 和 y 的大小”。若返回 “负数”，意味着 “x 比 y 小”；返回 “零”，意味着 “x 等于 y”；返回 “正数”，意味着 “x 大于 y”。

### Comparator 简介
Comparator 是比较器接口.

我们若需要控制某个类的次序，而该类本身没有实现 Comparable 接口；那么，我们可以建立一个“该类的比较器” 来进行排序。这个 “比较器” 只需要实现 Comparator 接口即可。

也就是说，我们可以通过 “实现 Comparator 类来新建一个比较器”，然后通过该比较器对类进行排序。

### Comparator 定义
```
public interface Comparator<T> {

    int compare(T o1, T o2);

    boolean equals(Object obj);
}
```
* 若一个类要实现 Comparator 接口：它一定要实现 compareTo(T o1, T o2) 函数，但可以不实现 equals(Object obj) 函数。
**为什么可以不实现 equals(Object obj) 函数呢？ 因为任何类，默认都是已经实现了 equals(Object obj) 的。 Java 中的一切类都是继承于 java.lang.Object，在 Object.java 中实现了 equals(Object obj) 函数；所以，其它所有的类也相当于都实现了该函数。**
* int compare(T o1, T o2) 是 “比较 o1 和 o2 的大小”。返回 “负数”，意味着 “o1 比 o2 小”；返回 “零”，意味着 “o1 等于 o2”；返回 “正数”，意味着 “o1 大于 o2”。

### Comparator 和 Comparable 比较
Comparable 是排序接口；若一个类实现了 Comparable 接口，就意味着 “该类支持排序”。
而 Comparator 是比较器；我们若需要控制某个类的次序，可以建立一个 “该类的比较器” 来进行排序。
不难发现：Comparable 相当于 “内部比较器”，而 Comparator 相当于 “外部比较器”。

### 测试
```
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

/**
 * @author: mayuan
 * @desc:
 * @date: 2018/07/22
 */
public class ComparatorAndComparable {

    public static void main(String[] args) {
        List<Person> list = new ArrayList<>(10);

        list.add(new Person("aa", 19));
        list.add(new Person("A", 9));
        list.add(new Person("b", 11));
        list.add(new Person("BB", 2));
        list.add(new Person("cC", 23));
        list.add(new Person("aA", 54));

        System.out.printf("原始列表顺序为: %s\n", list);

        // 根据 name 进行排序
        Collections.sort(list);
        System.out.printf("排序后列表顺序为: %s\n", list);

        Collections.sort(list, new AscAgeComparator());
        System.out.printf("根据 age 升序排序: %s\n", list);

        Collections.sort(list, new DescAgeComparator());
        System.out.printf("根据 age 降序排序: %s\n", list);

    }

    private static class Person implements Comparable<Person> {

        private int age;
        private String name;

        public Person() {

        }

        public Person(String s, int a) {
            this.age = a;
            this.name = s;
        }

        public int getAge() {
            return age;
        }

        public void setAge(int age) {
            this.age = age;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        @Override
        public String toString() {
            return name + "-" + age;
        }

        @Override
        public boolean equals(Object obj) {
            if (obj == this) {
                return true;
            }
            if (null != obj && (obj instanceof Person)) {
                if (this.age == ((Person) obj).age && this.name == ((Person) obj).name) {
                    return true;
                }
            }
            return false;
        }

        @Override
        public int compareTo(Person o) {
            return this.name.compareTo(o.name);
        }
    }

    private static class AscAgeComparator implements Comparator<Person> {
        @Override
        public int compare(Person p1, Person p2) {
            return p1.getAge() - p2.getAge();
        }
    }

    private static class DescAgeComparator implements Comparator<Person> {
        @Override
        public int compare(Person p1, Person p2) {
            return p2.getAge() - p1.getAge();
        }
    }
}
```

输出结果:
```
原始列表顺序为: [aa-19, A-9, b-11, BB-2, cC-23, aA-54]
排序后列表顺序为: [A-9, BB-2, aA-54, aa-19, b-11, cC-23]
根据 age 升序排序: [BB-2, A-9, b-11, aa-19, cC-23, aA-54]
根据 age 降序排序: [aA-54, cC-23, aa-19, b-11, A-9, BB-2]
```