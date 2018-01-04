---
title: Linux输入输出重定向
date: 2017-11-24 17:06:41
tags: linux
categories: linux
---

本文记录下Linux系统的输入输出重定向的知识点。
<!-- more -->

### linux shell下常用输入输出操作符是：
- 标准输入   (stdin) ：代码为 0 ，使用 < 或 << ； /dev/stdin -> /proc/self/fd/0   0代表：/dev/stdin
- 标准输出   (stdout)：代码为 1 ，使用 > 或 >> ； /dev/stdout -> /proc/self/fd/1  1代表：/dev/stdout
- 标准错误输出(stderr)：代码为 2 ，使用 2> 或 2>> ； /dev/stderr -> /proc/self/fd/2 2代表：/dev/stderr

### /dev/null ：代表空设备文件
>  ：代表重定向到哪里，例如：echo "123" > /home/123.txt
1  ：表示stdout标准输出，系统默认值是1，所以">/dev/null"等同于"1>/dev/null"
2  ：表示stderr标准错误
&  ：表示等同于的意思，2>&1，表示2的输出重定向等同于1

### 1 > /dev/null 2>&1 语句含义：
1 > /dev/null ： 首先表示标准输出重定向到空设备文件，也就是不输出任何信息到终端，说白了就是不显示任何信息。
2>&1 ：接着，标准错误输出重定向（等同于）标准输出，因为之前标准输出已经重定向到了空设备文件，所以标准错误输出也重定向到空设备文件。

### cmd >a 2>a 和 cmd >a 2>&1 为什么不同？
cmd >a 2>a ：stdout和stderr都直接送往文件 a ，a文件会被打开两遍，由此导致stdout和stderr互相覆盖。
cmd >a 2>&1 ：stdout直接送往文件a ，stderr是继承了FD1的管道之后，再被送往文件a 。a文件只被打开一遍，就是FD1将其打开。

#### 两者的不同点在于：

cmd >a 2>a 相当于使用了FD1、FD2两个互相竞争使用文件 a 的管道；
cmd >a 2>&1 只使用了一个管道FD1，但已经包括了stdout和stderr。
从IO效率上来讲，cmd >a 2>&1的效率更高。
