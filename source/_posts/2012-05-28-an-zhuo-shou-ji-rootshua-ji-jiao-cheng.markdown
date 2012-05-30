---
layout: post
title: "安卓手机root刷机教程: 我的HTC Saga root记录"
date: 2012-05-28 22:35
comments: true
categories: 
---

整理旧文一篇，部分内容可能已经过时

<!--more-->

## HBOOT

HBOOT相当于手机的bios

同时按住音量下键和电源键开机，就进入了HBOOT界面，如果有S-ON字样，则需要先解锁，才可以自由的刷机。如果已经是S-OFF，则已经解锁

解锁，简单说就是刷入一个定制的HBOOT，分软解和硬解

## 软解

软解方法由伟大的[AlpharevX](http://alpharev.nl/x/beta)开发

 * 去 http://revolutionary.io/ 下载相应的版本，并填写系统平台(Windows还是Linux)、机型和SN号(可以打开后盖取下电池后看到)等信息，得到一个beta key，记下这个key
 * 用USB连接电脑，确认系统设置-应用程序-开发-USB调试已经打开
 * 运行下载的revolutionary程序，按提示输入beta key，等待，机器会自动重启进入HBOOT界面
 * revolutionary会询问是否安装recovery，如果选择不安装，也可以在以后用fastboot或RomManager安装

另：xda上高手开发了一个新的软解工具，[HTC Super Tool](http://forum.xda-developers.com/showthread.php?t=1343114)


## 硬解

硬解需要白卡(smartcard，HTC用于工程调试之用，因其白色而得名)或[xtc clip](http://www.xtcclip.com/)(第三方开发的专门用来模拟白卡的设备)，不推荐

 * 软解免费，硬解要花上一笔钱
 * 软解可逆，就是恢复到S-ON状态，有利于保修。而硬解貌似不可逆
 * 软解后功能更完整，如可以使用fastboot，硬解则不能

## Recovery

recovery相当于电脑的一键ghost，刷recovery的前提是你的HBOOT必须是S-OFF

## HBOOT ENG

如果你进入HBOOT界面，第一行有AlpharevX字样，就是用AlpharevX软解的，是工程版(eng)HBOOT，可以直接用fastboot刷机

启动到fastboot

 * 拔掉USB连线，关机
 * 同时按住音量下键和电源键开机，此时进入了HBOOT界面
 * 选择第一项FASTBOOT，按电源键进入fastboot模式，这时上面显示红底白字的FASTBOOT USB
 * 通过USB连接到电脑，电脑显示设备安装成功

刷入recovery，一般都用[clockworkmod](http://www.clockworkmod.com/rommanager)

```
fastboot flash recovery recovery_name.img
```


## 硬解的HBOOT

有些水货机卖家已经给你硬解，特征是HBOOT界面S-OFF信息在第一行，这种机用上面的fastboot刷recovery会出现如下错误
```
writing 'recovery' ... FAILED (remote: not allowed)
```

建议用AlpharevX再软解一遍。之后就可以用fastboot随便刷recovery了。

也可以下载专门的recovery包PG88IMG.zip，不要改名字，放在sd卡根目录，关机，再启动到HBOOT界面，稍等一会儿，HBOOT会自动搜索更新文件刷新recovery。

注：PG88IMG.zip有中文版，可以到一些安卓论坛上搜索


## Rom安装

 * 拷贝rom包zip文件到sd卡根目录
 * 同时按住音量下键和电源键开机，进入HBOOT界面
 * 按音量键选择recovery
 * 清空数据
 * 选择rom包，刷机

## 参考资源

[To install Clockworkmod after Revolutionary](http://forum.xda-developers.com/showthread.php?p=14693680)：S-OFF后怎样安装Clockworkmod Recovery，并提供了recovery和工具的下载

[Unity](http://www.virtuousrom.com/p/unity_23.html)：好用的sense rom

[RomManager](http://www.clockworkmod.com/rommanager)：RomManager提供的clockworkmod和rom下载

[CyanogenMod](http://download.cyanogenmod.com/?type=stable&device=saga)：saga的CM rom