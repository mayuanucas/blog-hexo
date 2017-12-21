---
title: Mac安装rar和unrar
date: 2017-12-21 21:19:53
tags: [mac, rar, unrar]
categories: mac
---

本文记录如何在mac上安装rar和unrar。
<!-- more -->
Mac上默认的压缩无法指定把压缩的文件进行分片，并指定每个分片压缩包的大小。但是通过rar命令行的方式进行压缩的，就可以实现。

### 安装
**从rar官网下载最新版的压缩文件**
下载地址：https://www.rarlab.com/download.htm

### 解压并安装
把下载完的压缩包，解压。默认生成名为rar的文件夹。
```shell
cd rar

sudo install -c -o $USER rar /usr/local/bin

sudo install -c -o $USER unrar /usr/local/bin
```

### 测试安装
```shell
rar

unrar

```
出现使用说明，即为安装成功。
