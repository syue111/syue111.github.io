---
author: dashashi
comments: true
date: 2012-07-17 08:08:09+00:00
layout: post
slug: nios%e7%94%9f%e6%88%90%e8%bf%87%e7%a8%8b%e4%b8%ad%e6%8f%90%e7%a4%bacygwin-error-remap%e4%b9%8b%e7%b1%bb%e7%9a%84%e9%94%99%e8%af%af%e7%9a%84%e8%a7%a3%e5%86%b3%e5%8a%9e%e6%b3%95
title: NIOS生成过程中提示cygwin error remap之类的错误的解决办法
wordpress_id: 1424
categories:
- 电子设计
---

这两天学了一下NIOS，用Qsys弄好之后编译的过程中提示cygwin error remap之类的错误

一开始找到一个说用rebaseall命令的，没效果，重装NIOS也不行，后来查到一个说是由于诺顿杀毒软件

我的杀毒软件是Avast，但是也有可能，于是禁用之，果断就好了

一直禁用nios毕竟不是办法，其实只要把quartus/bin/cygwin/bin/sh.exe加到Avast的应用程序行为控制的白名单就OK了
