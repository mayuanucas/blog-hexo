---
title: linux的find命令用法
date: 2017-08-10 08:46:01
tags:
- linux
- shell
categories: 
- linux

---
#### find是linux下很强大的搜索工具,但速度慢且很费硬盘。但不管怎么说，此命令的使用频率依然很高。
<!-- more -->

#### 使用选项：
#### find [路径] <表达式> [操作]

### 1、name选项，按名称查找
 查找当前目录下的manage.py 文件：find . -name 'manage.py'

### 2、atime/ctime/mtime选项，根据时间(24小时为单位)查找 注：a表示access，c表示create，m表示modify
  查找24小时之内创建的文件： find . -ctime -1
  查找24小时之前创建的文件： find . -ctime 1
  注：atime和mtime用法一致

### 3、amin/cmin/mmin选项，根据时间查找
  查找10分钟之内创建的文件： find . -cmin -10
  查找10分钟之前创建的文件： find . -cmin 10
  注：amin和mmin用法一致

### 4、anewer/cnewer/mnewer,查找比某一文件新的文件
  查找在hello.py之后访问过的文件：find . -anewer hello.py

### 5、user
  查找属于某一用户的文件：find . -user the5fire

### 6、type
  查找所有文件：find . -type f
  查找所有目录包含demo的目录：find . -type d -name '*demo*'

### 7、exec,据说是很强大的参数
  查找'setup.py'文件，然后打开: find . -name 'setup.py' -exec vim {} \;
  另外一个最常用的,强制删除项目下面的所有.svn文件目录，find . -name '.svn' -exec rm -rf {} \;
### 8、empty
  显示所有的空白文件，并显示详细：find . -empty -ls #加ls完全画蛇添足，只是为了说明这个参数。

### 9、size
  显示大小为10k的文件：find . -size 10k
  显示所有大于10k的文件：find . -size +10k
  显示所有小于10k的文件：find .-size -10k

### 10、or、and、not， 或、与、非查询
  查找大于10k或者名称含有demo的文件：find . -size +10k -o -name '*demo*'
  查找大于10k且小于100k的文件：find . -size +10k -a -size -100k
  查找大于10k并且名称不含有demo的文件：find . -size +10k ! -name '*demo*'

### 11、perm，根据文件权限查找
  注：如查找权限为600的文件：find . -perm 600,如果权限前面加“-”号，表示满足一位匹配即可，
  如：find . -perm 007会匹配权限为007、077、777的文件

### 12、regex，用正则表达式查找
  如: find . -regex '.*/[0-9]\w.*'(匹配以数字开头的文件)

### 13、-maxdepth，限制目录深度查找
  查找一级目录下的所有py文件：find . -name '*.py' -maxdepth 1
