---
layout: post
title: "Emacs24的二三事"
date: 2012-06-13 21:39
comments: true
categories: emacs
---

今天，NBA总决赛第一场，24再次爽约，站在23对面的，是更年轻的35，23似乎要再度饮恨。

Emacs23和Emacs24也不那么和谐，原来的配置，启动直接报错，调教一番是免不了了

## color theme

根据[Color Theming in Emacs: Reloaded](http://batsov.com/articles/2012/02/19/color-theming-in-emacs-reloaded/)，Emacs24自带了几个颜色主题，我的选择是

```
(load-theme 'tango-dark t)
```

原来的color-theme不兼容24，可以删除之

## 菜单栏和工具栏

隐藏菜单栏和工具栏，原来给变量赋值nil不再有效，其实nil不再被用来开关minor-mode，有人解释了[原因](http://stackoverflow.com/questions/9423974/emacs-menu-bar-mode-and-tool-bar-mode-automatically-set-to-t)

```
;;#隐藏菜单和工具栏
(tool-bar-mode -1) 
(menu-bar-mode -1)
```

## 行号

setnu+是另一个引起麻烦的包，24自带的功能似乎也不错，删除之

```
M-x linum-mode
```

## tabbar

标签栏只能显示当前buffer，功能不正常，平时几乎很少用到，也把它删了

## 软件包管理(ELPA: Emacs Lisp Package Archive) 

Emacs24的一大亮点是自带了[包管理器](Emacs Lisp Package Archive)(不过并非24独享，22都可以装)，自动安装管理各种lisp功能包

```
;;#软件包服务器
(setq package-archives '(("gnu" . "http://elpa.gnu.org/packages/")
			 ("marmalade" . "http://marmalade-repo.org/packages/")))
```

试用了一下，列出所有软件包：

```
M-x package-list-packages
```

C-s搜索，i标记安装，d标记删除，x执行，.emacs.d目录下会生成elpa目录

装了个markdown-mode.el，还是要手动改一下配置，自己在auto-mode-alist注册文件类型。

总的来说很清爽，软件包都在elpa下自己的目录里，也没有自作主张的修改init.el，值得一试
