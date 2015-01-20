---
author: dashashi
comments: true
date: 2011-04-19 13:11:59+00:00
layout: post
slug: '1190'
title: Wordpress修改域名
wordpress_id: 1190
categories:
- 网站
tags:
- Wordpress
- 域名
---

在Wordpress里面有个域名设置神马的- -|这玩意儿还不能乱改- -|由于Wordpress里面的图片附件都是绝对引用的，所以修改之后会出现很多莫名其妙的问题，比如后台打不开，图片打不开什么的，如果真的改了的话需要在数据库里面做一些对应的修改

1、把wp-options中siteurl和home这两项改成新域名（记得加http://)

2、把wp_posts里面失效的内链(主要是图片)都修改一下，可以用下面这条SQL语句：UPDATE wp_posts SET post_content = replace(post_content, ‘http://www.old.com’, ‘http://www.new.com’);
