---
layout: post
title: "Archlinux更新记录20120516"
date: 2012-05-16 15:27
comments: true
categories: 
---

某次更新系统后重启，发现有触目的红色警告，由于屏幕滚动很快，没看清具体是什么内容。遂去/etc/rc.local中添加一句read，以使启动最后暂停，等待用户输入，任意键继续。

这次看清是ALSA出错了
```
Unknown hardware "foo" "bar" ...
Hardware is initialized using a guess method
```

搜索后发现可能跟内核升级有关，解决也很简单
```
sudo alsactl -f /var/lib/alsa/asound.state store
```

参考：[Error 'Unkown hardware' Appears After a Kernel Update](https://wiki.archlinux.org/index.php/Advanced_Linux_Sound_Architecture#Error_.27Unkown_hardware.27_Appears_After_a_Kernel_Update)