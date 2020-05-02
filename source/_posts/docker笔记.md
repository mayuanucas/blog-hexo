---
title: docker笔记
date: 2020-05-02 19:16:08
tags:
- docker
categories:
- 笔记系列
---

Docker 使用Google公司推出的Go语言进行开发实现，基于Linux内核的cgroup，namespace，以及AUFS类的Union FS等技术，对进程进行封装隔离，属于操作系统层面的虚拟化技术。由于隔离的进程独立于宿主和其他的隔离的进程，因此也称其为容器。

<!-- more -->

# 环境难题

软件开发最大的麻烦事之一，就是环境配置。用户计算机的环境都不相同，你怎么知道自家的软件，能在那些机器跑起来？

用户必须保证两件事：操作系统的设置，各种库和组件的安装。只有它们都正确，软件才能运行。举例来说，安装一个 Python 应用，计算机必须有 Python 引擎，还必须有各种依赖，可能还要配置环境变量。

如果某些老旧的模块与当前环境不兼容，那就麻烦了。开发者常常会说："它在我的机器可以跑了"（It works on my machine），言下之意就是，其他机器很可能跑不了。

环境配置如此麻烦，换一台机器，就要重来一次，旷日费时。很多人想到，能不能从根本上解决问题，软件可以带环境安装？也就是说，安装的时候，把原始环境一模一样地复制过来。

# Docker是什么？

Docker 属于 Linux 容器的一种封装，提供简单易用的容器使用接口。它是目前最流行的 Linux 容器解决方案。

Docker 将应用程序与该程序的依赖，打包在一个文件里面。运行这个文件，就会生成一个虚拟容器。程序在这个虚拟容器里运行，就好像在真实的物理机上运行一样。有了 Docker，就不用担心环境问题。

总体来说，Docker 的接口相当简单，用户可以方便地创建和使用容器，把自己的应用放入容器。容器还可以进行版本管理、复制、分享、修改，就像管理普通的代码一样。

# Docker能做什么？

在项目开发测试过程中，我们常常面临的问题：
1. 测试环境与开发环境不一致，工单提交后，开发人员经常对我们说：“我这边是正常的！”
2. 测试环境的搭建可能因为JDK的版本不同而失败等等问题。

Docker可以帮我们解决这一系列问题！	

# Docker的用途

Docker 的主要用途，目前有三大类。

**（1）提供一次性的环境。** 比如，本地测试他人的软件、持续集成的时候提供单元测试和构建的环境。

**（2）提供弹性的云服务。** 因为 Docker 容器可以随开随关，很适合动态扩容和缩容。

**（3）组建微服务架构。** 通过多个容器，一台机器可以跑多个服务，因此在本机就可以模拟出微服务架构。

# Docker命令

```shell
docker search 镜像名称 //搜索镜像
docker pull 镜像名称:版本号 //拉取对应版本的镜像
docker pull 镜像名称 //默认拉取最新的镜像
docker image ls -a //查看本地已下载的镜像
docker images //查看本地已下载的镜像
docker ps //查看正在运行的容器
docker ps -a //查看所有的容器（包括run、stop、exited状态的）
docker container ls //查看正在运行的容器
docker rm 容器ID //只能删除没有在运行的容器
docker rm -f 容器ID //可以删除正在运行的容器
docker run -d -p 本地主机端口号:容器服务端口号 --name 容器名字 [-e 配置信息修改] 镜像名字
docker start 容器ID //启动容器
docker stop 容器ID //终止容器
docker rmi 镜像名称orID //删除镜像
```

# 例子

## MySQL8.0

### Docker仓库搜索mysql

```shell
docker search mysql
```

### Docker仓库拉取mysql8.0

```shell
# 默认拉取最新版本
docker pull mysql
# 或者指定拉取版本
docker pull mysql:8.0
```

### 查看本地仓库镜像

```shell
docker image ls -a
# 或者
docker images
```

### 运行MySQL8.0容器

```shell
docker run -d -p 3308:3306 --name mysql8 -e MYSQL_ROOT_PASSWORD mysql
备注：
-p 将本地主机的端口映射到docker容器端口（因为本机的3306端口已被其它版本占用，所以使用3308）
--name 容器名称命名
-e 配置信息，配置root密码
-d 后台运行
```

### 查看容器运行情况

```shell
docker ps -a
```

### Docker登陆mysql

```shell
docker exec -it mysql8 bash
# 进入容器后执行下面的命令，并输入密码
mysql -u root -p
```


