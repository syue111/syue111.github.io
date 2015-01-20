---
author: dashashi
comments: true
date: 2011-08-17 13:45:30+00:00
layout: post
slug: '%e4%b8%80%e4%b8%aa%e5%bc%95%e5%8f%91%e7%9a%84%e8%a1%80%e6%a1%88'
title: 一个;引发的血案
wordpress_id: 1319
categories:
- STM32
tags:
- I2C
- STM32
---

话说今天用STM32自带的I2C来读写AT24C02，历经各种坎坷- -|最后能够成功写入，但是读取不成功，怀疑是连续读取的问题，于是照着官方例程改写成只读取一个字节，成功了，然后回头，把之前连续读取8个字节的程序段改成只读取一个字节


程序段如下




<!-- more -->






    
    for(i = 0; i<1; ++i){
    	if(i == 0){
    		I2C_AcknowledgeConfig(I2C2, DISABLE);
    		I2C_GenerateSTOP(I2C2, ENABLE);
    	}
    	while(!I2C_CheckEvent(I2C2, I2C_EVENT_MASTER_BYTE_RECEIVED))
    	buf[i] = I2C_ReceiveData(I2C2);
    }
    //I2C_AcknowledgeConfig(I2C2, DISABLE);
    //I2C_GenerateSTOP(I2C2, ENABLE);
    //while(!I2C_CheckEvent(I2C2, I2C_EVENT_MASTER_BYTE_RECEIVED));
    //buf[0] = I2C_ReceiveData(I2C2);







加了注释的是只读取一位的，不加注释的是连续读取的，但是改了循环条件，实际上也只读取一位了，乍看上去基本上没有啥区别，但是就是无法读取，事实上我甚至改成






    
    i = 0;
    //for(i = 0; i<1; ++i){
    //	if(i == 0){
    		I2C_AcknowledgeConfig(I2C2, DISABLE);
    		I2C_GenerateSTOP(I2C2, ENABLE);
    //	}
    	while(!I2C_CheckEvent(I2C2, I2C_EVENT_MASTER_BYTE_RECEIVED))
    	buf[i] = I2C_ReceiveData(I2C2);
    //}
    //I2C_AcknowledgeConfig(I2C2, DISABLE);
    //I2C_GenerateSTOP(I2C2, ENABLE);
    //while(!I2C_CheckEvent(I2C2, I2C_EVENT_MASTER_BYTE_RECEIVED));
    //buf[0] = I2C_ReceiveData(I2C2);







还是没有看出问题在哪里




好吧，看过标题的话你一定看出来了，






    
    while(!I2C_CheckEvent(I2C2, I2C_EVENT_MASTER_BYTE_RECEIVED))
    buf[i] = I2C_ReceiveData(I2C2);







while后面忘了加;




其实照理说，即使忘了加分号，起作用的也是最后一次赋值，按说应该也能得到正确结果，不过悲剧的是I2C_ReceiveData()这个函数，函数内部实现很简单，直接读取I2C_DR然后返回，而关于I2C_DR在数据手册中有这么一段：

    
    –Receiver mode: Received byte is copied into DR (RxNE=1). A continuous transmit stream
    can be maintained if DR is read before the next data byte is received (RxNE=1).







这才是悲剧之源。




至于正确的代码，只要把;加上就好了- -||




分析一下这次的问题，表面上看是忘了打一个;，而又由于缩进的存在而把自己骗了，不过仔细一想，本身






    
    while(!I2C_CheckEvent(I2C2, I2C_EVENT_MASTER_BYTE_RECEIVED));







这种写法就不规范，至少应该写成






    
    while(!I2C_CheckEvent(I2C2, I2C_EVENT_MASTER_BYTE_RECEIVED)){
    }







而最好的写法其实是






    
    t = TIMEOUT;
    while(I2C_GetFlagStatus(sEE_I2C, I2C_FLAG_RXNE) == RESET)
    {
    	if((t--) <= 0)
    	输出错误信息;
    }


好吧，真的要说的话，用t作为变量名貌似也不太好- -|






嗯嗯- -写下来做个标记- -下次别再犯这种错误- -||
