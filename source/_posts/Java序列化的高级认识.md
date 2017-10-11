---
title: Java序列化的高级认识
date: 2017-10-11 10:17:00
tags: [java, 序列化]
categories: java
---

将 Java 对象序列化为二进制文件的 Java 序列化技术是 Java 系列技术中一个较为重要的技术点，在大部分情况下，开发人员只需要了解被序列化的类需要实现 Serializable 接口，使用 ObjectInputStream 和 ObjectOutputStream 进行对象的读写。然而在有些情况下，光知道这些还远远不够，本文记录java序列化的高级知识点。

<!-- more -->
{% asset_img xuliehua.png Java序列化高级认识%}
