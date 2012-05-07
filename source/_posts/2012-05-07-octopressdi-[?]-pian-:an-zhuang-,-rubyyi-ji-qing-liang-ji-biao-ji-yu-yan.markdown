---
layout: post
title: "Octopress第一篇：安装、ruby以及轻量级标记语言"
date: 2012-05-07 23:23
comments: true
categories: 
---

作为一个pythoner，知道jekyll/octopress是ruby所写后，首先想到的是有没有相应的pythonic工具，[这里](http://wiki.python.org/moin/PythonBlogSoftware)有一个不完全列表

尝试过[hyde](http://ringce.com/hyde)

* 它最吸引我的是支持我喜爱的Asciidoc
* 可是默认模板实在太丑陋
* 文档简陋，没用过django，连添加文章的命令都没有，手动拷贝修改了一篇默认生成的文章，折腾很久都没能把文章添加到默认首页，当然也许是我学艺不精，但我已经不喜欢它了
* 看上去完成度这么低的项目，看github上仓库的最近提交，居然都是a year ago

看了下[Tinkerer](http://tinkerer.bitbucket.org/index.html)，界面不错，基于Sphinx，可惜对此不感兴趣

还有一个列表里没提到的[pelican](https://github.com/ametaireau/pelican)，貌似用的人还不多

最近python-cn列表里有人[兴高采烈](https://groups.google.com/forum/?fromgroups#!topic/python-cn/Xh1LGTGW20A)的开了个octopress博客，正好我前段时间也看过教程，想趁机了解一下ruby，于是决定试一把octopress

## 安装
[Octopress的文档](http://octopress.org/docs/)清晰明了，网上安装记录、教程一搜一大堆，我也不啰嗦，做个备忘，罗列一下命令，谈一点想法吧

```
bash < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer) # 还需要在.bashrc中添加一些配置
rvm install 1.9.2
rvm use 1.9.2 --default
git clone git://github.com/imathis/octopress.git octopress
cd octopress # 会询问你是否信任.rvmrc文件，选择yes
gem install bundler
bundle install
bundle update # 不执行，则下一步rake会报版本不匹配
rake install
rake setup_github_pages
```

下面是日常使用的命令：

```
rake new_post["My first post"]
rake generate # 第一次使用，最好先编辑一下_config.yml
rake preview # 本地查看，地址是http://localhost:4000
rake deploy
git add . # 以下几句是把markdown源文件也提交到仓库
git commit -m 'your message'
git push origin source
```

## 关于Ruby
这是我第一次真正和ruby打交道，在我的Archlinux系统上也没有因为依赖关系而装上ruby。于是我想有没有一种绿色、非root方式安装ruby。

我需要一个独立于系统的python环境时，就直接把archlinux的python包解压缩到用户目录，所以一度也想对ruby这么干。但在仔细阅读安装教程和了解了各命令的用途后，非常惊讶，ruby的方式是那么干净、简单和清晰。

是的，这就是我第一次使用rvm、gem、bundle、rake之后的感受。而python下的virtualenv、setuptools、pip之类，就显得丑陋了。网上也有人发出过[亲爱的python，你为什么这么丑](http://grokcode.com/746/dear-python-why-are-you-so-ugly/)的疑问，在比较过ruby和python应用的用户界面后，ruby的性感，python的丑陋更是一目了然

这不由让我想起了之前从GNOME转到KDE时见异思迁的痛苦，同一个功能，比如ftp客户端，KDE下的应用总是比GNOME无论是从界面还是细节上，都更能抓住我的心，但当时还在用Ubuntu，GNOME是默认的桌面环境，当时还被一些诸如“GNOME更开放更自由”之类开源原教旨主义观点困扰，着实纠结了一番。

好吧，作为近10年的pythoner，我还没叛变，我只是对眼前漂亮的日本妞吹了吹口哨，回头搂住洋老婆扬长而去。“那妞虽漂亮，可惜是个日本人，而且我更了解我老婆，能力强，人脉广，有个好工作，在google上班呢，过日子还得找这样的”，我想

## 关于轻量级标记语言
我最早使用的轻量级标记语言是[reStructuredText](http://docutils.sourceforge.net/)，后来学过docbook(哦，这个不算轻量级了吧)，但没真正用它写过任何东西。

现在最喜欢的的是[Asciidoc](http://www.methods.co.nz/asciidoc/)，可惜rst已经成了事实上的标准

我从没用过markdown，这也是我几乎纠结一下午的原因，我不想再学一门标记语言，Octopress显然不支持我最喜欢的Asciidoc，退而求其次想用rst，如有可能，只使用纯pythonic工具，这也是作为一个python的强迫症吧，但在看了一下[jekyll-rst](https://github.com/xdissent/jekyll-rst)后，我发现不太喜欢这种python/ruby混合的方式，也许是年纪大了经不起折腾，也许是我的洁癖又犯了，也许两者都有……

所以最后还是选择了markdown，给了自己两个理由，人是需要理由的动物：

* markdown语言中立，跨语言性更好。比如rst就没有php的实现，而我可以在wordpress中使用markdown
* markdown足够简单，可以现学现用，学习成本低。简单文档使用markdown，复杂文档可以使用Asciidoc

