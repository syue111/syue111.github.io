---
author: dashashi
comments: true
date: 2011-04-18 15:29:49+00:00
layout: post
slug: linux%e4%b8%8b%e7%9a%84%e4%b8%8b%e8%bd%bd%e5%b7%a5%e5%85%b7
title: Linux下的下载工具
wordpress_id: 1180
categories:
- Linux
tags:
- Debian
- Linux
- php
- vps
---

话说美国的VPS装东西的时候需要下载国外的数据时速度真是打了鸡血一样地快啊，不过唤作下载国内的东西就慢得蛋疼了，wget单线程下载速度实在蛋疼，百度了一下，最后找到axel这玩意儿- -|几十K的速度开了多线程能到几百K，安装命令如下- -|

wget  http://alioth.debian.org/frs/download.php/3015/axel-2.4.tar.gz
tar zxvf axel-2.4.tar.gz
cd axel-2.4
./configure
make
make install

于是果断可以了- -|
