---
layout: post
title: "我的Desire S，我的ICS"
date: 2012-10-02 16:11
comments: true
categories: android
---

先前CM9.0发布，没有Desire S版本，失望之余跑去xda上询问[为什么](http://forum.xda-developers.com/showthread.php?t=1830505)，得到答案是因为HTC没有发布内核源码，而且CM9.0几乎不会更新，开发者已经转移到Android 4.1也就是CM10上了。之后[HTCDev](http://www.htcdev.com/)上源码放出也证实了这点，依然没有Desir S的CM9

xda上rom很多，眼花缭乱，正为选择性障碍发愁时，看到MIUI上有4.0刷机包，教程也很傻瓜，一直久仰大名，从未用过，老鼠虽然爱的是大米，在地主家也没有余粮的年份不妨吃一回小米。

按教程刷机简单总结如下：

 * 升级hboot，利用hboot自动刷的PG88IMG.zip，版本[7.00.1002](http://forum.xda-developers.com/showthread.php?t=1679338)
 * 刷官方4.0，依然通过hboot刷PG88IMG.zip，这一步主要是用来刷radio 20.72.30.0833U_3831.17.00.310
 * 刷recovery，我装的是[4EXT Recovery Touch](http://forum.xda-developers.com/showthread.php?t=1377745)
 * 刷小米rom，我习惯用recovery刷

上面提到的hboot、官方rom刷机包都在小米提供的工具包里。

本来刷了官方4.0 rom后，想就用sense算了，不想刷小米了，无奈有个硬伤，账户和同步里不能添加谷歌帐号，人人之类倒有一大堆。结果用了一段时间小米，总觉得不满意，一次出差差点没接到电话后，终于下定决心换rom。

首选是曾经用过的virtuous，岂料今非昔比，内核不是原生，从其它机型移植过来的，beta版也算了，最大问题是居然没有中文。

在一阵迷茫后，选中了[ICE DS](http://forum.xda-developers.com/showthread.php?t=1567715)。

 * 先刷`Full_wip_DS.zip`进行清理，再刷rom包
 * recovery选中rom包进入刷机过程后，惊讶的发现是一个图形触摸界面，你可以进行一些配置，选择要安装的组件，next，就象Windows上装一个普通软件一样简单直观
 * 可选组件有Bravia Engine，Stock Battery，Beats Audio，HTC IME with Arrows
   * Bravia Engine是图像处理引擎，拍照增强，据说弱光下有更好效果
   * Stock Battery貌似是1%进度显示电量百分比
   * Beats Audio魔音，是音效增强
   * HTC IME with Arrows是HTC输入法带方向键，我猜类似下面这样的，没选。{% img https://lh5.googleusercontent.com/-w9lp6LOQjeY/UGHFhOd_aJI/AAAAAAAAKSA/33FEP12ZRIs/s288/mfc.jpg %}
 * 此外最大的惊喜是内核带sweep2wake功能，杀手级应用，一下子就让我喜欢上了，之前我一直是用音量键唤醒（rom支持）和搜索键关屏（使用一款叫[关屏锁定](https://play.google.com/store/apps/details?id=com.katecca.screenofflock)的APP，又名Screen Off and Lock）
 
喜欢就是全部，不喜欢就是零。由漂亮的安装界面和sweep2wake喜欢上了ICE DS rom，虽然在后面的使用中，也陆续碰到了小米时期的一些问题，使我知道我的那些问题并不是小米的错，但我已是变心的情侣，眼睛里容不下一粒小米。

 * 下拉菜单的快速设置里没有静音选项，一个将就的办法是铃声想起，可以把手机翻个身面朝下
 * 上面提到小米的“罪证”之一接电话卡，是因为我设置了QQ通讯录的来点显示界面，和rom自带的有冲突，两个会同时显示，解两次锁才能接听，解决是QQ通讯录里设置来电显示为系统默认
 * [通话录音](https://play.google.com/store/apps/details?id=com.skvalex.callrecorder)不能录音的问题。去设置里把录音格式改为amr即可。是app的问题。
 
我想，在CM10正式发布前，我会一直用它了
