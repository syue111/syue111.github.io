---
author: dashashi
comments: true
date: 2014-03-17 15:19:20+00:00
layout: post
slug: stm32%e4%b8%ad%e9%87%8d%e5%ae%9a%e5%90%91printf%e5%88%b0swo%e5%8f%a3
title: STM32中重定向printf到SWO口
wordpress_id: 1488
categories:
- STM32
---

printf在命令行编程的时候是非常常用的，虽然是个老函数，但是功能强大，经久不衰- -||

51等8位单片机由于RAM比较小，栈就比较小，跑printf比较吃力，但是STM32这种32位单片机跑printf就很容易了，而作为一种调试手段，printf十分方便、直观。

比较常见的方法是把printf重定向到串口，不过这需要外接一个串口线，比较麻烦。其实STM32自带的SWO口是能够异步输出数据的，而且不需要外接什么设备，ST-LINK/J-Link等带SWO口的调试器都支持。

下面以STM32F4Discovery开发板+GCC为例说明。根据这里的方法，也可以把printf定位到其他外设。<!-- more -->

PS：IAR在编译选项里自带了printf via SWO的功能，就不需要外加设置了。http://community.silabs.com/t5/Microcontroller-How-to-Guides/SWO-printf-in-IAR/td-p/98257

首先来说说怎么把信息输出到SWO口，一句话搞定。
{% highlight cpp %}
ITM_SendChar(ch);
{% endhighlight %}
这是在core_cm4.h(如果是F1系列的那就是core_cm3.h)中定义的内联函数。不过不需要特意去include这个头文件，通过#include "stm32f4xx.h"就间接地将core_cm4.h包含进来。不过说起来，ITM这个东西其实严格来说是Cortex-M提供的一个特性，而不是STM32。

利用这个函数把信息输出到SWO口之后再打开St-Link Utility，在菜单里找到ST-LINK→Printf via SWO Viewer就会弹出一个窗口，设置System Clock为单片机内核频率，点Start就能看到输出的信息了。

接下来就是把printf函数输出的字符串重定向过去了。由于单片机的外设功能是根据需求变化的，编译器不可能确定printf需要用到的外设资源，于是乎它就干脆留了个接口，也就是_write函数，当然除了_write函数之外还有_read等其他函数，不过这里我们用不到。其声明为
{% highlight cpp %}
int _write(int fd, char* ptr, int len);
{% endhighlight %}

关于_write函数，说简单点，就是所有涉及到输出字符串的函数，比如printf和putchar()，最终都会跑到_write函数，这里fd是文件标识符，说开来就比较复杂了，这里我们用得到的就只有STDOUT_FILENO跟STDERR_FILENO，其中前一个是标准输出的文件标识符的预定义变量，后一个是错误输出的文件标识符预定义变量。第二个变量ptr是需要输出的字符串首地址，len就是输出长度。当我们调用printf函数后，C运行库会把输入变量转换为最终需要输出的字符串，然后调用_write函数，将结果输出。我们的工作就是实现一个_write函数。

新建一个_write.c文件，内容如下：

{% highlight c %}
#include <stdio.h>
#include <unistd.h>

#include "stm32f10x.h"

#ifdef _DEBUG

int _write(int fd, char* ptr, int len)
{
	if (fd == STDOUT_FILENO || fd == STDERR_FILENO)
	{
		int i = 0;
		while(i<len){
			ITM_SendChar(ptr[i++]);
		}
		return len;
	}

#endif
{% endhighlight %}

加了个#ifdef _DEBUG的效果是未加_DEBUG定义的时候就忽略下面的东西，因为这东西主要是用在调试阶段，RELEASE版本里面都用不到了，而且多少还是会影响速度。

其他东西就很简单了- -不需要多说明了吧。

直接编译能通过，但是链接会报错，提示无法找到_read之类的一堆函数。<del>在链接脚本的下面libgcc.a ( * )后面加上libnosys.a ( * )，就不会报错了。</del>在链接文件的最后加上在链接文件里的最后面新开一行加上 GROUP(libnosys.a)，或者在链接文件里的最后一个}前面加上


	/DISCARD/ :
	{
	libnosys.a ( * )
	}


具体原因涉及到Cortex-M3使用的newlib库的实现，就不具体展开了。好吧好吧，其实我也不知道。

如果想把信息定位到串口，可以直接把ITM_SendChar改成相应的串口函数，也可以利用DMA，先把数据拷贝到DMA缓冲区，让DMA自动传数据，提高响应速度。

收工。
