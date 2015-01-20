---
author: dashashi
comments: true
date: 2011-08-30 03:44:37+00:00
layout: post
slug: '%e9%99%a4%e6%b3%95%e3%80%81%e4%bd%8d%e8%bf%90%e7%ae%97%e3%80%81%e7%bc%96%e8%af%91%e5%99%a8%e4%bc%98%e5%8c%96%e5%92%8c%e6%97%a0%e7%ac%a6%e5%8f%b7%e6%95%b0%e7%9a%84%e6%81%a9%e6%80%a8%e6%83%85%e4%bb%87-2'
title: 除法、位运算、编译器优化和无符号数的恩怨情仇（二）
wordpress_id: 1371
categories:
- 未分类
---

忽然发现，其实在变量名字前面加个volatile就可以防止被优化掉- -|搞得这么麻烦- -||

嗯嗯 - -刚刚测试了在MDK下面的情况，现在测试VS2010里面的情况

<!-- more -->

测试代码如下:

    
    	volatile unsigned int t;
    	t = rand();
    	t %= 64;


测试的时候采用release编译模式(/O2)

先来试试无符号数的取模

    
    	t %= 64;
    00DC100D  and         dword ptr [t],3Fh


很漂亮地优化成了一句位运算

试试除法

    
    	t /= 64;
    001A100D  shr         dword ptr [t],6


同样的

乘法也看卡

    
    	t *= 64;
    0105100D  mov         eax,dword ptr [t]
    01051010  shl         eax,6
    01051013  mov         dword ptr [t],eax


不知道为什么先把数据弄到寄存器里面位运算了又放回去- -|

乘以48

    
    0127100D  mov         eax,dword ptr [t]
    01271010  lea         eax,[eax+eax*2]
    01271013  shl         eax,4
    01271016  mov         dword ptr [t],eax


乘以170

    
    	t *= 170;
    00DC100D  mov         eax,dword ptr [t]
    00DC1010  imul        eax,eax,0AAh
    00DC1016  mov         dword ptr [t],eax


从测试结果来看，跟mdk基本没区别

再试试有符号数的情况

    
    	t %= 64;
    0098100D  mov         eax,dword ptr [t]
    00981010  and         eax,8000003Fh
    00981015  jns         wmain+1Ch (98101Ch)
    00981017  dec         eax
    00981018  or          eax,0FFFFFFC0h
    0098101B  inc         eax
    0098101C  mov         dword ptr [t],eax



    
    	t /= 64;
    0001100D  mov         eax,dword ptr [t]
    00011010  cdq
    00011011  and         edx,3Fh
    00011014  add         eax,edx
    00011016  sar         eax,6
    00011019  mov         dword ptr [t],eax



    
    	t *= 64;
    000B100D  mov         eax,dword ptr [t]
    000B1010  shl         eax,6
    000B1013  mov         dword ptr [t],eax



    
    	t *= 48;
    00FF100D  mov         eax,dword ptr [t]
    00FF1010  lea         eax,[eax+eax*2]
    00FF1013  shl         eax,4
    00FF1016  mov         dword ptr [t],eax



    
    	t *= 170;
    011A100D  mov         eax,dword ptr [t]
    011A1010  imul        eax,eax,0AAh
    011A1016  mov         dword ptr [t],eax


测试结果基本上跟mdk没什么区别，

结论也是一样的：只需要无符号数的时候就别用有符号数，给编译器留下优化的空间。


