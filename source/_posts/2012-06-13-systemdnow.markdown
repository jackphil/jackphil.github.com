---
layout: post
title: "高速启动，现在开始systemd"
date: 2012-06-13 13:15
comments: true
categories: 我爱开源
---

最近一次系统更新，Archlinux用systmed-tools替换了udev。

我年纪渐大，响应胡总号召，不象以前那么爱折腾了。

有人问Lennart创建systemd的动机，回答是“爱折腾”

该折腾还得折腾，与各位Linuxer共勉

## 什么是systemd： 一些八卦

systemd被设计用来改进sysvinit的缺点，它和ubuntu的upstart是竞争对手，预计会取代它们

systemd的很多概念来源于苹果的launchd

创始人Lennart是redhat员工，但systemd不是redhat项目

systemd的目标是：尽可能启动更少进程；尽可能将更多进程并行启动

systemd尽可能减少对shell脚本的依赖。传统sysvinit使用inittab来决定运行哪些shell脚本，大量使用shell脚本被认为是效率低下无法并行的原因

systemd使用了Linux专属技术，不再顾及POSIX兼容，一度谣传Debian为了它的BSD项目将不会使用systemd

天下武功，唯快不破，加速吧，Linux！

<!--more-->

## 安装

```
sudo pacman -Rcsn sysvinit syslog-ng
sudo pacman -S systemd systemd-arch-units systemd-sysvcompat
```

删除sysvinit，安装systemd-sysvcompat，我更喜欢这样一个纯的systemd环境。还有一个好处是可以不用设置内核启动参数``init=/bin/systemd``

系统升级的时候只给我用systemd-tools替代了udev，但没装管理工具包systemd，网络也没有，害得我不得不到别的机子上下好，再拷过来安装

## 服务管理

```
systemctl is-enabled <program>.service #查询服务是否开机启动
sudo systemctl enable <program>.service #开机运行服务
sudo systemctl disable <program>.service #取消开机运行
sudo systemctl start <program>.service #启动服务
sudo systemctl stop <program>.service #停止服务
sudo systemctl restart <program>.service #重启服务
sudo systemctl reload <program>.service #重新加载服务配置文件
systemctl status <program>.service #查询服务运行状态
systemctl --failed #显示启动失败的服务
```
systemctl命令取代了rc.d命令

## 开机模块加载

/etc/modules-load.d/<program>.conf，相当于原rc.conf中的MODULES变量

```sh
# Load virtio-net.ko at boot
virtio-net
```

模块黑名单仍在/etc/modprobe.d/下，如blacklist.conf:

```
blacklist badmod.ko
```

## Locale
/etc/locale.conf，相当于原rc.conf中的LOCALE

```
LANG=en_US.UTF-8
LC_COLLATE=C
```

## 日志服务
systemd自带日志服务，参考[systemd Journal](http://linuxtoy.org/archives/systemd-journal.html)

```
sudo journalctl
```
可以删除syslog-ng了

## 主机名

/etc/hostname，相当于原来rc.conf中的HOSTNAME变量

```
myhostname
```

## 网络

```
sudo systemctl enable NetworkManager.service
```
不象rc.conf有专门的配置简单网络的地方，还是用NetworkManager、wicd之类的工具吧

如果你坚持使用简单静态配置，可以参考[[SOLVED] static ethernet setup under systemd?](https://bbs.archlinux.org/viewtopic.php?pid=1110003)

## 运行级别

systemd用target替代了runlevel的概念，提供了更大的灵活性，如你可以继承一个已有的target，并添加其它服务，来创建自己的target

```
sudo systemctl list-units --type=target #查询当前target
sudo systemctl isolate graphical.target #改变当前target，重启无效
sudo systemctl enable multi-user.target #改变启动时默认target
sudo systemctl enable kdm.service #graphical是默认target，指定使用的display manager
```

## 优化

systemd有自己的"e4rat"

```
sudo systemctl enable systemd-readahead-collect.service
sudo systemctl enable systemd-readahead-replay.service
```

/etc/fstab，修改/home分区options，检查/home分区时并行启动其它服务

```
defaults,noauto,x-systemd.automount
```

## 其他

```
sudo systemctl reboot #systemctl还有系统关机、重启、挂起等功能
sudo systemctl suspend
```

## 参考资源

[systemd-Archlinux Wiki](https://wiki.archlinux.org/index.php/Systemd): 本文基本上可以说是此文的翻译

[systemd on freedesktop](http://www.freedesktop.org/software/systemd/man/systemd.service.html): systemd官方文档，如欲进一步研究，比如service文件中各项含义等，请移步

[采访 Systemd 和 PulseAudio 创始人 Lennart](http://linuxtoy.org/archives/interview-creater-of-systemd-and-pulseaudio-lennart.html): Lennart是可有趣的人，文后链接中还能找到许多有用的文章
