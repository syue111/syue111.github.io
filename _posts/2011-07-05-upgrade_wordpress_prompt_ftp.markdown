---
author: dashashi
comments: true
date: 2011-07-05 01:53:59+00:00
layout: post
slug: '%e5%8d%87%e7%ba%a7wordpress%e6%97%b6%e6%8f%90%e7%a4%ba%e8%be%93%e5%85%a5ftp%e7%94%a8%e6%88%b7%e5%90%8d%e5%af%86%e7%a0%81'
title: 升级Wordpress时提示输入FTP用户名密码
wordpress_id: 1243
categories:
- 网站
tags:
- ftp
- ssh
- vps
- Wordpress
- 用户名密码
---

今天起来发现Wordpress有更新了，于是去后台点自动更新，Wordpress提示要输入FTP的用户名密码- -
但是我都是直接用的ssh操作VPS,压根就没这玩意儿- -其实是权限问题，把文件所有者修改一下就好了
chmod -R www-data /home/www
搞定- -
