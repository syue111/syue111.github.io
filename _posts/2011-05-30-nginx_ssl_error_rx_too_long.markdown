---
author: dashashi
comments: true
date: 2011-05-30 06:24:52+00:00
layout: post
slug: nginx%e9%85%8d%e7%bd%aessl%e8%bf%94%e5%9b%9essl_error_rx_record_too_long%e7%9a%84%e8%a7%a3%e5%86%b3%e5%8a%9e%e6%b3%95
title: nginx配置SSL返回ssl_error_rx_record_too_long的解决办法
wordpress_id: 1215
categories:
- 网站
tags:
- google
- ipv6
- Nginx
- ssl
- vps
---

今天在弄webproxy的SSL

一开始把V4的ssl配好了，接下来配V6的

搞出来之后打不开，Firefox返回ssl_error_rx_record_too_long<!-- more -->

一直以为是nginx的问题

google了一下也没发现什么解决办法

试着把地址映射到V4，发现问题依旧，说明不是IPV6的缘故，仔细看了一下配置文件，发现忘了加ssl on;囧……

另外贴几个教程

nginx配置ssl [http://www.vpser.net/build/nginx-lnmp-ipv6.html](http://www.vpser.net/build/nginx-lnmp-ipv6.html)（其实地址那部分可以用listen [::]:80 ipv6only=on;来代替，而且这样可以同时监听多个IPV6地址）

startssl申请及nginx下面https的配置：[http://webparty.blog.163.com/blog/static/16868491820100317333380/](http://webparty.blog.163.com/blog/static/16868491820100317333380/)


