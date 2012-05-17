---
layout: post
title: "Octopress技巧集"
date: 2012-05-16 21:21
comments: true
categories: 
---

本文收集Octopress使用过程中学到的一些技巧，会经常更新（当然还要看我心情懒不懒得折腾）

<!--more-->

## 首页文章只显示摘要

在正文中插入标记
```
正文开头内容，也是首页摘要显示的部分
<!--more-->
以下内容不会显示在首页文章列表中
```

编辑_config.yml‵‵
```
excerpt_link: "阅读全文" # 用于提示阅读完整文章的文字链接
```

## 绑定自家域名

创建一个CNAME文件
```
echo 'your-domain.com' >> source/CNAME
```
去你自家域名服务提供商修改设置

* 如果是顶级域名，修改A记录，使之指向207.97.227.245
* 如果是二级或以下域名，修改CNAME记录即可，使之指向username.github.com

一般deploy后，github这边需要10分钟左右才生效

## 添加统计代码

我用的是[百度统计](http://tongji.baidu.com)，将代码添加到source/_includes/custom/footer.html中即可
```
<p>
  Copyright &copy; {{ site.time | date: "%Y" }} - {{ site.author }} -
  <span class="credit">Powered by <a href="http://octopress.org">Octopress</a></span> -
 <script type="text/javascript">
var _bdhmProtocol = (("https:" == document.location.protocol) ? " https://" : " http://");
document.write(unescape("%3Cscript src='" + _bdhmProtocol + "hm.baidu.com/h.js%3F7b011ba580af2ede1ka05568d5d4828a' type='text/javascript'%3E%3C/script%3E"));
</script>
</p>
```