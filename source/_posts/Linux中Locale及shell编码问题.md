---
title: Linux中Locale及shell编码问题
date: 2017-08-18 15:49:19
tags: 
- linux
- 编码
categories: 
- linux

---
##### Linux下关于locale的设定
>locale是国际化与本土化过程中的一个非常重要的概念，对于中文用户来说，通常会涉及到的国际化或者本土化，大致包含三个方面：看中文，写中文，与window中文系统的兼容和通信。从实际经验上看来，locale的设定与看中文关系不大，但是与写中文，及window分区的挂载方式有很密切的关系。本人认为就像一个纯英文的Windows能够浏览中文，日文或者意大利文网页一样，你不需要设定locale就可以看中文。那么，为什么要设定locale呢？什么时候会用到locale呢？

<!-- more -->

##### 什么是locale
1. locale这个单词中文翻译成地区或者地域，其实这个单词包含的意义要宽泛很多。Locale是根据计算机用户所使用的语言，所在国家或者地区，以及当地的文化传统所定义的一个软件运行时的语言环境。
2. 这个用户环境可以按照所涉及到的文化传统的各个方面分成几个大类，通常包括用户所使用的语言符号及其分类(LC_CTYPE)，数字 (LC_NUMERIC)，比较和排序习惯(LC_COLLATE)，时间显示格式(LC_TIME)，货币单位(LC_MONETARY)，信息主要是提示信息,错误信息, 状态信息, 标题, 标签, 按钮和菜单等(LC_MESSAGES)，姓名书写方式(LC_NAME)，地址书写方式(LC_ADDRESS)，电话号码书写方式 (LC_TELEPHONE)，度量衡表达方式(LC_MEASUREMENT)，默认纸张尺寸大小(LC_PAPER)和locale对自身包含信息的概述(LC_IDENTIFICATION)。所以说，locale就是某一个地域内的人们的语言习惯和文化传统和生活习惯。
3. 一个地区的locale就是根据这几大类的习惯定义的，这些locale定义文件放在/usr/share/i18n/locales目录下面，例如en_US, zh_CN and de_DE@euro都是locale的定义文件，这些文件都是用文本格式书写的，你可以用编辑器打开，看看里边的内容，当然出了有限的注释以外，大部分东西可能你都看不懂，因为是用的Unicode的字符索引方式。

##### 什么是字符集
字符集就是字符，尤其是非英语字符在系统内的编码方式，也就是通常所说的内码，所有的字符集都放在 /usr/share/i18n/charmaps，所有的字符集也都是用Unicode编号索引的。Unicode用统一的编号来索引目前已知的全部的符号。而字符集则是这些符号的编码方式，或者说是在网络传输，计算机内部通信的时候，对于不同字符的表达方式，Unicode是一个静态的概念，字符集是一个动态的概念，是每一个字符传递或传输的具体形式。就像Unicode编号U59D0是代表姐姐的“姐”字，但是具体的这个字是用两个字节表示，三个字节，还是四个字节表示，是字符集的问题。例如：UTF-8字符集就是目前流行的对字符的编码方式，UTF-8用一个字节表示常用的拉丁字母，用两个字节表示常用的符号，包括常用的中文字符，用三个表示不常用的字符，用四个字节表示其他的古灵精怪的字符。而GB2312字符集就是用两个字节表示所有的字符。需要提到一点的是Unicode除了用编号索引全部字符以外，本身是用四个字节存储全部字符，这一点在谈到挂载windows分区的时候是非常重要的一个概念。所以说你也可以把Unicode看作是一种字符集（我不知道它和UTF-32的关系，反正UTF-32就是用四个字节表示所有的字符的），但是这样表述符号是非常浪费资源的，因为在计算机世界绝大部分时候用到的是一个字节就可以搞定的26个字母而已。所以才会有UTF-8，UTF-16等等，要不然大同世界多好，省了这许多麻烦。

##### 生成 locale
设置 locale 前，需要先准备需要的 locale。要列出所有启用的locale，使用：
``` shell
$ locale -a
```
可生成的 Locale 保存在 /etc/locale.gen 中,用以下的格式来定义：[language][_TERRITORY][.CODESET][@modifier].要开启某个Locale,反注释对应的行即可.
尽管在你的电脑上你很可能只使用一种语言,但同时开启其它的 Locale 有时会有帮助、甚至是必要的.比如你正运行着一个多用户的系统，而有的用户并不懂en_US，那么你的系统需要支持他们需要的 locale.
例如对于使用美式英语的用户,反注释 en_US.UTF-8 UTF-8 一行.
``` shell
/etc/locale.gen
...
#en_SG ISO-8859-1
en_US.UTF-8 UTF-8
#en_US ISO-8859-1
...
```
编辑完成以后,通过下面的命令生成 Locale :
``` shell
# locale-gen
```

##### Linux下LC_ALL=C的含义
在很多的shell脚本中，我们经常会看见某一句命令的前面有一句“LC_ALL=C”
SAR_CMD="LC_ALL=C sar -u -b 1 5 | grep -i average "
 这到底是什么意思？
 **LC_ALL=C 是为了去除所有本地化的设置，让命令能正确执行。**

 在Linux中通过locale来设置程序运行的不同语言环境，locale由ANSI C提供支持。locale的命名规则为<语言>_<地区>.<字符集编码>，如zh_CN.UTF-8，zh代表中文，CN代表大陆地区，UTF-8表示字符集。在locale环境中，有一组变量，代表国际化环境中的不同设置：
 1.    LC_COLLATE
 定义该环境的排序和比较规则

 2.    LC_CTYPE
 用于字符分类和字符串处理，控制所有字符的处理方式，包括字符编码，字符是单字节还是多字节，如何打印等。是最重要的一个环境变量。

 3.    LC_MONETARY
 货币格式

 4.    LC_NUMERIC
 非货币的数字显示格式

 5.    LC_TIME
 时间和日期格式

 6.    LC_MESSAGES
 提示信息的语言。另外还有一个LANGUAGE参数，它与LC_MESSAGES相似，但如果该参数一旦设置，则LC_MESSAGES参数就会失效。LANGUAGE参数可同时设置多种语言信息，如LANGUANE="zh_CN.GB18030:zh_CN.GB2312:zh_CN"。

 7.    LANG
 LC_\*的默认值，是最低级别的设置，如果LC_\*没有设置，则使用该值。类似于 LC_ALL。


 8.    LC_ALL
 它是一个宏，如果该值设置了，则该值会覆盖所有LC_*的设置值。注意，LANG的值不受该宏影响。


 C"是系统默认的locale，"POSIX"是"C"的别名。所以当我们新安装完一个系统时，默认的locale就是C或POSIX。

##### 填坑
{% asset_img encoding01.png locale问题记录%}
