---
author: dashashi
comments: true
date: 2014-02-19 10:23:01+00:00
layout: post
slug: stm32%e7%9a%84eclipse%e5%bc%80%e5%8f%91%e7%8e%af%e5%a2%83%e9%85%8d%e7%bd%ae%ef%bc%88%e9%87%87%e7%94%a8gcc%e7%bc%96%e8%af%91%e5%99%a8%ef%bc%89
title: STM32的Eclipse开发环境配置（采用GCC编译器）
wordpress_id: 1453
categories:
- STM32
---

STM32比较常用的开发环境是MDK跟IAR，但是这两个都是商业软件，且自带的编辑器功能实在是弱

Eclipse作为开源界最常用的IDE，不仅功能强大，扩展性强大，而且可以免费使用。事实上市面上有很多商业或开源的STM32开发平台都是基于Eclipse，比如收费的TrueSTUDIO和免费的CooCox，连IAR都推出了Eclipse插件。不过既然用到了Eclipse，最佳拍档当然是同样开源的GCC。本文就将介绍利用Eclipse+GCC开发STM32的基本方法，本文不涉及基于OpenOCD的GDB调试。<!-- more -->

参考本文的方法，采用VIM等编辑器配合Makefile开发STM32程序也是可以的。下面进入主题。

本文以STM32F4Discovery开发板为例，下载器为板载的ST-Link，STM32F1跟F0等型号大同小异。

需要用到的软件包括

1、Java Runtime Environment（JRE），由于Eclipse是Java编写的，没JRE的话Eclipse跑不起来。下载地址：http://www.java.com/en/download/manual.jsp 这个安装比较简单，一路下一步就好了。

2、最新版的Eclipse，下载地址http://www.eclipse.org/downloads/ ，推荐下载Eclipse IDE for C/C++ Developers版本，如果是其他版本还需要另外安装CDT等插件。下载下来的是绿色版，解压就OK了。运行的话双击里面的eclipse.exe就可以。

3、GCC for ARM。以前比较常用的是Code Sourcery公司编译的GCC。现在在launchpad.net网站上有一个ARM公司员工维护的版本，对浮点数等支持比较不错，推荐使用。下载地址：https://launchpad.net/gcc-arm-embedded ，推荐下载安装版（win32.exe结尾的那个)。安装也是一路下一步，安装完后最后有一项Add path to environment variable. 一定要勾上。由于这个GCC里面的所有命令都是带arm-none-eabi-前缀的，即使加到PATH环境变量也不会有什么冲突。如果顺利的话，安装完后在CMD命令提示符中输入arm-none-eabi-gcc -v，应该会显示GCC的版本信息。如果提示找不到命令，说明GCC的安装有问题。

[![]({{site.url}}/wp_uploads/2014/02/20140219161932-300x233.png)]({{site.url}}/wp_uploads/2014/02/20140219161932.png)

4、MinGW，这是一套用于编译Win32的编译工具集，里面附带了一个叫MSYS的Linux Shell工具包，这里只需要用到里面的Make工具。熟悉Linux Shell的话会发现在Windows下用MinGW做一些批量工作相当方便。下载地址：http://sourceforge.net/projects/mingw/files/Installer/ ，下载[mingw-get-setup.exe](http://sourceforge.net/projects/mingw/files/Installer/mingw-get-setup.exe/download)，双击运行后点Install，安装好后MinGW自带了一个图形安装器，如下图：

[![3]({{site.url}}/wp_uploads/2014/02/3-300x205.png)]({{site.url}}/wp_uploads/2014/02/3.png)

其中最下面一个msys-base必选，其他的如果不知道是干嘛的就没必要选了。勾选后在Install菜单中选择Apply Changes开始安装。安装好后需要自行添加路径到PATH环境变量。方法是在计算机图标上右击属性，在高级系统设置菜单中点击环境变量按钮，在用户变量或者系统变量中找到PATH，把“C:\MinGW\bin;C:\MinGW\msys\1.0\bin;”添加在最后面。如果添加在用户环境变量里就只对当前登录的用户有效，系统变量则对所有用户有效。我是安装在C盘的，装在其他分区的记得做相应修改。完了之后重新开一个CMD窗口，输入make -v，如果提示找不到命令说明安装有问题，如果正常显示make的版本信息，说明安装成功了。

[![4]({{site.url}}/wp_uploads/2014/02/4-260x300.png)]({{site.url}}/wp_uploads/2014/02/4.png)

5、STM32官方固件库，F4的下载地址是http://www.st.com/web/en/catalog/tools/PF257901 。其他芯片在官网也可以找到。下载后解压备用。

6、ST-Link Utility，ST-Link的工具包，用于下载程序。下载地址：http://www.st.com/web/en/catalog/tools/PF258168

7、mBed的源码，其实只用到两个文件，下载地址：https://github.com/mbedmicro/mbed/tree/master/libraries/mbed/targets/cmsis/TARGET_STM/TARGET_STM32F407VG/TOOLCHAIN_GCC_ARM

在Eclipse下面开发其实也还是有很多种方法，可以采用用Eclipse管理Makefile的方法，也可以采用CDT自带的Cross GCC插件，另外也有第三方写了一个Cross ARM GCC插件，这个插件将GCC的一些编译命令图形化了，如果对GCC不熟可以选用这个插件。本文选用Cross GCC插件，自己配置GCC 编译命令的方法。

下面创建一个模版工程，以后的工程可以直接从这个工程复制，减少工作。

在Eclipse里面，创建一个基于Cross GCC的C工程，输入项目名后点Next；下面选择要创建的配置有序需要改的东西比较多，可以只勾选Debug，以后需要Release的时候再从配置好的Debug复制。之后需要填写编译器前缀和路径，输入arm-none-eabi-，别忘了最后的小横线。路径的话因为前面已经设置了环境变量了，可以不用选。点Finish创建工程。

[![5]({{site.url}}/wp_uploads/2014/02/5-252x300.png)]({{site.url}}/wp_uploads/2014/02/5.png)[![6]({{site.url}}/wp_uploads/2014/02/6-252x300.png)]({{site.url}}/wp_uploads/2014/02/6.png)[![7]({{site.url}}/wp_uploads/2014/02/7-252x300.png)]({{site.url}}/wp_uploads/2014/02/7.png)

在源文件下面创建三个目录，src，inc，lib分别用于存放源文件，头文件和库文件。在lib下面建立一个stdperiph文件夹，把固件库STM32F4xx_DSP_StdPeriph_Lib_V1.3.0\Libraries\STM32F4xx_StdPeriph_Driver下面的src和inc文件夹复制过去。根据芯片的不同，部分源文件是多余的，比如F407就需要删除stm32f4xx_fmc.c。就需要将再创建一个cmsis文件夹，在cmsis创建一个inc子文件夹，把STM32F4xx_DSP_StdPeriph_Lib_V1.3.0\Libraries\CMSIS\Include里面的头文件复制过去。这里的头文件有一些可能用不到，比如F4就用不到cm0跟cm3的一些头文件，确定用不到的头文件可以删除。另外在STM32F4xx_DSP_StdPeriph_Lib_V1.3.0\Libraries\CMSIS\Device\ST\STM32F4xx\Include里面还有几个头文件，也复制到cmsis下面的inc里面。

再在cmsis下面创建一个src文件夹，<del>把STM32F4xx_DSP_StdPeriph_Lib_V1.3.0\Libraries\CMSIS\Device\ST\STM32F4xx\Source\Templates\TrueSTUDIO下面相应的startup文件复制过去，F4Discovery的芯片是STM32F407系列的，因此startup文件是startup_stm32f40_41xxx.s。</del>把mBed里的startup_STM32F40x.s复制过去。这是F40x系列的，其他系列的芯片去上层目录找相应的文件。复制好后千万记得把扩展名小写的s改成大写的S。eclipse默认只编译大写S结尾的汇编文件。最后在src文件夹中还需要一个时钟配置文件system_stm32f4xx.c，可以从STM32F4xx_DSP_StdPeriph_Lib_V1.3.0\Project\STM32F4xx_StdPeriph_Templates里面复制，这里面这个文件对应的晶振是25M的，如果不是25M的晶振需要相应的进行修改。当然也可以利用http://www.st.com/web/en/catalog/tools/PF257927里面的工具生成system_stm32f4xx.c文件。

之后是根目录中的src跟inc目录。把从STM32F4xx_DSP_StdPeriph_Lib_V1.3.0\Project\STM32F4xx_StdPeriph_Templates中把stm32f4xx_it.h和stm32f4xx_conf.h复制到根目录下的inc目录，然后把stm32f4xx_it.c复制到src目录中，再把stm32f4xx_it.c开头的#include "main.h" 跟void SysTick_Handler(void)函数里的TimingDelay_Decrement();删掉。之后顺便创建一个main.c。

<del>最后从\STM32F4xx_DSP_StdPeriph_Lib_V1.3.0\Project\STM32F4xx_StdPeriph_Templates\TrueSTUDIO\STM32F40_41xxx复制一个链接脚本文件STM32F407IG_FLASH.ld到根目录，并改名为stm32f407vg_flash.ld。这个链接文件是针对F407IG的，如果是其他型号的芯片的话，根据芯片的RAM跟ROM配置修改相应的值，除此之外栈起始地址_estack也得改。另外在最下面libgcc.a ( * )后面加上libnosys.a ( * )，这个是在重定向printf的时候需要用到的，这里不加也不会出错。</del>

最后把mBed里的STM32F407.ld复制到根目录，并改名为stm32f407vg_flash.ld。这是F40x系列的，其他系列的芯片去上层目录找相应的文件。这个链接文件是针对512K ROM，96K RAM的型号的，根据芯片的不同，改成相应的大小。STM32F4芯片的RAM地址是从0x20000000开始的芯片RAM的实际地址是从0x20000000开始的，<del>这个头文件保留了0x000~0x194，也就是404个字节的空间，具体原因我表示不知道……改的时候记得LENGTH是实际大小减去0x194就OK了。</del>但是在mbed中实现了一个动态设置中断向量表的功能 https://github.com/mbedmicro/mbed/blob/9b7d23d47153c298a6d24de9a415202705889d11/libraries/mbed/targets/cmsis/TARGET_STM/TARGET_STM32F4XX/cmsis_nvic.c ，因此保留了从0x20000000开始的404字节空间，由于我们没有用到这个功能，因此这一行可以改成
    RAM (rwx) : ORIGIN = 0x20000000, LENGTH = 128K
其中128K是内存大小,根据芯片不同数值也不一样


文件创建工作算是完成了，完成后的文件结构该是下面这样的：

[![8]({{site.url}}/wp_uploads/2014/02/8-238x300.png)]({{site.url}}/wp_uploads/2014/02/8.png)



接下来需要在stm32f4xx.h中做一些配置。首先是有一行宏定义#define HSE_VALUE    ((uint32_t)25000000)。如果外部晶振的频率不是25M，也需要进行对于的修改，比如8M就改成8000000。需要说明的是，如果晶振不同的话要改的地方不止这一个，在stm32f4xx_system.c中还要修改相应的PLL选项，不过ST提供了一个比较好用的xls文件可以自动生成这个文件，可以去官网找一下。

之后在

{% highlight c %}
	#if !defined (STM32F40_41xxx) && !defined (STM32F427_437xx) && !defined (STM32F429_439xx) && !defined (STM32F401xx)
{% endhighlight %}
这一句前面，注意是前面，添加两行宏定义
{% highlight c %}
#define STM32F40_41xxx
#define USE_STDPERIPH_DRIVER
{% endhighlight %}

前一行是定义设备，后一行是使用固件库的开关。有的人习惯把这两个定义放在IDE设置里，优点是不同型号的芯片迁移比较方便，可以用一个工程管理不同的芯片，缺点是项目跟IDE联系比较紧密，如果换其他IDE或者Makefile等方法迁移性就比较差，因此我把这两行直接放在头文件里。

之后需要对编译选项进行一些设置，右击工程，在菜单中选择Properties。选择C/C++ Build中的Setting。选中后在右边的Tool Settings里面可以设置编译选项。第一个Cross Settings就是编译器前缀与路径，创建工程的时候已经设置好了。接下来在Cross GCC Compiler中的设置保持默认，Preprocessor中关于预处理的一些选项，保持默认。Symbols可以添加一些宏定义，比较常用的可以在Debug配置里面添加一个_DEBUG标记，在Release配置中添加一个_RELEASE标记。这样的话，代码里面就可以用#ifdef _DEBUG进行条件编译。之后是include，头文件定义。需要添加的目录是inc, lib/stdperiph/inc与lib/cmsis/inc三个目录。添加的时候通过点击Workspace按钮添加工程相对路径，而不是添加绝对路径，这样工程路径改变后照样能正常编译。Optimizatzation优化选项，根据需要选择O0-O3,数字越大优化越多，Os表示对生成代码大小进行优化，一般选O3。然后是Debug调试选项，数字越大调试信息越多。Warning编译警告选项，推荐勾选-Wall 和 -Wextra，这样会提示所有警告，一个比较好的习惯是消灭所有警告，因为每一个警告都表示一个隐患。Miscellaneous杂项，这个需要自己填写与芯片相关的几个参数，STM32F4是
	-c -mcpu=cortex-m4 -mthumb -mfloat-abi=hard -mfpu=fpv4-sp-d16
其中-c参数表示只编译不链接，因为我们有很多源文件，全部编译好再统一链接。然后-mcpu=cortex-m4表示cortex-m4核心，如果是F1,F2,F3系列是cortex-m3，如果是F0系列应该是cortex-m0。-mthumb表示采用thumb指令集，cortex-m系列的都是只支持thumb指令集的。最后两项-mfloat-abi=hard -mfpu=fpv4-sp-d16，分别表示采用硬浮点数指令，浮点数协处理器型号为fpv4-sp-d16。只有F4才需要也只有F4支持这两个选项，F0跟F1都是不到浮点数模块的，因此不需要填。填了反而会出错。

完了是链接选项，Cross GCC Linker保留默认设置。General里面的设置都不选。Libraries第三方库，我们这里没用到第三方库，所以留空，像STM32F4是自带了编译好的DSP库的，需要用到的话要在这里进行相应的设置。Miscellaneous杂项，填写
	-T"${workspace_loc:/${ProjName}/stm32f407vg_flash.ld}" -mcpu=cortex-m4 -mthumb -mfloat-abi=hard -mfpu=fpv4-sp-d16 -Xlinker --gc-sections
-T"${workspace_loc:/${ProjName}/stm32f407vg_flash.ld}"是指定前面生成的链接文件，接下来的几项跟编译选项是一样的。这几项必须与编译选项保持一致，因为链接器需要根据这个选项选择对应的库文件。最后一项-Xlinker --gc-sections表示移除未使用的块，意思是如果在源文件里面包括一些函数，从未被调用也不可能被调用到，则链接的时候把他们删除以节约空间。最后Shared Library Settings共享库设置，保持默认就可以。最下面还有个汇编编译器设置，保持默认即可。

在Build Artifact里面的Artifact Type选Executable，Artifact name输入${ProjName}，Artifact extension中填入elf。gcc默认只能生成elf格式的结果，即使填上hex，生成的文件也是elf文件，只是扩展名是hex而已。由于下载器只认hex文件，还需要把elf文件转换为hex。在Build Steps中有一个Post-build steps，意思是在编译完成后运行的命令，在它的Command里面添加如下命令：
	arm-none-eabi-objcopy  "${ProjName}.elf" -O ihex "${ProjName}.hex"
利用objcopy工具生成hex文件。

保存设置后编写源文件main.c，编写如下代码：

{% highlight cpp %}
#include "stm32f4xx.h"
int main() {
	GPIO_InitTypeDef GPIO_InitStructure;
	/* Enable the GPIO_LED Clock */
	RCC_AHB1PeriphClockCmd(RCC_AHB1Periph_GPIOD, ENABLE);
	/* Configure the GPIO_LED pin */
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_12;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_OUT;
	GPIO_InitStructure.GPIO_OType = GPIO_OType_PP;
	GPIO_InitStructure.GPIO_PuPd = GPIO_PuPd_UP;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOD, &GPIO_InitStructure);
	while (1) {
		volatile int i = 10000000;
		while (i--)
		;
		i = 10000000;
		GPIO_SetBits(GPIOD, GPIO_Pin_12);
		while(i--)
		;
		GPIO_ResetBits(GPIOD, GPIO_Pin_12);
	}
}
void assert_failed(uint8_t* file, uint32_t line)
{
	while(1){
	}
}
{% endhighlight %}

这个代码是用于测试的，比较简单采用减变量实现延时。实际软件中最好不要用这种方法。软件下面的assert_failed函数是固件库里面对参数合法性判断用的，如果不合法就会进入这个函数，可以在里面加一些相应的代码实现错误指示灯等功能。Ctrl+B开始编译。编译好后打开ST-Link Utilities，将生成的hex下载到板子里，不出意外的话，你就能看到LED灯在闪烁了。如果出现underfined reference错误，可以试试在链接文件最后新开一行，加上
	GROUP(libnosys.a)
为了方便调试，在Eclipse可以添加一个快捷命令实现hex文件的下载，在Eclipse的Run->External Tools-> External Tools Configurations中，添加一个名为Download to board via st-link的外部工具命令，路径选择C:\Program Files (x86)\STMicroelectronics\STM32 ST-LINK Utility\ST-LINK Utility\ST-LINK_CLI.exe，即ST-Link的命令行工具，Arguments参数输入-P "${project_loc}/${config_name:${project_name}}/${project_name}.hex" -V -Rst，前面的环境变量可以找到生成的Hex文件-V表示下载后校验，-Rst表示下载后复位芯片。

OK，项目模版完成了，以后需要新建项目的时候，可以直接从这个模版复制。复制的时候最好在Eclipse里面复制而不是资源管理器里面，这样Eclipse会自动修改那些与项目名相关的设置。不过Eclipse有一个小BUG，复制项目后需要改一处设置，不然会导致修改头文件不会自动重新编译，可以参考 {{site.url}}{% post_url 2014-02-09-eclipse_cdt_headfiles_dependency %}

本文没有介绍基于OpenOCD的GCC调试方法，因为我觉得单片机的调试由于涉及到很多时序的东西，单步之类的并不是好用，最好用的调试方法是printf等能打印出调试信息的方法。之后我会介绍通过ST-Link自带的SWO口实现Printf的方法，这样就不需要添加任何的引线，直接用下载的线就可以实现调试。

最后分享一下我认为Eclipse最强大的快捷键Alt+/，功能是代码提示，有了这功能，基于固件库的编程就方便多了。
