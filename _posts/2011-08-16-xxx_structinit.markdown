---
author: dashashi
comments: true
date: 2011-08-16 09:20:45+00:00
layout: post
slug: '%e5%85%b3%e4%ba%8estm%e5%9b%ba%e4%bb%b6%e5%ba%93%e4%b8%adxxx_structureinit%e5%87%bd%e6%95%b0'
title: 关于STM固件库中XXX_StructInit()函数
wordpress_id: 1312
categories:
- STM32
tags:
- STM32 固件库 初始化
---

话说今天调一个东西，用到了DAC，明明能用的程序，莫名其妙的不能用，最后发现一个调用一个完全不相干的函数DisableOutput会使DAC失效，而不调用就能够正常使用。仔细看了一下源码，确实是完全不相干的。<!-- more -->

调试中发现DAC_CR寄存器的TEN2位在两种情况下是不一样的，估计问题就出在这一位上面，但是调用这个完全不相干的函数为什么会发生这种情况？

继续单步跟踪，终于发现了问题，在DAC_Init的时候，DAC_InitStructure有一个成员DAC_LFSRUnmask_TriangleAmplitude，由于我没有用到，所以没有去改动，

与此同时，我又没有调用DAC_StructInit，因此这个变量的值是个不定的值

最后，在DAC_Init的时候，有这么一句

    
      tmpreg2 = (DAC_InitStruct->DAC_Trigger | DAC_InitStruct->DAC_WaveGeneration |
                 DAC_InitStruct->DAC_LFSRUnmask_TriangleAmplitude | DAC_InitStruct->DAC_OutputBuffer);



    
    它把不确定的<a>DAC_LFSRUnmask_TriangleAmplitude给或进去了- -|于是乎- -|各种悲剧- -|</a>



    
    为了防止出现这种情况，要么在一开始调用一下DAC_StructInit,要么所有成员变量都赋一下值，感觉还是第一种方法来的比较保险，彻底。



    
    还好这次问题不是太大就发现了这个问题- -要是以后大程序里面有这么个BUG就坑爹了- -
