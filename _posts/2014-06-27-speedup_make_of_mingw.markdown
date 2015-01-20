---
author: dashashi
comments: true
date: 2014-06-27 18:28:36+00:00
layout: post
slug: '%e6%8f%90%e9%ab%98mingw%e4%b8%8b%e7%9a%84make%e9%80%9f%e5%ba%a6'
title: 提高MinGW下的make速度
wordpress_id: 1498
categories:
- 写程序
---

用gcc编STM32，发现make的过程很慢，每次编译之前都有接近1分钟的等待，<!-- more -->放狗一搜，发现4年前已经有人提交bug了，在http://sourceforge.net/p/mingw/bugs/1476/

里面提到用[http://prdownloads.sourceforge.net/mingw/csmake-3.81-MSYS-1.0.11-2.tar.bz2?download](http://prdownloads.sourceforge.net/mingw/csmake-3.81-MSYS-1.0.11-2.tar.bz2?download)这个链接的csmake，能够提高速度

从上面的链接下载后解压缩，将csmake.exe复制到MSYS的bin文件夹下，我的是D:\MinGW\msys\1.0\bin，并将名字改为make.exe，如果怕出问题可以先把原先的make.exe改名为make-bak.exe做备份。

具体原理是在于linux下文件名是区分大小写的，而Windows下面文件是不区分大小写的，MinGW的make对文件名的大小写进行了一定的处理，导致速度变慢。

不过侧面体现出……MSYS已经4年多没更新了……
