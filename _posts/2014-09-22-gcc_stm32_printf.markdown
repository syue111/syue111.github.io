---
author: dashashi
comments: true
date: 2014-09-22 03:46:16+00:00
layout: post
slug: '%e8%a7%a3%e5%86%b3%e7%94%a8gcc%e7%bc%96%e8%af%91stm32%e6%b5%ae%e7%82%b9%e6%95%b0%e7%9a%84printf%e4%b8%8d%e6%ad%a3%e5%b8%b8%e7%9a%84%e9%97%ae%e9%a2%98'
title: 解决用GCC编译STM32浮点数的printf不正常的问题
wordpress_id: 1509
categories:
- STM32
---

用我

{{site.url}}{% post_url 2014-02-19-eclipse_for_stm32_by_gcc %}

和
{{site.url}}{% post_url 2014-03-17-stm32_printf_via_swo %}

里面的方法配置的STM32模版在printf与sprintf浮点数时有问题，原因在<!-- more -->https://answers.launchpad.net/gcc-arm-embedded/+question/245658 里提到了，是由于使用的Startup文件跟Linkscript有问题- -|

换用mBed中提供的startup文件跟链接文件就好了，mBed是ARM公司搞的类似Arduino平台，不过芯片都是Cortex-M的，当然也有STM32的。

mBed的链接为https://github.com/mbedmicro/mbed/tree/master/libraries/mbed/targets/cmsis/TARGET_STM/

TARGET_STM32F407VG/TOOLCHAIN_GCC_ARM

可以clone下来，也可以直接把需要的两个文件下载下来。下载下来后直接把内容复制到相应的文件即可。

如果重定向了printf，可能会提示一堆undefined reference，在链接文件里的最后面加上 GROUP(libnosys.a)就OK了，这句话的意思是把libnosys链接进来。
