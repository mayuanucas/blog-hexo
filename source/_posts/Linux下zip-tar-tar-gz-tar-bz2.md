---
title: Linux下zip tar tar.gz tar.bz2
date: 2017-09-02 09:35:23
tags: [linux, zip, tar, tar.gz, tar.bz2, shell]
categories: linux

---
压缩、解压缩是日常工作中常用的两个操作，对于 Windows 平台而言，最常用的格式是 zip 和 rar，国内大多数是用 rar，国外大多数是用 zip。
而对于类 Unix 平台而言，常用的格式是 tar 和 tar.gz，zip 比较少一些，rar 则几乎没有。
<!-- more -->

##### ZIP 格式
zip 格式是开放且免费的，所以广泛使用在 Windows、Linux、MacOS 平台，要说 zip 有什么缺点的话，就是它的压缩率并不是很高，不如 rar及 tar.gz 等格式。

将文件或文件夹压缩为一个 zip 文档的命令如下：
``` bash
zip -r archive_name.zip file_to_compress
zip -r archive_name.zip directory_to_compress/
```

解压 zip 文档的命令如下：
``` bash
unzip archive_name.zip
```

##### TAR 格式
严格的说，tar 只是一种打包格式，并不对文件进行压缩，主要是为了便于文件的管理，所以打包后的文档大小一般远远大于 zip 和 tar.gz，但这种格式也有很明显的优点，例如打包速度非常快，打包时 CPU 占用率也很低，因为不需要压缩嘛。

将文件或文件夹打包为 tar 文档的命令如下：
``` bash
tar -cvf archive_name.tar file_to_compress
tar -cvf archive_name.tar directory_to_compress
```

解包一个 tar 文档的命令如下：
``` bash
tar -xvf archive_name.tar
```

##### TAR.GZ
tar.gz 可以说是对于 tar 的一个补充，它会对文件进行压缩，且压缩率略优于 zip，而对于 CPU 的占用率却不怎么高。Linux 平台下的大多数开源软件或源代码都是采用这种格式。
将文件或文件夹打包压缩为 tar.gz 文档的命令如下：
``` bash
tar -zcvf archive_name.tar.gz file_to_compress
tar -zcvf archive_name.tar.gz directory_to_compress
```

解压一个 tar.gz 文档的命令如下：
``` bash
tar -zxvf archive_name.tar.gz
```

##### TAR.BZ2
相比以上几种格式，tar.gz2 拥有最高的压缩率，但是压缩的时候所需要的时间也最长，CPU 占用率也最高。将文件或文件夹压缩为 tar.bz2 的命令如下：
``` bash
tar -jcvf archive_name.tar.bz2 file_to_compress
tar -jcvf archive_name.tar.bz2 directory_to_compress
```

解压一个 tar.bz2 文件的命令是：
``` bash
tar -jxvf archive_name.tar.bz2
```

##### tar.Z
解压：
``` bash
tar -Zxvf FileName.tar.Z
```
压缩：
``` bash
tar -Zcvf FileName.tar.Z DirName
```

##### 解压 7zip 文件
在Linux平台下解压 *.7z 文件(7zip 文件)，可以使用命令 p7zip。
首先检查你的系统中是否已经安装有 p7zip 这个工具:
``` bash
which p7zip
```
执行上述命令后，如果输出了 p7zip 的绝对路径，则表明你的系统中已经安装了此工具,你可以直接跳过 安装 p7zip 这一部分内容；如果上述命令没有输出任何信息，则表明你的系统中没有安装 p7zip，那么你首先需要安装该工具。

* 安装 p7zip
在Ubuntu中，你可以执行下面的命令来安装 p7zip:
``` bash
sudo apt-get install p7zip
```
* 对于 Red Hat 系列的Linux系统，则使用下面的命令来安装:
``` bash
yum install p7zip
```

* 压缩文件
压缩文件很简单，直接将想要压缩的文件作为 p7zip 的参数即可。如下:
``` bash
p7zip example.txt
```
    执行上述命令后，会产生一个名为 example.txt.7z 的压缩文件.

* 解压文件
解压文件和压缩文件的方法很类似，只是多了一个选项 -d, 假如现在想要解压上面的 example.txt.7z 文件, 可以使用下面的命令:
``` bash
p7zip -d example.txt.7z
```

* 关于 p7zip 的更多信息，可以通过 man p7zip 来查看.
