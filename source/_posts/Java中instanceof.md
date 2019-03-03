---
title: Java中instanceof
date: 2018-07-22 22:33:08
tags: [java]
categories: Java基础知识
---

本文记录Java中instanceof运算符的一些知识点.

<!-- more -->

### 概述
Java中的instanceof运算符用来判断一个对象是否属于指定的类以及该类的后代类，也可以判断一个对象所属的类是否实现了指定的接口，如果属于返回true，否则返回false；

instanceof运算符的使用：
```
obj instanceof ClassName/Interface;
```
需要注意的是：obj一般是存放对象引用的对象变量，而instanceof运算符是将obj中引用的对象实例的类型与指定类型ClassName进行比较；如果obj中存放的是类A的实例对象的引用，则instanceof运算符将比较类A是否与类名为ClassName的类是同一个类，或者类A是否继承于类名为ClassName的类，即是否为类名为ClassName的类的后代类，或者obj是否为实现了接口Interface的类的实例，如果符合上述条件之一，则返回结果为true；否则返回false.

### 总结
* 若左操作数是 null 则结果直接返回 false，不再运算右操作数是什么类.
* instanceof 只能用于对象的判断，不能用于基本类型的判断.
* instanceof 操作符的左右操作数必须有继承或实现关系，否则编译会失败.
* instanceof 的右操作符必须是一个接口或者类，如果是 null ,则编译不通过.
* 数组类型也可以使用 instanceof 来判断

###  Java 的 instanceof 与 obj.instanceof(class) 的区别
java 中的 instanceof 运算符用来在运行时指出对象是否是特定类的一个实例，通过返回一个布尔值来指出这个对象是否是这个特定类或者是它的子类的一个实例；用法为 result = object instanceof class，参数 result 布尔类型，object 为必选项的实例，class 为必选项的任意已定义的对象类，如果 object 是 class 的一个实例则 instanceof 运算符返回 true，如果 object 不是指定类的一个实例或者 object 是 null 则返回 false；但是 instanceof 在 java 的编译状态和运行状态是有区别的，在编译状态中 class 可以是 object 对象的父类、自身类、子类，在这三种情况下 java 编译时不会报错，在运行转态中 class 可以是 object 对象的父类、自身类但不能是子类，当为父类、自生类的情况下 result 结果为 true，为子类的情况下为 false。

clazz.isInstance(obj) 表示这个对象能不能被转化为这个类，一个对象是本身类的一个对象，一个对象能被转化为本身类所继承类（父类的父类等）和实现的接口（接口的父接口）强转，所有对象都能被 Object 强转，当 obj 参数为 null 时一定为 false。
```
    class A {}

    class B extends A {}

    B b = new B();

    A a = new A();

    A ba = new B();

    b instanceof B;    //true

    b instanceof A;    //true

    b instanceof Object;    //true

    null instanceof Object;    //false

    b.getClass().isInstance(b);    //true

    b.getClass().isInstance(a);    //false

    a.getClass().isInstance(ba);    //true

    b.getClass().isInstance(ba);    //true

    b.getClass().isInstance(null);    //false

    A.class.isInstance(a);    //true

    A.class.isInstance(b);    //true

    A.class.isInstance(ba);    //true

    B.class.isInstance(a);    //false

    B.class.isInstance(b);    //true

    B.class.isInstance(ba);    //true

    Object.class.isInstance(b);    //true

    Object.class.isInstance(null);    //false
```

### Java 的 instanceof 与 clazz.getClass() 的区别
instanceof 进行类型检查规则是你属于该类吗？或者你属于该类的派生类吗？而 obj.getClass() 获得类型信息采用 == 来进行检查是否相等的操作是严格比较，不存在继承方面的考虑。
```
    class A {}

    class B extends A {}

    new A() instanceof A;    //true

    new A() instanceof B;    //false

    new A().getClass() == A.class;    //true

    new A().getClass() == B.class;    //false

    new B() instanceof A;    //true

    new B() instanceof B;    //true

    new B().getClass() == A.class;    //false

    new B().getClass() == B.class;    //true
```

###  Java 的 instanceof 实现原理
JVM有一条名为 instanceof 的指令，而Java源码编译到Class文件时会把Java语言中的 instanceof 运算符映射到JVM的 instanceof 指令上。链接：https://www.zhihu.com/question/21574535/answer/18998914
