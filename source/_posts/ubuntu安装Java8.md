---
title: ubuntu安装Java8
date: 2018-04-14 10:24:55
tags: 
- linux, ubuntu, J
- java
categories: 
- linux
---

本文记录如何在 Ubuntu16.04 系统安装 Oracle Java8.
<!-- more -->

### 第一步：添加PPA并安装Oracle Java8
在终端里依次运行下面的命令：
```
sudo add-apt-repository ppa:webupd8team/java

sudo apt update

sudo apt install oracle-java8-installer
```

第三条命令安装一个installer，这个installer会从Oracle的官网下载Java二进制文件，然后安装到Ubuntu，Linux Mint，Elementary OS系统．在安装过程中，你需要同意Oracle的使用条款．

### 第二步：检查版本号
```
java -version
javac -version
```

### 第三步：设置Java环境变量
运行下面的命令设置Java环境变量
```
sudo apt-get install oracle-java8-set-default
```
命令完成后，在/etc/profile.d/目录下会有两个新的文件: jdk.csh和jdk.sh．这两个文件是shell脚本，里面是设置Java环境变量的命令，共有５个环境变量

```
cat /etc/profile.d/jdk.sh
```
输出：
```
export J2SDKDIR=/usr/lib/jvm/java-8-oracle
export J2REDIR=/usr/lib/jvm/java-8-oracle/jre
export PATH=$PATH:/usr/lib/jvm/java-8-oracle/bin:/usr/lib/jvm/java-8-oracle/db/bin:/usr/lib/jvm/java-8-oracle/jre/bin
export JAVA_HOME=/usr/lib/jvm/java-8-oracle
export DERBY_HOME=/usr/lib/jvm/java-8-oracle/db
```

我们需要使用source来运行/etc/profile这个shell脚本，以使这些环境变量的设置命令生效．
```
source /etc/profile
```

现在我们可以用echo来查看这五个Java环境变量了，比如查看JAVA_HOME.
```
echo $JAVA_HOME
```
