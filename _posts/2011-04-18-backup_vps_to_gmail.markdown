---
author: dashashi
comments: true
date: 2011-04-18 15:37:07+00:00
layout: post
slug: '%e6%8a%8avps%e4%b8%8a%e7%9a%84%e5%86%85%e5%ae%b9%e8%87%aa%e5%8a%a8%e5%a4%87%e4%bb%bd%e5%88%b0gmail'
title: 把VPS上的内容自动备份到Gmail
wordpress_id: 1182
categories:
- Linux
tags:
- dropbox
- ftp
- gmail
- Linux
- vps
- 备份
---

网站备份十分重要啊……百度了下主要有三种方法，一种是用FTP,一种是发到邮箱，还有就是用Dropbox这种东西

由于我没什么FTP空间- -|Dropbox这玩意儿要后台程序，又麻烦，最后还是决定用发送到邮箱的办法

方法的话基本就是[http://www.vpser.net/security/vps-auto-bakup-send-by-gmail.html](http://www.vpser.net/security/vps-auto-bakup-send-by-gmail.html)里面的方法

里面用到mysqldump导出数据，而lnmp装好的mysql并没有把mysqldump在的目录加到PATH里面，还要自己弄一下

另外就是tar.gz压缩出来的东西在Windows解压缩会有乱码- -|不过在Linux下面解压缩测试了一切正常
