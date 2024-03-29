---
title: linux sudo免密码
date: 2017-12-18 10:41:29
tags: 
- linux
categories: 
- linux
---
本文记录下如何在linux 上，运行sudo 命令时免密码输入。(假设该电脑只有你自己使用)
<!-- more -->
### 编辑 /etc/sudoers
``` shell
sudo vim /etc/sudoers
```

### 添加下面这行 (假设用户名为 ubuntu)
``` shell
ubuntu ALL=(ALL:ALL) NOPASSWD : ALL
```

### 添加位置说明
当把添加的内容添加到第20行后一行时，会不起作用。有效的做法是：不要把需要添加的内容紧接着添加到第20行后面，而是添加到第27行之后的位置。

### 修改后的 /etc/sudoers 文件内容如下
``` shell
#
# This file MUST be edited with the 'visudo' command as root.
#
# Please consider adding local content in /etc/sudoers.d/ instead of
# directly modifying this file.
#
# See the man page for details on how to write a sudoers file.
#
Defaults    env_reset
Defaults    mail_badpass
Defaults    secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap    /bin"

# Host alias specification

# User alias specification

# Cmnd alias specification

# User privilege specification
root    ALL=(ALL:ALL) ALL
# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "#include" directives:

ubuntu  ALL=(ALL:ALL) NOPASSWD : ALL # <- 用户自己添加的内容

#includedir /etc/sudoers.d
```

### 指定可以免密码的命令
例如：
``` shell
sudo apt upate
```

添加下面的内容到 /etc/sudoers
``` shell
ubuntu ALL=(ALL:ALL) NOPASSWD : /usr/bin/apt
```
这样再次运行命令时，就不需要输入密码了。
