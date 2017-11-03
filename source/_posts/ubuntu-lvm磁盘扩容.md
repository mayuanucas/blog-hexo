---
title: ubuntu-lvm磁盘扩容
date: 2017-11-03 10:59:50
tags: [linux, lvm]
categories: linux
---

本文记录Ubuntu16.04系统，在使用LVM情况下的磁盘扩容操作。

<!-- more -->

#### 虚拟机设置

关闭虚拟机的情况下，对虚拟机的磁盘扩展50G。接着打开虚拟机，登录系统进行操作。

#### 创建分区

在仅有一块磁盘的情况下，是对 /dev/sda 这个设备创建分区。
``` shell
sudo fdisk /dev/sda

n // 新建分区（按照默认操作或根据情况选择）
t // 改变分区类型（LVM的分区类型为： 8e）
w // 保存并退出

```
在以上操作完成后，使用
``` shell
sudo fdisk -l
```
就可以看到新创建的分区了。

#### 让新分区支持LVM逻辑卷管理器技术
(假设 新创建的分区为 sda4 大小为 50G)
``` shell
sudo pvcreate /dev/sda4
```

#### 把新的分区加入到卷组中
(此处的 卷组为 ubuntu-vg )
``` shell
sudo vgextend ubuntu-vg /dev/sda4
```

#### 逻辑卷扩展
(此处扩展的是 / 对应的挂载设备 /dev/ubuntu-vg/root)
``` shell
sudo lvextend -L +50G /dev/ubuntu-vg/root
```
{% asset_img img01.png %}


#### 重新识别磁盘容量
``` shell
sudo resize2fs /dev/ubuntu-vg/root
```
{% asset_img img02.png %}
