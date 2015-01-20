---
author: dashashi
comments: true
date: 2012-01-25 16:01:28+00:00
layout: post
slug: centos%e4%b8%8b%e9%9d%a2%e8%a3%85nginx%e8%8e%ab%e5%90%8d%e5%85%b6%e5%a6%99502%e4%ba%86
title: CentOS下面装nginx莫名其妙502了
wordpress_id: 1417
categories:
- 未分类
---

我vps上面是CentOS6+nginx+spawn-fcgi+php的架构

今天重启了一下vps，莫名其妙的vps上很多网站都502了，除了几个wp的博客都是正常的，phpmyadmin，phpwind都不行，连探针都打不开，排错排了半天，想起来之前装了一下eAccelerator玩，感觉可能是eAccelerator的问题，于是乎去/etc/php.d/eaccelerator.ini里面把eAccelerator禁用了- -果断就好了- -

改了一下eaccelerator里面的配置，发现怎么弄，只要一启用就要502，忽然发现，虽然在配置文件里面禁用了eAccelerator，而且压根没装zend，但是在phpinfo跟探针里面都显示eAccelerator跟zend是存在的，于是猜想这个php是集成了这两个东西的，然后我自己又装了一个- -于是就出问题了- -|||
