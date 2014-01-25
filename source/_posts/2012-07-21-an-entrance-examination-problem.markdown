---
layout: post
title: "一道高考数学题"
date: 2012-07-21 11:00
comments: true
categories: 
---

本篇主要用来测试kramdown+mathjax。符号表达参考[维基百科](http://zh.wikipedia.org/zh/Help:%E6%95%B0%E5%AD%A6%E5%85%AC%E5%BC%8F#.E6.A0.87.E5.87.86.E5.87.BD.E6.95.B0)，具体用法则参考了[kramdown文档](http://kramdown.rubyforge.org/syntax.html#math-blocks)

题目来自今年高考全国卷大纲版理科数学： $\Delta ABC$的内角$A,B,C$的对边为$a,b,c$，已知$\cos(A-C)+\cos B=1$，$a=2c$，求$C$

<!--more-->

解答：

$\because$ A、B、C是三角形内角，故有$B=\pi -(A+C)$

$\therefore$

$$
\begin{align*}
 & \cos(A-C)+\cos B=\cos(A-C)+\cos[\pi -(A+C)] \\
 & =\cos(A-C)-\cos(A+C)= \\
 & 2\sin A\sin C=1\tag{1}
\end{align*}
$$

$\because a=2c$

$\therefore$ 根据正弦定理，有$\sin A=2\sin C$，代入式(1)，得$\sin^{2} C=\frac{1}{4}$，

$\because 0<C<\pi$，

$\therefore$

$$
\begin{align*}
 & \sin C=\frac{1}{2}
\end{align*}
$$

$\because a=2c$，有$a>c$，根据三角形大边对大角的性质，C不可能是钝角

$\therefore$

$$
\begin{align*}
C=\frac{\pi}{6}
\end{align*}
$$

$Q.E.D$

