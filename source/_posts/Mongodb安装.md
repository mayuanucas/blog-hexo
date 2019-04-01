---
title: Mongodb安装
date: 2017-12-12 20:14:49
tags: 
- mongoDB
- linux
categories: 
- NOSQL
---
本文记录在 Ubuntu 16.04系统上进行 MongoDB 的安装、启动、停止、卸载等操作。
<!-- more -->
## MongoDB 3.4 安装

### 添加Key
``` bash
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
```

### 添加地址到list
``` bash
# Ubuntu 16.04

echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
```

### 更新并安装mongodb
``` bash
sudo apt update && sudo apt -y install mongodb-org
```

## MongoDB 3.4 常用操作

### 启动
``` bash
sudo service mongod start
```

### 验证MongoDb是否成功启动
**Verify that the mongod process has started successfully by checking the contents of the log file at /var/log/mongodb/mongod.log for a line reading**


### 启动
``` bash
sudo service mongod stop
```

### 重新启动MongoDB
``` bash
sudo service mongod restart
```

## 卸载 MongoDB Community Edition
``` bash
sudo service mongod stop
sudo apt-get purge mongodb-org*

sudo rm -r /var/log/mongodb
sudo rm -r /var/lib/mongodb
```
