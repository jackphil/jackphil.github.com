---
layout: post
title: "Archlinux简易安装指南"
date: 2012-05-30 13:57
comments: true
categories: 我爱开源
---

江湖传说，屌丝装电脑，高富床上搞，最惨的还是装企鹅，只能自己在床上装电脑

据说老毛床上微软，因为他喜欢蓝萍，我们拒绝微软，厌恶蓝屏，各位看官，你准备好了么

<!--more-->

## 准备安装环境

我喜欢U盘安装，绿色方便，格式化硬盘什么的也没有限制

[Universal USB Installer](http://www.pendrivelinux.com/universal-usb-installer-easy-as-1-2-3/)可以方便的将iso文件制作为启动U盘。这个方法的一个缺点是你要准备一个干净的U盘，上面原有的文件都会被抹去。

其实我使用的是移动硬盘，大量文档备份转移的话会很麻烦，幸好我装有[grub4dos](http://code.google.com/p/grub4dos-chenall/)。archlinux的iso镜像使用syslinux作为启动引导，稍微研究一下它的配置文件syslinux.cfg，可以很容易的转换为grub的配置格式，以archlinux-2011.08.19-core-x86_64.iso为例，下面是我的menu.lst

```
title Boot Arch Linux Live CD
kernel /arch/boot/x86_64/vmlinuz archisobasedir=arch archisolabel=MYLABEL
initrd /arch/boot/x86_64/archiso.img
```

* 你只需把iso中arch目录拷贝到移动硬盘根目录即可
* MYLABEL是移动硬盘启动分区卷标
* 网络环境允许（如无线网卡驱动能自动识别），还是建议选择netinstall镜像

[UNetbootin](http://unetbootin.sourceforge.net/)似乎可以自动化上面的工作，有兴趣可以尝试一下。

## 安装界面

* 软件源选163的，我这里能满速
* 网络设置如果是无线网卡，Ctrl+Alt+Fn切换到别的控制台

```
wpa_passphrase mywireless_ssid "secretpassphrase" > /etc/wpa_supplicant.conf
ip link set wlan0 up
wpa_supplicant -B -Dwext -i wlan0 -c /etc/wpa_supplicant.conf
dhcpcd wlan0 # 或者回到安装界面指定静态地址
ip addr show wlan0
ping www.163.com # 测试一下
```
* 分区：
    + 我一般根目录(含boot)20G，其余给home
    + 我现在6G内存没设swap分区，用了一阵没发现什么问题。一般就内存1-2倍，封顶1G吧（我虚拟机给了1G内存，安装程序自动分配给了256M交换分区）
    + 分区格式ext4或btrfs
* 挑选软件时只选一下sudo，我喜欢进入系统后再一点点添加
* 修改配置文件：
    + rc.conf: 设一下主机名HOSTNAME
    + resolv.conf: dns设置，我一般第一个设为8.8.8.8，第二dns为本地电信提供的dns地址
    + local.gen，选择zh_CN.utf8，使用编辑器搜索功能加快定位
    + 设置root密码


## 第一次进入系统

* 添加用户：``useradd -m -g users -G wheel -s /bin/bash myname``
* 为刚添加的用户设置密码：``passwd myname``
* 修改sudo配置：visudo，我一般允许wheel用户组运行任何命令，且无需密码
* 退出root，用新用户名登录

美女已经入室，接下来就看你怎么调教了
