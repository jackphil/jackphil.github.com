---
layout: post
title: "KDE的语义学桌面: 看上去很美"
date: 2012-05-20 15:52
comments: true
categories: 
---

目前，貌似只有KDE4的家伙们热衷于语义学桌面，并把它强推给了用户。在我看来，这只不过是旧式图书馆技术。这些家伙都是图书馆学毕业的吗？

<!--more-->

## 什么是语义学

当然，这里谈的是计算机世界的语义学，不是老板对我说要好好工作，他的话语到底有什么含义这样的职场语义学。

语义学大致说来是基于数学基础中的一阶逻辑而来的一套知识表示系统。1998年，互联网发明者蒂姆·伯纳斯·李提出了“语义网”的概念，W3C一直在制定和维护相关的标准。2011年6月，谷歌、雅虎、微软联手推出了schema.org网站，进一步推动语义网的发展

我的理解语义网与以往的图书馆技术没什么两样，就是一套元数据规范。

* 在数据交换时可能有用，但一般来说只要公开数据接口数据交换就不会有什么问题，不一定非要用什么语义网
* 至于知识表示，图书馆也有一套分类法，试图结构化所有人类知识，个人感觉，这些都是互联网史前的观念

## KDE语义学桌面的几个概念

Soprano是一套用来存取语义数据（我理解为元数据）库的QT接口

Virtuoso是一个语义数据库，其它类似的还有Redland, Sesame2

Nepomuk是KDE版的Soprano。当然有所扩展和增强，如自带了一个谓词表（弄个同义和近义词表不就得了？）

Strigi是Nepomuk的文件索引器，能从文件中自动抽取元数据存储到Virtuoso

Akonadi是KDE的PIM(Personal Information Management: 个人信息管理)数据统一存取框架，它使用MySQL或SQLITE在本地缓存你PIM信息，如gmail邮件、网络通讯录等，同时也会抽取元数据存储到Virtuoso

## 关掉它！

其实写这篇Blog的初衷就是我想扔掉语义学桌面这套东西。扔掉之前，先做一番小小的调查研究，以免娶了小姐丢了丫鬟，辜负了美意。结果还是觉得这是一套强加给用户的范式，包办婚姻害死人啊。

### 关闭Nepomuk
Nepomuk是KDE4的核心组件，仍不掉，唯一的办法只能关掉它！

打开 ~/.kde/share/config/nepomukserverrc，将“Start Nepomuk”设置为false（你也可以在系统设置-桌面搜索中，不选启用Nepomuk语义学桌面来关闭）
```
[Basic Settings]
Start Nepomuk=false
```

打开~/.kde/share/config/kdedrc，将nepomuksearchmodule的“autoload”设置为false:
```
[Module-nepomuksearchmodule]
autoload=false
```
Strigi依赖Nepomuk，关闭Nepomuk后，Strigi也就自动关闭了

### 关闭Akonadi
Akonadi作为PIM数据统一存取框架，与Nepomuk相对独立，需要单独关闭

打开~/.config/akonadi/akonadiserverrc，设置“StartServer”为false
```
[QMYSQL]
StartServer=false
```
事实上根据[KDE Userbase](http://userbase.kde.org/Akonadi#Disabling_the_Akonadi_subsystem)的说法，这个设置也只是不自动启动Akonadi server而已，如果某个应用需要akonadi它还会按需启动，相当于Windows服务管理中的"手动"，坏消息是没有提供"禁止"功能

我的测试环境下，Akonadi服务还是会自动启动(没找到是哪个应用触发了它)，所以选用了MySQL后端，但不配置相应的MySQL数据库，甚至大多数情况下，KDE启动的时候MySQL服务都没启动，Akonadi启动失败，自动停止。使用SQLITE后端的话，由于不需要数据库特殊配置，就会成功启动

你可以使用命令行控制Akonadi服务
```
ubuntuku@satellite:~$ akonadictl --help
Akonadi server manipulation tool
Usage: akonadictl [command]
 
Commands:
  start      : Starts the Akonadi server with all its processes
  stop       : Stops the Akonadi server and all its processes cleanly
  restart    : Restart Akonadi server with all its processes
  status     : Shows a status overview of the Akonadi server
```

### 一些清理
清理并不必须，有些甚至还会再次自动生成，只是纯粹出于个人爱好

```
rm -rf .kde4/share/apps/nepomuk/
rm -f .config/akonadi/agent* # 删除除akonadiserverrc外的所有文件
rm -rf .local/share/akonadi/ # akonadi的出错信息在这儿
``` 

## 参考资源

[Akonadi, Nepomuk and Strigi explained](http://thomasmcguire.wordpress.com/2009/10/03/akonadi-nepomuk-and-strigi-explained): 关于Akonadi，Nepomuk和Strigi很好的名词解释

[How to Disable Nepomuk & Akonadi](http://ubuntuku.org/16/how-to-disable-nepomuk-akonadi/): 我的关闭语义学桌面的内容主要摘自这里

[语义网的春天](http://select.yeeyan.org/view/163202/204263): 介绍了语义网的七层结构，schema.org

[语义网](http://zh.wikipedia.org/zh/%E8%AF%AD%E4%B9%89%E7%BD%91): 语义网的维基页

[知识表示方法](http://wlzy.aynu.edu.cn/jsj/wlkc/rgznyl2/word/text/chapter04/sec4/part3/text.htm): 语义网比较专业的内容，有一些一阶逻辑表达式

[Nepomuk和Akonadi](https://wiki.archlinux.org/index.php/KDE#Soprano): Archlinux Wiki的KDE页面，介绍了Nepomuk和Akonadi的一些配置

## 声明

文中提到的概念和评论都是自己对网络上找到的相关文章的归纳和个人理解，欢迎指正和讨论