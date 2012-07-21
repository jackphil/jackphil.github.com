---
layout: post
title: "archlinux快速设置"
date: 2012-05-30 14:45
comments: true
categories: 我爱开源
---

按[Archlinux简易安装指南](http://blog.jackphil.com/blog/2012/05/30/archlinuxjian-yi-an-zhuang-zhi-nan/)装好系统后，还只是个毛坯房，住不得人，这里介绍的软件和配置，好比装修和家具，在我看来，都是必不可少的，他们带给我“家”的感觉

<!--more-->

## 显卡
笔记本是ATI显卡，我选择了开源驱动

添加源，在/etc/pacman.conf最后添加

```
[radeon]
Server = http://spiralinear.org/perry3d/x86_64
```

```
pacman -S xf86-video-ati-git #安装驱动
```

建议开机加载驱动，/etc/rc.conf

```
MODULES=(... radeon ...)
```

## 桌面
桌面我选择的是KDE

```
sudo pacman -S kdebase kde-l10n-zh_cn xorg-server xorg-xinit ttf-dejavu
```

* 我这里选择了最小安装: kdebase
* 如果你不使用kdm或gdm这类登录管理器，而使用startx启动桌面，则需要装xorg-xinit
* 最小安装kde甚至都不安装英文字体，如果没有合适的字体，kde启动后将停在splash画面，所以你需要安装一个字体，如ttf-dejavu，同样的如果你的locale是中文，但却没有中文字体，你也会进不了桌面，建议安装wqy-bitmapfont、wqy-zenhei
* 如果使用startx启动桌面，环境配置文件是.xinitrc

{% gist 2834391 %}

* 如果使用kdm登录管理器，环境配置文件是.xprofile
{% gist 2834351 %}

* xorg的配置文件在/etc/X11/xorg.conf.d/目录下，你可以依样创建自己的配置文件，不过现在大部分情况下，xorg都能很好的识别设备（显卡、鼠标键盘等）并加载相应的模块，不需要额外配置

/etc/rc.conf

```
DAEMONS=(... dbus ...)
```
KDE使用dbus作为进程间通信机制


## KDM
kdm在kdebase-workspace包里，即使是最小安装kde，也已经顺带装上了

开机运行，建议修改/etc/inittab，取消注释以下两行

```
id:5:initdefault:
...
x:5:respawn:/usr/bin/kdm -nodaemon
```

可以在登录KDE后，在"系统设置""-"登录屏幕"中进行设置，如你可以在"便利"标签下设置开机自动登录

## 触摸板

```
sudo pacman -S xf86-input-synaptics # 安装驱动
ican toggletouchpad
```
如果你的笔记本没有相应的触摸板开关Fn组合键，可以使用[ican](https://gist.github.com/2836665)工具

## 输入法

```
sudo pacman -S ibus ibus-pinyin ibus-qt
ibus-setup # 初始化设置
```

* 为了能在qt程序中使用输入法，需要安装ibus-qt
* ibus只是输入法框架，你需要在初始化设置中添加拼音输入法

配置随桌面启动，在[.xinitrc](https://gist.github.com/2834391)或[.xprofile](https://gist.github.com/2834351)设置

## 终端程序

```
sudo pacman -S yakuake
```

yakuake基于konsole，我习惯设置konsole字体为14号
