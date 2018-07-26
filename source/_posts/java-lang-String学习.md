---
title: java.lang.String学习
date: 2018-07-26 22:16:01
tags: [Java]
categories: Java基础知识
---

本文记录对 Java 中 String 类的知识点.
<!-- more -->

### String不是基本数据类型
**首先String不属于8种基本数据类型，String是一个类.** 因为对象的默认值是null,所以 String 的默认值也是null,但它又是一种特殊的类.有其它类没有的一些特性.

### String的构造函数
new String()和new String("")都是申明一个新的空字符串,是空串而不是null.

### 常量池(constant pool)
常量池(constant pool)指的是在编译期被确定,并被保存在已编译的.class文件中的一些数据.它包括了关于类、方法、接口等中的常量，也包括字符串常量.
```
String s1 = "abcd";
String s2 = "abcd";
String s3 = "ab" + "cd";
// true
System.out.println(s1 == s2);
// true
System.out.println(s2 == s3);
```
首先，我们要知道Java会确保一个字符串常量只有一个拷贝.因为例子中的s1和s2中的”abcd”都是字符串常量，它们在编译期就被确定了，所以s0==s1为true；而”ab”和”cd”也都是字符串常量，当一个字符串由多个字符串常量连接而成时，它自己肯定也是字符串常量，所以s3也同样在编译期就被解析为一个字符串常量，所以s3也是常量池中”abcd”的一个引用.所以我们得出s1==s2==s3.

用new String() 创建的字符串不是常量，不能在编译期就确定，所以new String() 创建的字符串不放入常量池中，它们存放在堆上。
```
String s1 = "abcd";
// s2 是运行时创建的对象”abcd”的引用,无法在编译期确定.
String s2 = new String("abcd");
// s3 因为有后半部分的 new String("cd"),所以无法在编译期确定.
String s3 = "ab" + new String("cd");
// false
System.out.println(s1 == s2);
// false
System.out.println(s1 == s3);
// false
System.out.println(s2 == s3);
```

### String.intern()
```
public native String intern();
Returns a canonical representation for the string object.
A pool of strings, initially empty, is maintained privately by the class String.

When the intern method is invoked, if the pool already contains a string equal to this String object as determined by the equals(Object) method, then the string from the pool is returned. Otherwise, this String object is added to the pool and a reference to this String object is returned.

It follows that for any two strings s and t, s.intern() == t.intern() is true if and only if s.equals(t) is true.

All literal strings and string-valued constant expressions are interned. String literals are defined in section 3.10.5 of the The Java™ Language Specification.

Returns:
a string that has the same contents as this string, but is guaranteed to be from a pool of unique strings.
```
根据JDK文档的说明，这个方法是一个 native 的方法，注释写的非常明了。“如果常量池中存在当前字符串, 就会直接返回当前字符串. 如果常量池中没有此字符串, 会将此字符串放入常量池中后, 再返回”。

实际代码例子:
```
public class MyString {

    public static void main(String[] args) {
        String s1 = new String("aaa");
        String s2 = "aaa";
        // false
        System.out.println(s1 == s2);

        s1 = new String("bbb").intern();
        s2 = "bbb";
        // true
        System.out.println(s1 == s2);

        s1 = new String("ccc").intern();
        s2 = new String("ccc").intern();
        // true
        System.out.println(s1 == s2);

        s1 = "abcd";
        s2 = "ab" + "cd";
        // true
        System.out.println(s1 == s2);

        String temp = "hh";
        s1 = "a" + temp;
        s2 = "ahh";
        // false
        System.out.println(s1 == s2);

        temp = "hh";
        s1 = ("a" + temp).intern();
        s2 = "ahh";
        // true
        System.out.println(s1 == s2);

        // 同时会生成堆中的对象 以及 常量池中1的对象，但是此时s1是指向堆中的对象的
        s1 = new String("1");
        // 字符串常量池中已经存在 字符串"1"
        s1.intern();
        s2 = "1";
        // false
        System.out.println(s1 == s2);

        // 此时生成了四个对象->常量池中的"1" + 2个堆中的"1" + s3指向的堆中的对象（注此时常量池不会生成"11")
        String s3 = new String("1") + new String("1");
        // jdk1.7之后，常量池不仅仅可以存储对象，还可以存储对象的引用，此时会直接将s3的地址存储在常量池
        s3.intern();
        // jdk1.7之后，常量池中的地址其实就是s3的地址, 此时 s4 的值为 s3 的地址
        String s4 = "11";
        // jdk1.7之前false， jdk1.7之后true
        System.out.println(s3 == s4);

        s3 = new String("2") + new String("2");
        // 常量池中不存在字符串"22"，所以会新开辟一个存储"22"对象的常量池地址
        s4 = "22";
        // 字符串常量池中已经存在字符串"22",所以会返回"22"的引用(但此处并没有更新s3,s3依然是堆上对象的引用)
        s3.intern();
        // false
        System.out.println(s3 == s4);

    }
}
```
