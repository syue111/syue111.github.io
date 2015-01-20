---
author: dashashi
comments: true
date: 2011-08-04 17:45:50+00:00
layout: post
slug: '%e7%94%a8vim%e7%bb%93%e5%90%88keil%ef%bc%88mdk%ef%bc%89%e5%86%99stm32%e3%80%81c51%e7%ad%89%e7%a8%8b%e5%ba%8f'
title: 用VIM结合Keil（MDK）写STM32、C51等程序
wordpress_id: 1278
categories:
- 电子设计
tags:
- ARM
- C++
- C51
- keil
- MDK
- php
- STM32
- VIM
- vps
---

话说这段时间经常用Keil写些东西，写C51的时候倒还好，写STM32的程序的时候，由于我是用的STM32的官方固件库，里面的各种标识符相当长- -看起来可读性是挺不错的，

不过一个字母一个字母敲还是挺蛋疼的，于是准备用外部编辑器。好在Keil可以很方便的调用外部编辑器，
<!-- more -->
具体方法是在Tool→Customize Tools Menu下面添加一个选项，名字随便输，最好加个(&X)在最后面（X可以使随便一个字母），Command里面输入想要使用的编辑器的完整路径，Argument输入#E，Initial Folder空着，这样子在Keil里面按Alt，然后T，X就直接用设定的编辑器打开当前文件了，很方便。

调用是没问题了，不过用什么编辑器好呢？我在网上翻了一下，有不少人用Source Insight之类的编辑器，确实也挺好用的，不过这种编辑器还要创建工程，比较麻烦- -|加上最近折腾vps搞的vim挺熟练的，干脆就用vim吧- -不过肯定是要经过一番配置的- -|

先去www.vim.org下载最新版的gvim，安装的时候我选择直接安装在d:\vim\而没有装在C的program Files下面，一方面是重装系统之后懒得配置，另外一方面是路径里面包含空格的话会有点小问题，所以干脆放D盘根目录了。

vim这玩意儿上手有点麻烦，第一次用的话找到vim目录下面的vimtutor.bat，有个几十分钟的教程，之后应该会有点感觉了。
vim的配置文件是vim目录下的_vimrc,很多东西都要在这里修改。为了照顾windows用户的习惯，vim现在有一些windows风格的快捷键，不过我感觉由于跟之前的一些快捷键会冲突，反而不方便，所以注释掉_vimrc一下两行，注释的方法是在行首加一个"，如果不习惯vim的操作方法也可以保留。

source $VIMRUNTIME/vimrc_example.vim
source $VIMRUNTIME/mswin.vim

现在的vim在插入模式下按Ctrl+P能够根据上下文进行一个简单的代码提示，不过显然还不是很好用，下面加插件。
先装OmniCppComplete，下载地址：http://www.vim.org/scripts/script.php?script_id=1520
安装方法很简单，直接把解压出来的东西全部塞到d:\vim\vimfiles就可以了
然后在_vimrc里面加上 filetype plugin on
去http://ctags.sourceforge.net/下一个ctags，放到c:\windows\下面（其实只要是%PATH%里面的目录就可以），在命令提示符中找到stm32的标准库文件，我是把整个库文件包括例子都放在C:\Keil\ARM\STM32F10x_StdPeriph_Lib_V3.5.0下面了

运行如下命令生成tags文件
R:\>ctags -R --c++-kinds=+p --fields=+iaS --extra=+q c:\Keil\ARM\STM32F10x_StdPeriph_Lib_V3.5.0，我的当前目录是R:，所以生成的tags文件在R盘下面在d:\vim\vimfiles下面建一个 tags目录，把生成的tags复制过来，顺便改个名字，我改成stm32f10x_tags，然后在_vimrc加一条set tags+=d:\Vim\vimfiles\tags\stm32f10x_tags

如果需要，也可以用同样的办法扫描其他库，然后分别加进去重新打开以下vim，再试一下Ctrl+P，可以发现现在稍微智能些了，能够根据C的语法提供提示，而且在".","->"后面还会自动蹦出成员函数。
不过老师按Ctrl+P挺麻烦的，当然万能的VIM肯定有相应的插件，我选中SuperTab，这个能够用tabs来代替Ctrl+P，比较方便，下载地址:http://www.vim.org/scripts/script.php?script_id=1643

下载下来是个vba文件，用vim打开，输入:so % 就安装好了，现在按tab的时候如果前面不是空格，就会进行自动提示，不过现在的supertab还不是很好用，一方面是tab之后会自动选中一个，如果不是满意的还要删掉，另外就是现在的提示没有经过OmniCppComplete处理，提示的是所有符合前缀的tag在_vimrc里面加上

let g:SuperTabDefaultCompletionType = "<c-x><c-o><c-p>"
这样子提示就比较完美了，不过还有点小问题，主要是有时候会重复提示
写代码的时候没有自动括号显然不太方便，有这方面的插件，AutoClose http://www.vim.org/scripts/script.php?script_id=2009
下载下来放在D:\Vim\vimfiles\plugin就可以了
还有一个Taglist，这个插件也很好用，显示函数、变量列表的http://www.vim.org/scripts/script.php?script_id=273
另外还有一些缩进啊什么的也可以修改一下
set shiftwidth=4 " 设定 << 和 >> 命令移动时的宽度为 4
set tabstop=4 " 设定 tab 长度为 4
再另外还有一些杂七杂八的好用的小插件，自己根据需要添加就好了，vim官网上面可以慢慢看。
现在用这个vim来写代码应该比较舒服了。
（完）
