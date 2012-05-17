---
layout: post
title: "我爱命令行之sed"
date: 2012-05-17 19:22
comments: true
categories: 
---

## 语法

常用基本格式
```
 sed -i -e '指令1' -e '指令2' inputfile
 sed -f scriptfile
```

* -i: 直接修改文件，而不是输出到标准输出，一般等调试无误后，最后使用该参数
* -e: 后跟指令，可以多次使用
* -f: 从文件中读取指令

<!--more-->

## 操作指令

### 删除

一般格式
```
 开始地址,结束地址d
 /正则表达式/d
```

第一种删除从开始到结束的行，第二行删除有匹配到字符串的行

### 替换

一般格式
```
 开始地址,结束地址s/被替换的文本（正则表达式）/用来替换的文本/标志
```

地址和标志部分可以省略。s(substitute)后的第一个字符被用来作为分割符，所以并非一定要用斜杠，
还可以使用比如冒号（有时候会带来方便，比如匹配网址时，就可以避免和网址中的斜杠冲突）。

用到的标志:

* g: 全局替换，而不只是每行的第一个匹配
* n: 替换行中的第n个匹配


## 应用实例


### 删除指定行

```
 sed -e '/^#acl/d' inputfile
```

删除#acl开头的行。在MoinMoin升级中，由于用户名改变的原因，页面都变成了只读，只好删除之。

### DOS文件和Linux文件的转换

众所周知，Windows上文本文件以CR（回车）和 LF（换行）结束一行，而Linux上只有LF。

```
 sed -i -e 's/\r$//' inputfile #dos to linux
 sed -e 's/$/\r/' inputfile #linux to dos
```

### 正则表达式分组（对匹配字符串的一部分进行操作）

```
 sed -e 's:\(.\)\(}}}\):\1\n\2:' inputfile
 sed -e 's:\({{{\)\([^\n]\):\1\n\2:' inputfile
```

这是我升级MoinMoin时用过的，旧版{{{和}}}可能与内容在同一行，新版必须单独一行。用圆括号分组（注意带反斜杠），
按出现顺序编号引用（同样注意使用反斜杠）。

### sed脚本

```
 #!/bin/sed -f
```

## 参考资料

[通用线程 -- sed 实例](http://www.ibm.com/developerworks/cn/linux/shell/sed/sed-1/index.html) : IBM网站上Daniel Robbins写的教程，正则表达式分组我就是从该教程第三部分学到的。