---
title: Linux-screen命令详解
date: 2017-11-02 14:44:04
tags: 
- linux
categories: 
- linux
---

Screen是一个可以在多个进程之间多路复用一个物理终端的全屏窗口管理器。Screen中有会话的概念，用户可以在一个会话中创建多个screen窗口，在每一个screen窗口中就像操作一个真实的telnet/SSH连接窗口那样。
通俗的讲，screen命令用于新建一个或多个“命令行窗口”，在新建的这“窗口”中，可以执行命令；每个“窗口”都是独立并行的。

<!-- more -->

{% asset_img img01.png %}
