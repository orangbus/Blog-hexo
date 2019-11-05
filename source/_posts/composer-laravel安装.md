---
title: composer-laravel安装
date: 2017-10-11 16:05:44
tags: laravel
categories: 
- laravel
keyword: composer安装，laravel安装教程,3分钟在Linux（Deepin）下跑起Laravel
---

近期在闲暇之余开启了 Laravel 学习之路，特地去感受那份**优雅**。正所谓`万丈高楼平地起`，环境的搭建乃是学习任何一个框架甚至任何一门语言最重要的起点。本文更多的目的为了做一些学习中的相关记录，并且选择**自认为**的最佳实践。本人主力工作环境是 Windows ，在裸 Deepin 下走一遍流程能熟悉相关依赖，更清晰认识环境搭建本质。

```
环境搭建大致有如下步骤：
1. 安装 php
    sudo apt-get install php
2. 安装 composer
    * php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');"
    * php composer-setup.php
    * php -r "unlink('composer-setup.php');"
    * sudo mv composer.phar /usr/local/bin/composer
3. 设置 composer 镜像
    * composer config -g repo.packagist composer https://packagist.phpcomposer.com
4. 安装 php 扩展
    * sudo apt-get install php-mbstring php-mcrypt php-xml php-zip
5. 使用 composer 安装 Laravel
    * cd ~/Documents/
    * composer create-project --prefer-dist laravel/laravel laravel-study
6. 运行 Laravel
    * cd laravel-study/
    * php artisan serve
7. 浏览器访问
    * 浏览器打开 http://127.0.0.1:8000
    * 看到 Laravel 初始界面

```

<!--more-->这样就成功搭建了最小依赖环境，并且没有使用第三方 web 服务器，其中 composer 使用了中国镜像，成功避开了墙的威力（初次使用 composer 那种难受记忆犹新）。其中步骤中第一步和第三步是可以并入第一步执行安装操作的，之所以分开是为了区分依赖关系。因为安装了 php 才能使用 php 进行 composer 安装，而执行第三步那三个扩展安装是为了 composer 安装 Laravel 时能一步到底，中途不会因为缺少 php 扩展而宣告失败。最后进入自定义名称的文件夹，跑起 artisan 命令，默认就是 8000 端口了，当然这一步也可以用 `php -S 0.0.0.0:8080 -t public/` 指定端口去跑。

composer中文网：http://www.phpcomposer.com/

laravel-5.4中文开发手册：https://www.kancloud.cn/feimo87/laravel5-4

原文-转载：https://segmentfault.com/a/1190000010454690?share_user=1030000008192090