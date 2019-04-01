---
title: aria2的配置文件&启动脚本
date: 2017-11-02 14:46:52
tags:
- linux
- 效率
categories: 
- 效率工具
---

本文介绍 aria2 的使用.

<!-- more -->

#### 基础

首先，aria2或者叫做aria2c，它是一个下载器，嗯。
常用的两种模式是直接下载，比如 aria2c "http://host/file.zip" 这样，当它完成后就退出了，就像wget（估计你们也不知道吧）那样。
另一种就是rpc server模式，特点就是，它启动之后什么都不干，然后等着从rpc接口添加任务，下载完也不退出，而是一直等着。对，就像迅雷干的那样，当然，它不会上传你硬盘上的数据。
因为第一种方式要每次都敲命令，除非像我是原生nix，没有命令行就没法用电脑，估计也没什么用，于是常用的就是第二种。一般启动命令是 aria2c --enable-rpc --rpc-listen-all=true --rpc-allow-origin-all -c -D 。但是，其实*这个命令是不好的！不要使用这种启动方式。
首先，用命令方式导致配置不方便修改保存，-D导致无法看到出错信息。
推荐启动方式是使用配置文件 $HOME/.aria2/aria2.conf 。嗯，我知道路由上这个地址是无法修改或者重启后会丢失的，那么你可以放到别的地方，然后 aria2c --conf-path=<PATH> 注意 <PATH> 填完整路径，因为鬼知道这个程序是从那个路径启动的。-D (用于后台执行, 这样ssh断开连接后程序不会退出） 只有在确认OK之后在启动脚本中使用。

#### 启动脚本

``` shell
aria2c --conf-path=/etc/aria2.conf -D
```

#### aria2.conf

``` java
## `#`开头为注释内容, 选项都有相应的注释说明, 根据需要修改 ##
## 被注释的选项填写的是默认值, 建议在需要修改时再取消注释  ##
## 如果出现`Initializing EpollEventPoll failed.`之类的
## 错误提示, 可以取消event-poll选项的注释                  ##

## 文件保存相关 ##

# 文件的保存路径(可使用绝对路径或相对路径), 默认: 当前启动位置
dir=Aria2Data
# 启用磁盘缓存, 0为禁用缓存, 需1.16以上版本, 默认:16M
disk-cache=32M
# 文件预分配方式, 能有效降低磁盘碎片, 默认:prealloc
# 预分配所需时间: none < falloc ? trunc < prealloc
# falloc和trunc则需要文件系统和内核支持, NTFS建议使用falloc, EXT3/4建议trunc
file-allocation=falloc
# 断点续传
continue=true

## 下载连接相关 ##

# 最大同时下载任务数, 运行时可修改, 默认:5
max-concurrent-downloads=1
# 同一服务器连接数, 添加时可指定, 默认:1
max-connection-per-server=5
# 最小文件分片大小, 添加时可指定, 取值范围1M -1024M, 默认:20M
# 假定size=10M, 文件为20MiB 则使用两个来源下载; 文件为15MiB 则使用一个来源下载
min-split-size=10M
# 单个任务最大线程数, 添加时可指定, 默认:5
split=5
# 整体下载速度限制, 运行时可修改, 默认:0
#max-overall-download-limit=0
# 单个任务下载速度限制, 默认:0
#max-download-limit=0
# 整体上传速度限制, 运行时可修改, 默认:0
#max-overall-upload-limit=0
# 单个任务上传速度限制, 默认:0
#max-upload-limit=0
# 禁用IPv6, 默认:false
disable-ipv6=true

## 进度保存相关 ##

# 从会话文件中读取下载任务
input-file=aria2.session
# 在Aria2退出时保存`错误/未完成`的下载任务到会话文件
save-session=aria2.session
# 定时保存会话, 0为退出时才保存, 需1.16.1以上版本, 默认:0
save-session-interval=60

## RPC相关设置 ##

# 启用RPC, 默认:false
enable-rpc=true
# 允许所有来源, 默认:false
rpc-allow-origin-all=true
# 允许非外部访问, 默认:false
rpc-listen-all=true
# 事件轮询方式, 取值:[epoll, kqueue, port, poll, select], 不同系统默认值不同
#event-poll=select
# RPC监听端口, 端口被占用时可以修改, 默认:6800
#rpc-listen-port=6800

## BT/PT下载相关 ##

# 当下载的是一个种子(以.torrent结尾)时, 自动开始BT任务, 默认:true
#follow-torrent=true
# BT监听端口, 当端口被屏蔽时使用, 默认:6881-6999
listen-port=51413
# 单个种子最大连接数, 默认:55
#bt-max-peers=55
# 打开DHT功能, PT需要禁用, 默认:true
enable-dht=false
# 打开IPv6 DHT功能, PT需要禁用
#enable-dht6=false
# DHT网络监听端口, 默认:6881-6999
#dht-listen-port=6881-6999
# 本地节点查找, PT需要禁用, 默认:false
#bt-enable-lpd=false
# 种子交换, PT需要禁用, 默认:true
enable-peer-exchange=false
# 每个种子限速, 对少种的PT很有用, 默认:50K
#bt-request-peer-speed-limit=50K
# 客户端伪装, PT需要
peer-id-prefix=-TR2770-
user-agent=Transmission/2.77
# 当种子的分享率达到这个数时, 自动停止做种, 0为一直做种, 默认:1.0
seed-ratio=0
# 强制保存会话, 话即使任务已经完成, 默认:false
# 较新的版本开启后会在任务完成后依然保留.aria2文件
#force-save=false
# BT校验相关, 默认:true
#bt-hash-check-seed=true
# 继续之前的BT任务时, 无需再次校验, 默认:false
bt-seed-unverified=true
# 保存磁力链接元数据为种子文件(.torrent文件), 默认:false
bt-save-metadata=true
```
