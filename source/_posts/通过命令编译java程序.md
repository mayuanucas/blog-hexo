---
title: 通过命令编译java程序
date: 2017-09-04 20:57:01
tags: 
- java, 编译, 
- shell
categories: 
- java基础知识
---
 写本文的初衷是回顾一下java的编译命令, 并记录在linux系统上，通过java命令运行程序。
 <!-- more -->

 ##### 编译并执行单个文件
 - 在目录下~/workspace/utils/com/libai 下新建HelloWorld.java的文件，文件内容为:
 ``` java
package com.libai;

public class HellowWorld {
    public static void main(String[] args) {
        System.out.println("hello world");
    }
 }
 ```
- 在目录~/workspace/utils下执行
``` bash
javac com/libai/HelloWorld.java
```
命令来编译文件。此时会在HelloWorld.java文件所在的目录下生成HelloWorld.class的二进制文件。

- 在目录~/workspace/utils下执行
``` bash
java com.libai.HelloWorld
```
来执行HelloWorld.class。屏幕会输出hello world，说明文件执行成功。

- 也可以在任意路径下指定classpath路径来执行，命令为:
``` bash
java -classpath ~/workspace/utils com.libai.HelloWorld
```
其中classpath指定了类的搜索路径。

##### 打包
将上述例子中的程序打成jar包，可以在~/workspace/utils目录下通过执行命令
``` bash
jar cvf my.jar com
```
来生成jar文件。其中my.jar为要生成的jar文件的名字。通过
``` bash
java -classpath my.jar com.libai.HelloWorld
```
来执行jar文件。
