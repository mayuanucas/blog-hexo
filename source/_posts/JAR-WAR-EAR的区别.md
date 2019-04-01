---
title: JAR_WAR_EAR的区别
date: 2017-11-27 20:24:37
tags: 
- java
categories: 
- java基础知识
---

本文记录JAR、WAR、EAR的区别，以便更好地理解。
<!-- more -->

### JAR文件
扩展名为 .jar
(Java Application Archive）包含Java类的普通库、资源（resources）、辅助文件（auxiliary files）等。

### WAR文件
扩展名为 .war
(Web Application Archive) 包含全部Web应用程序。在这种情形下，一个Web应用程序被定义为单独的一组文件、类和资源，
用户可以对jar文件进行封装，并把它作为小型服务程序（servlet）来访问。

#### WAR文件的使用
如果想生成war文件：可以使用如下命令：jar -cvf web1.war *
如果想查看web1.war中都有哪些文件，可以使用命令：jar -tf web1.war
如果想直接解压web1.war文件，可以使用命令：jar -xvf web1.war
另外，也可使用winrar软件选择zip压缩方式，并将压缩文件后缀名改为war即可压缩生成war文件；同样使用winrar软件可以强行打开war文件，或者强行解压war文件
使用jar命令与winrar软件的区别在于前者在压缩文件的同时会生成MetaINF文件夹，内包含MANIFEST.MF文件。

### EAR文件
扩展名为 .ear
(Enterprise Application Archive）包含全部企业应用程序。在这种情形下，一个企业应用程序被定义为多个jar文件、资源、类和Web应用程序的集合。EAR文件包括整个项目，内含多个ejb module（jar文件）和web module（war文件）
每一种文件（.jar, .war, .ear）只能由应用服务器（application servers）、小型服务程序容器（servlet containers）、EJB容器（EJB containers）等进行处理。
