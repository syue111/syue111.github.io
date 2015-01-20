---
author: dashashi
comments: true
date: 2013-01-14 09:11:04+00:00
layout: post
slug: openshift%e7%94%a8ssh%e7%99%bb%e9%99%86%e5%90%8e%e7%9a%84%e7%9b%ae%e5%bd%95%e7%bb%93%e6%9e%84
title: openshift用ssh登陆后的目录结构
wordpress_id: 1435
categories:
- 网站
---

之前把博客挂在sae上面- -不过速度还真是慢的可以- -

最近发现openshift好像还不错，还支持ssh，所以决定弄过去

折腾了一天弄openshift，ssh登陆功能比较强大

不过ssh登上去之后权限比较少，能访问的目录没几个<!-- more -->

首先是/tmp/，临时文件目录，这个目录里面的文件10天不访问就会被自动删除

然后就是home目录~，具体路径是/var/lib/openshift//var/lib/openshift/3b67be8bfb3c4af9ac5348b6aeaabb91 ，其中3b67be8bfb3c4af9ac5348b6aeaabb91是一串随机的字符串，每个不同的应用都不一样

除了这个目录之外其他目录的权限都很少，要么只能读，要么连读都没法- -

在home目录下面，有app-root  cron-1.4  git  mysql-5.1  php-5.3这几个目录，从名字就很容易看出来每个目录是干嘛的，在cron,mysql跟php目录下面，主要是相应的配置文件，git下面是应用对应的git库，另外home目录本身我们是没有权限对齐进行写入的，也就是没办法添加或删除这个目录下的文件

然后app-root下面就是应用的数据了，里面有data，repo跟runtime三个文件夹，data里面的数据是永久保存的，repo是个符号链接，其正式路径是~/runtime/repo，里面的数据就是git push上去的东西，如果wordpress之类的东西把附件放在这个目录的子目录下面，push新代码之后数据就会丢失！runtime下面有repo文件夹，以及前面data文件夹的符号链接，和一个.state文件，里面是当前应用的状态

由于repo里面的东西，每次push之后就会丢失，所以保险起见，我把wp-contest文件夹整个移动到data下面，再在repo里面弄了一个符号链接，这样每次重新push就不会出现这个问题

在~下面还有隐藏文件夹.env，里面都是环境变量配置的一些文件，都是只读的，有个脚本USER_VARS，它会

从.uservars的脚本里面读相应的环境变量

但是我试了一下，.uservars打不开,官方在https://bugzilla.redhat.com/show_bug.cgi?id=883317 里面说暂时还没开放这个功能- -

网上很多利用ssh管理openshift空间的帖子中，大都直接把数据放在repo里面，这样万一不小心用git push了一次，数据就全部丢失了，随意最好还是把有用的数据放在data里面之后做符号链接，为了方便，可以再git的脚本里面添加相应的代码

如果嫌麻烦，为了保险起见，最好想办法把git的功能禁用掉- -我在网站里面没找到相应的选项，干脆就把git文件夹下面的东西都删了- -这样再对空间进行git操作就会报错- -结果就是没法用git上传代码了- -
