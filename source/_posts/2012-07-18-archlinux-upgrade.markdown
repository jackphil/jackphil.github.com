---
layout: post
title: "archlinux更新日志：迁移/lib到/usr/lib"
date: 2012-07-18 22:16
comments: true
categories: 我爱开源
---

这次更新的一个变化是/lib目录和/usr/lib目录合并了，更新后/lib目录将只是一个指向/usr/lib的链接。由于这次变动牵涉到的软件比较多，Archlinux网站上提供了专门的[升级指导](https://wiki.archlinux.org/index.php/DeveloperWiki:usrlib)

```
sudo pacman -Syu --ignore glibc
sudo pacman -Su
```

正如指导中提到的，第二个命令后还是报/lib已经存在的错误，用如下命令查看哪些目录不属于glibc

```
find /lib -exec pacman -Qo -- {} +
sudo rm -rf /lib/modules
```

我的情况是/lib/modules目录及其子目录下有一些文件，查了一下，没有自己编译的内核模块，所以直接给删了。

升级成功

## 为什么要合并？

也许你和我一样，脑子里有十万个为什么：为什么要合并，合并的好处是什么，我的/usr单独分区怎么办，[FHS](http://www.pathname.com/fhs/)标准改了么

带着这些问号去找谷歌娘，经过几个来回，谷歌娘终于吐露了实情：

这次改变是Fedora主导的，freedesktop上有一个[说明](http://www.freedesktop.org/wiki/Software/systemd/TheCaseForTheUsrMerge)，要点如下：

不止/lib，/bin、/sbin、/lib64都将合并到/usr下对应目录。Archlinux连/lib和/lib64都合并了，它们都指向了/usr/lib，而不是说明中的/usr/lib64

合并后将增加兼容性，给软件维护人员带来福利

* 不同Linux、Unix系统不同打包者有时候会把同一程序放到不同的目录，现在/bin，或者/usr/bin，都将不是问题。我想路径硬编码的情况更多应该发生在脚本中，为什么不干脆把/sbin和/bin也合并了，完全看不出还保留它们的必要
* Solaris 11已经实现了类似的合并，Linux跟进，与主要的商业Unix系统保持一致。有趣的是Solaris 15年前就开始这么做了，而事实上在SysV Unix上，/bin一直是/usr/bin的链接
 
合并方便系统发行商将系统资源放在统一的/usr目录下，发布一个单独的只读/usr分区，多个客户系统可以通过网络或本地方便的共享，客户系统将主要包含用户的配置文件，可以变得更小。关于这一点，我想说，明显受到Android等智能设备的影响。开源对我来说就是给与你完全掌控自己设备的能力，如果我们要一次次期待尼奥们带给我们越狱或root工具，解放/usr可写权限，我想问这还符合开源精神吗？厂商们会说，但为安全故，来把/usr锁。的确，随着人们越来越离不开手机等移动设备，安全问题也越来越不容忽视。我想将来GPL4有没有可能强制要求使用Linux的厂商必须提供一个安全工具，允许用户解锁他们的手机

有人问对于单独的/usr分区，/usr还没挂载怎么启动系统？单独的/bin、/lib目录可以使我们拥有一个最小化的急救系统，回答是这些现在都交给initrd了

总的来说，个人感觉，这次改变更多是由厂商而不是社区用户推动的，上面这些理由对于我这样的个人用户来说并无多少切身体会，说服力不够，至少我从来没遇到过由于/bin、/usr/bin混乱而找不到可执行程序的情况，我想即使偶尔碰到，去目录下做个链接，甚至直接修改脚本都可以很轻松的搞定。当然，我也没什么好反对的，对我来说无所谓，不过最好先把FHS改一改
