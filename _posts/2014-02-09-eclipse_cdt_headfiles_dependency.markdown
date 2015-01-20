---
author: dashashi
comments: true
date: 2014-02-09 14:41:42+00:00
layout: post
slug: eclipsecdt%e4%b8%ad%e9%81%87%e5%88%b0%e5%a4%b4%e6%96%87%e4%bb%b6%e4%be%9d%e8%b5%96%e6%97%a0%e6%95%88%e7%9a%84%e8%a7%a3%e5%86%b3%e5%8a%9e%e6%b3%95
title: Eclipse/CDT中遇到头文件依赖无效的解决办法
wordpress_id: 1448
categories:
- 写程序
---

在Eclipse/CDT中写C程序，有时候会遇到改了头文件重新编译，相应的源文件并没有被重新编译的情况。

一般发生这种情况的工程项目都是从其他项目复制的，解决办法如下：

1、在项目属性对话框中找到C/C++ Build选项

2、选中后在右边选择第三个Refresh Policy选项卡

3、选中原有的Resources，按Delete删除

4、点Add Resource，将项目根目录添加

5、收工

![1]({{site.url}}/wp_uploads/2014/02/1-300x208.png)
