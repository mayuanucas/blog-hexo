---
title: Nginx编译安装
date: 2017-12-12 20:15:05
tags: 
- nginx
- linux
categories: 
- linux
---

本文记录在 Ubuntu 16.04 上编译安装 nginx
<!-- more -->
## 编译安装Nginx
### 环境说明

- **Ubuntu 16.04**
- **Nginx 1.10.3**

### 安装编译依赖的库
``` bash
sudo apt install gcc libpcre3 libpcre3-dev openssl libssl-dev libperl-dev zlib1g-dev perl libgd2-xpm-dev
```

### 进入解压后的nginx的文件夹
``` bash
./configure --prefix=/usr/local/nginx --pid-path=/usr/local/nginx/logs/nginx.pid --error-log-path=/usr/local/nginx/logs/error.log --http-log-path=/usr/local/nginx/logs/access.log --with-http_ssl_module --with-http_image_filter_module --with-ipv6 --with-http_realip_module --with-http_addition_module --with-http_dav_module --with-http_flv_module --with-http_mp4_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_perl_module --with-mail --with-mail_ssl_module
```

### configure成功后执行安装
``` bash
# 执行make和make install 安装
make && sudo make install
```
