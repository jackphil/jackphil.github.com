---
layout: post
title: "Archlinux和VirtualBox"
date: 2012-05-30 16:30
comments: true
categories: 我爱开源
---

Archlinux我所欲也，VirtualBox亦我所欲也，两者可以得兼，AV是也

现在机器性能越来越强，新机器上跑的虚拟机性能不会比5年前的电脑差，如果只是上上网聊聊天，听听音乐看看电影，简直就是浪费。

我经常尝试新系统、新软件，自从有了虚拟机就象是孙悟空得了那金箍棒，腰不酸腿不疼，吃嘛嘛香，花果山我的地盘我做主

再说现在虚拟化和云计算一样，正时髦呢，好比杨幂上戛纳，咱显摆的就是虚拟

维基百科上有张[虚拟机软件比较表](http://zh.wikipedia.org/wiki/%E7%B3%BB%E7%BB%9F%E8%99%9A%E6%8B%9F%E6%9C%BA%E6%AF%94%E8%BE%83#.E8.99.9B.E6.93.AC.E6.A9.9F.E5.99.A8.E6.AF.94.E8.BC.83)，我只用过其中的bochs、qemu、virtualbox、vmware。据说内核开发bochs比较好，我还想学学内核，当会专文记录，暂且按下不表。从开源和性能方面考虑，virtualbox实在是居家旅行杀人灭口的必备良药啊

<!--more-->

## 在Archlinux中安装VitualBox

```
sudo pacman -S virtualbox
sudo gpasswd -a $USER vboxusers
```
virtualbox安装后，系统中多了个vboxusers用户组，只有属于该组的用户能使用虚拟机

/etc/rc.conf
```
MODULES=(…… vboxdrv vboxnetflt)
```

/etc/rc.local
```
# Dry-load vbox* modules and trigger a rebuild if modprobe fails
modprobe -nqs vbox{drv,pci,net{flt,adp}} >/dev/null 2>&1 || ( /usr/bin/vboxbuild && . /etc/rc.conf && modprobe -ab ${MODULES[*]} )
```
内核更新后需要重新编译模块，上面提供的方案先尝试加载模块，如果失败则自动编译

[官方wiki](https://wiki.archlinux.org/index.php/VirtualBox#Automatic_re-compilation_of_the_virtualbox_modules_with_every_kernel_update)上现在推荐的方法是使用mkinitcpio hook，以便在内核更新时会自动编译，相关hook在aur中

## 在VirtualBox中安装Archlinux

按通常方法在virtualbox中安装archlinux后，archlinux还只是“可用”，要“好用”就需要一些特别设置和调整

```
sudo pacman -S virtualbox-archlinux-additions
sudo groupadd vboxsf 
sudo gpasswd -a $USER vboxsf
```
要实现开机自动加载共享目录等功能，用户必须是vboxsf组成员，你需要手工创建该用户组

/etc/rc.conf
```
MODULES=(... vboxguest vboxsf vboxvideo)
……
DAEMONS=(... vbox-service ...)
```
vbox-service默认开启了很多实用功能，如与host主机同步时间，开机加载共享目录等，你也可以用VBoxService命令手动控制它们

.xinitrc
```
VBoxClient-all &
exec ck-launch-session startkde
```
VBoxClient命令提供了剪贴板共享等高级桌面功能，VBoxClient-all用来开启所有这些功能

创建/etc/modprobe.d/blacklist.conf
```
blacklist i2c_piix4
```
虚拟机不含SMBus系统总线，启动时udev会有报错信息，将i2c_piix4模块列入黑名单即可


## 技巧及其它

* 共享文件夹在客户机中会被挂载到/media目录下的sf_sharedfolder目录