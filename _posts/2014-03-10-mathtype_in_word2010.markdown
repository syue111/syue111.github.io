---
author: dashashi
comments: true
date: 2014-03-10 03:26:27+00:00
layout: post
slug: word2010%e8%a3%85mathtype%e5%90%8e%e8%bf%90%e8%a1%8c%e6%8a%a5%e9%94%99%e7%9a%84%e8%a7%a3%e5%86%b3%e5%8a%9e%e6%b3%95
title: Word2010装Mathtype后运行报错的解决办法
wordpress_id: 1481
categories:
- 用电脑
---

错误提示为[The MathType DLL cannot be found.Please reinstall mathtype.](http://blog.csdn.net/shih010/article/details/5063479)

放狗搜了一下，大部分都说把C:\Program Files (x86)\Microsoft Office\Office14\STARTUP或者C:\Users\用户名\AppData\Roaming\Microsoft\Word\STARTUP里面的dotm文件删了，或者重装Mathtype啥的。

首先重装肯定是没效果的，如果有效果估计你也不会看到这里- -

至于删除Startup里面的东西，结果就是mathtype用不了了- -

<!-- more -->

其实解决办法很简单，在Mathtype的安装目录下面C:\Program Files (x86)\MathType\MathPage，里面有32位跟64位两个不同版本的Mathpage.wll，根据你装的office版本，把相应的wll文件复制到C:\Program Files (x86)\Microsoft Office\Office14\STARTUP跟C:\Users\用户名\AppData\Roaming\Microsoft\Word\STARTUP中的一个文件夹下面。具体复制到哪里就要看dotm文件是在哪里了。正常情况下两个Startup文件夹应该是一个空的，一个有东西，我的电脑上是在C:\Program Files (x86)\Microsoft Office\Office14\STARTUP里面，实在不行可以两个都试试。


