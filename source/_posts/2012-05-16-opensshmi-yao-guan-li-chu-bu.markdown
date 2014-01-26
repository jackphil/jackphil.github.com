---
layout: post
title: "OpenSSH密钥管理初步"
date: 2012-05-16 20:09
comments: true
categories: 我爱开源
---

github需要ssh密钥来建立push时的安全连接，虽然以前在用gitorious时也接触过，但已经不记得了，还是找了几篇教程来边看边做，笔记如下

<!--more-->

## 生成密钥对

```
sudo pacman -S openssh # ssh-keygen在openssh包里
ssh-keygen -t rsa
```

一路默认回车即可。你也可以在出现如下提示时为你的私钥添加密码短句进行加密

```
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```

## 配置github
默认会在~/.ssh目录下生成私钥id_rsa和公钥id_rsa.pub，将公钥文件内容添加到github.com：账户设置-SSH Keys

根据[github官方文档](http://help.github.com/linux-set-up-git/)，一些教程里提到的GitHub token不再会有支持

你可以测试一下

```
ssh -T git@github.com
```
如果出现You’ve successfully authenticated字样，说明一切正常

## 安全但麻烦的passphrase(密码短句)
如果你在生成密钥对时设置了密码短句，则每次提交连接github都会询问你密码短句来解密私钥，我虽然平时除了github，很少用到ssh，但也颇感麻烦，一个简单解决办法是使用ssh-agent

```
sudo pacman -S kde-agent # ssh-agent随kde启动
ssh-add ~/.ssh/id_rsa # 会询问你密码短句
```

每次启动kde会话后，都要手动添加私钥到ssh-agent，你只需在此输入一次密码短句，此后只要ssh-agent进程不重启，你都不再需要输入密码短句

### 更新

2014-1-26: kde-agent从20140102-1版本起，取消了ssh-agent的相关功能。要随kde会话启动和退出ssh-agent，可以使用如下脚本：

```
echo -e '#!/bin/sh\n[ -n "$SSH_AGENT_PID" ] || eval "$(ssh-agent -s)"' > ~/.kde4/env/ssh-agent-startup.sh
echo -e '#!/bin/sh\n[ -z "$SSH_AGENT_PID" ] || eval "$(ssh-agent -k)"' > ~/.kde4/shutdown/ssh-agent-shutdown.sh
chmod 755 ~/.kde4/env/ssh-agent-startup.sh ~/.kde4/shutdown/ssh-agent-shutdown.sh
```

## 参考资源

[通用线程: OpenSSH 密钥管理](http://www.ibm.com/developerworks/cn/linux/security/openssh/part1/index.html): IBM网站上2001年的一篇教程，是由gentoo创始人写的，共有两部分，这是第一部分介绍基础知识，第二部分介绍ssh-agent和keychain

[Gentoo Linux Keychain指南](http://www.gentoo.org/doc/zh_cn/keychain-guide.xml): Gentoo的一篇教程

[SSH Keys](https://wiki.archlinux.org/index.php/SSH_Keys#ssh-agent): Archlinux的wiki文档
