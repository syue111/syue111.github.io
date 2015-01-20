---
author: dashashi
comments: true
date: 2011-07-11 17:11:09+00:00
layout: post
slug: wordpress%e5%88%86%e7%b1%bb%e7%9b%ae%e5%bd%95%e4%b8%a2%e5%a4%b1%ef%bc%8cphpmyadmin%e6%89%93%e4%b8%8d%e5%bc%80%ef%bc%8c%e6%8f%90%e7%a4%ba%e2%80%9c%e6%97%a0%e6%b3%95%e5%9c%a8%e5%8f%91%e7%94%9f%e9%94%99
title: Wordpress分类目录丢失，PHPMYADMIN打不开，提示“无法在发生错误时创建会话,请检查 PHP 或网站服务器日志,并正确配置 PHP 安装。”
wordpress_id: 1262
categories:
- 网站
tags:
- php
- session
- vps
- Wordpress
- 会话
---

刚刚谢了一篇日志，忽然发现分类目录显示不出来了- -|但是显然分类目录还是在的，只是没显示。

网上找了半天，说的什么内存啊的问题，我重启了VPS还是不行，没有发现什么好办法，发现一篇文章说在phpmyadmin里面修复一下数据库就好了，于是打开phpmyadmin，坑爹了，phpmyadmin都打不开了……提示“无法在发生错误时创建会话,请检查 PHP 或网站服务器日志,并正确配置 PHP 安装。”按照提示去找php的日志，坑爹的是我的php居然没开日志……于是又去找其他的解决办法，终于在一个网站里面找到一篇文章，说是session目录的权限不足导致的，打开php.ini，搜索了一下，发现session目录那一项是session.save_path=/tmp，而且被注释了……于是把注释去掉，重启PHP，还是不行，看了一下tmp目录的权限，发现没有给任意用户的写权限，干脆把tmp的权限改成777，搞定- -||
