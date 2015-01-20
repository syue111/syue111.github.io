---
author: dashashi
comments: true
date: 2011-07-18 14:23:24+00:00
layout: post
slug: '%e8%a2%abc%e7%9a%84%e4%bc%98%e5%85%88%e7%ba%a7%e5%9d%91%e4%ba%86'
title: 被C的优先级坑了
wordpress_id: 1276
categories:
- 写程序
tags:
- 优先级
- 位运算
---

话说今天弄12864的液晶屏，判断屏幕是否busy的时候用到了这么一句

return (status & 0x80 == 0);

看起来挺不错的……但是由于0x80 == 0的优先级比较高，返回值永远都是0，于是乎……悲剧了……

究其原因，感觉还是位运算用的少导致的- -|
