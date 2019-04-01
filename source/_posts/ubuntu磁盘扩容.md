---
title: ubuntu磁盘扩容
date: 2017-11-03 13:47:46
tags: 
- linux
categories: 
- linux
---

本文记录Ubuntu16.04系统，在使用LVM情况下的磁盘扩容操作。

<!-- more -->

### 常用的LVM部署命令

| 功能/命令 | 物理卷管理 | 卷组管理 | 逻辑卷管理 |
|---|---|---|---|
| 扫描 | pvscan | vgscan | lvscan |
| 建立 | pvcreate | vgcreate | lvcreate |
| 显示 | pvdisplay | vgdisplay | lvdisplay |
| 删除 | pvremove | vgremove | lvremove |
| 扩展 |   | vgextend | lvextend |
| 缩小 |   | vgreduce | lvreduce |

#### 虚拟机设置

关闭虚拟机的情况下，对虚拟机的磁盘扩展50G。接着打开虚拟机，登录系统进行操作。

#### 创建分区

在仅有一块磁盘的情况下，是对 /dev/sda 这个设备创建分区。
```
sudo fdisk /dev/sda

n // 新建分区（按照默认操作或根据情况选择）
t // 改变分区类型（LVM的分区类型为： 8e）
w // 保存并退出

```
在以上操作完成后，使用
```
sudo fdisk -l
```
就可以看到新创建的分区了。

#### 让新分区支持LVM逻辑卷管理器技术

```
sudo pvcreate /dev/sda4
```

#### 把新的分区加入到卷组中

(此处的 卷组为 ubuntu-vg )

```
sudo vgextend ubuntu-vg /dev/sda4
```

#### 逻辑卷扩展

(此处扩展的是 / 对应的挂载设备 /dev/ubuntu-vg/root)
(假设 新创建的分区为 sda4 大小为 50G)

```
sudo lvextend -L +50G /dev/ubuntu-vg/root
# 检查硬盘完整性
sudo e2fsck -f /dev/ubuntu-vg/root
```
{% asset_img img01.png %}


#### 重新识别磁盘容量

```
sudo resize2fs /dev/ubuntu-vg/root
```
{% asset_img img02.png %}

### 例2：添加两块新硬盘设备（ /dev/sdb /dev/sdc ）

#### 第1步：让新添加的两块硬盘设备支持LVM技术
```
sudo pvcreate /dev/sdb /dev/sdc
```

#### 第2步：把两块硬盘设备加入到storage卷组中，然后查看卷组的状态
```
sudo vgcreate storage /dev/sdb /dev/sdc
```

#### 第3步：切割出逻辑卷设备
```
lvcreate -n vo -L 1024G storage
```

#### 第4步：把生成好的逻辑卷进行格式化，然后挂载使用
Linux系统会把LVM中的逻辑卷设备存放在/dev设备目录中（实际上是做了一个符号链接），同时会以卷组的名称来建立一个目录，其中保存了逻辑卷的设备映射文件（即/dev/卷组名称/逻辑卷名称）。
```
sudo mkfs.ext4 /dev/storage/vo
sudo mkdir /mydata
sudo mount /dev/storage/vo /mydata
```

#### 第5步：查看挂载状态，并写入到配置文件，使其永久生效
```
sudo df -h
----------------
sudo echo "/dev/storage/vo /mydata ext4 defaults 0 0" >> /etc/fstab
```
