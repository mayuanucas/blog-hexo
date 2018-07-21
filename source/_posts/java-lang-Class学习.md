---
title: java.lang.Class学习
date: 2018-07-21 10:34:39
tags: [Java]
categories: Java基础知识
---

本文对Java中的Class 类进行深入的学习.

<!-- more -->

### Class Class<T>
java.lang.Object
----java.lang.Class<T>
```
public final class Class<T> 
extends Object
implements Serializable, GenericDeclaration, Type, AnnotatedElement
```
参数类型:
T - 由此 Class 对象建模的类的类型.例如: String.class 的类型是 Class<String> .如果建模的类的类型未知,使用 Class<?>

类Class的实例表示正在运行的Java应用程序中的类和接口。 枚举是一种类，注解是一种接口。 每个数组也属于一个类，它反映为一个Class对象，由具有相同元素类型和维数的所有数组共享。 原始Java类型（boolean，byte，char，short，int，long，float和double）以及关键字void也表示为Class对象。

Class没有公共构造函数。 相反，Class对象由Java虚拟机在加载类时自动构造，并通过调用类加载器中的defineClass方法。

```
public class MyClass {

    public static void main(String[] args) {
        // 类名.class叫做“类字面量”，因class是关键字, 所以类名.class编译时确定
        System.out.println("The class of String is: " + String.class.getName());
        printClassName(9);
    }

    // getclass()运行时根据实际实例确定，getClass()是动态而且是final的
    public static void printClassName(Object obj) {
        System.out.println("The class of " + obj +
                " is: " + obj.getClass().getName());
    }
}
```

### 示例
```
public class MyClass {

    public MyClass(){}

    public MyClass(String str){

    }

    public static void main(String[] args) {
        System.out.println("The class of String is: " + String.class.getName());
        System.out.println("The class of void is: " + void.class.getName());
        printClassName(9);

        Class<MyClass> c1 = MyClass.class;
        try {
            MyClass myClass1 = c1.newInstance();
            myClass1.printClassName(3.14);
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        }
    }

    public static void printClassName(Object obj) {
        System.out.println("The class of " + obj +
                " is: " + obj.getClass().getName());
    }
}
```

运行输出:
```
The class of String is: java.lang.String
The class of void is: void
The class of 9 is: java.lang.Integer
The class of 3.14 is: java.lang.Double
```