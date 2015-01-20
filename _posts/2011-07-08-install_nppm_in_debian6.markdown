---
author: dashashi
comments: true
date: 2011-07-08 12:17:47+00:00
layout: post
slug: '1245'
title: Debian 6(Squeeze)安装 Nginx + PHP5 + PHP-FPM + MySQL
wordpress_id: 1245
categories:
- 网站
tags:
- Debian
- Nginx
- php
- php-fpm
- vps
---

由于Debian的stable更新速度太坑爹了，一直还不支持php-fpm- -
php-fpm可以编译安装，但是编译安装升级什么的不太方便，所以通过添加源的办法来安装不错- -

<!-- more -->

1:增加源，支持php5-fpm
`vi /etc/apt/sources.list`
增加以下源
`deb http://packages.dotdeb.org stable all`

修改保存后
`wget http://www.dotdeb.org/dotdeb.gpg
cat dotdeb.gpg | apt-key add -
rm dotdeb.gpg`

`apt-get update`

2:安装 MySQL 5

`apt-get install mysql-server mysql-client`

在弹出的页面输入2次密码

修改mysql配置文件，去掉innodb，这样可以节省不少内存
`vi /etc/mysql/my.cnf`

增加下面语句
`skip-innodb`

保存后，mysql重启一下就生效

3：安装Nginx+php+php5-fpm+memcache

`apt-get install php5-cgi php5-mysql php5-gd php5-imagick php5-mcrypt php5-memcache memcached php5-fpm php5-cli nginx`

安装成功后，rcconf 把多余的服务x11-common去掉

`mkdir /var/www
chown www-data:www-data /var/www`

修改memcache的端口和内存大小
`vi /etc/memcached.conf`

`vi /etc/php5/cgi/php.ini`
修改下面这句
cgi.fix_pathinfo=1

我的是512M的vps，所以修改php-fpm的配置文件
vi /etc/php5/fpm/pool.d/www.conf

`pm.max_children = 25
pm.start_servers = 4
pm.min_spare_servers = 2
pm.max_spare_servers = 10
pm.max_requests = 500`

大家也可以根据自己服务器的条件和实际负载需要进行调整

修改nginx的配置文件

`vi /etc/nginx/sites-available/default`

添加：
`location ~ \.php$ {
fastcgi_pass 127.0.0.1:9000;
fastcgi_index index.php;
fastcgi_param SCRIPT_FILENAME /var/www$fastcgi_script_name;
include fastcgi_params;
}`

保存后，重启nginx
`/etc/init.d/nginx restart`

写一个测试php页面

<?php phpinfo(); ?>

保存为php文件

如果能正常显示那页面，那就大功告成

php默认的配置文件结构感觉没lnmp里面的好用，所以我把lnmp的配置文件做了一些修改拷贝到/etc/nginx下面了
