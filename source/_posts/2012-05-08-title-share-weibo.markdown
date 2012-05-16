---
layout: post
title: "Octopress第二弹：中文文件名、分享按钮、侧边栏上微博秀、disqus评论"
date: 2012-05-08 22:32
comments: true
categories: 
---

今天对博客进行了一番装修和调整，我对美工要求不高，对现在的样子基本满意了

## 不要在文件名中使用中文标点

```
rake new_post["My first post"] # 不建议在这里使用中文标点
```

Octopress可以把中文文件名转换成拼音，但中文标点会造成一些麻烦，使得url中包含一些奇怪的符号，有时会造成问题。我曾碰到过在本地预览时能显示文章列表，但不能打开文章，显示找不到文件。将文件名改为英文后解决。需要说明的是提交到github在线浏览没有这个问题

这里说的是文件名，并不影响你的文章标题中包含中文标点。你可以在文章内文的yaml front matter中的title字段随意使用中文标点，

## 分享按钮
我选择了[百度分享](http://share.baidu.com)，将获得的代码添加到source/_includes/post/sharing.html，你也可以依样画葫芦，把代码放在模板if结构中，

```
{% if site.baidu_share %}
代码
{$ endif %}
```
别忘了在_config.yml定义一下开关，如
```
# 百度分享
baidu_share: true
```

## 微薄秀
微博秀参考了[程序猎人的文章](http://programus.github.com/blog/2012/03/03/add-weibo-sidebar-into-octopress/)，不过

* 偷懒，直接用了从[微博秀](http://weibo.com/tool/weiboshow)得来的代码，没有把url中的参数作变量替换。
* 根据_confit.yml中default_asides的注释，把weibo.html放在了/source/_includes/custom/asides/目录下

```
{% if site.weibo_sina %}
<section>
  <h1>新浪微博</h1>
  <ul id="weibo">
    <li>
      <iframe 
        width="100%" 
        height="550" 
        class="share_self" 
        frameborder="0" 
        scrolling="no" 
        src="http://widget.weibo.com/weiboshow/index.php?language=&width=0&height=550&fansRow=2&ptype=1&speed=0&skin=1&isTitle=1&noborder=1&isWeibo=1&isFans=1&uid=2039187623&verifier=e0a28d12&dpc=1">
      </iframe>
    </li>
  </ul>
</section>
{% endif %}
````

别忘了在_config.yml中的default_asides列表中添加微博秀并在文件尾添加开关

## DISQUS评论
DISQUS评论功能是内置的，开启很简单

* 注册DISQUS帐号，添加站点
* 在_config.yml中填入site shortname即可