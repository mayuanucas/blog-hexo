---
title: hash算法
date: 2017-08-09 08:40:46
categories: hash
tags:
- hash

---

2017年2月23日谷歌在[blog](https://security.googleblog.com/2017/02/announcing-first-sha1-collision.html )上宣布实现了SHA-1的碰撞,将会在90天内公开算法。之前实现暴力破解需要12，000，000个gpu算一年，现在需要110个GPU算一年，破解效率大为提高。
SHA-1的安全性一直广受质疑，此次破解对于广泛使用SHA-1算法的协议如TTL，SSL，SSH都有影响。谷歌和微软已经在2016年停止了支持SHA-1的网站安全证书。那么此次破解都有些什么什么意义，对业界的影响有多大呢？

<!-- more -->
{% asset_img hash01.png  hash%}
