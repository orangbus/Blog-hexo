title: 【ownCloud】搭建一个私人网盘
author: OrangBus
tags:
  - OwnCloud
categories:
  - Web
date: 2019-11-06 14:14:00
---
前两天，360 云盘宣布将停止个人服务。一石激起千层浪，关于如何选择网盘，如何应对网盘关闭的讨论一下子又变得此起彼伏。没办法，目前的现状是，网盘很难有大的盈利空间，还面对严苛的内容审查和隐私保护，虽然于用户来说提供了便利，但于公司来说实在是一件出力不讨好的事情。<!--more-->

## 之前的网盘方案

国外的网盘我一直是三家一起用，分别是 Dropbox 存储代码和一些重要或私密文件；Google Drive 存储一些大文件和私密文件；OneDrive 存储一些电子书（同步太慢了）。国内的网盘我之前只用两家，一是坚果云，放一些个人常用的小文件，包括一些文档和软件配置文件；另一个是百毒云，放一些各处转存来的大文件、自己的照片和学习资料，一方面因其空间大，另一方面因其同步流畅。然而，百毒云前段时间把我的网盘**全面封掉**了，丢失了很多大学时的照片（其它文件要不不重要，要不有备份），申诉无果，实属无奈。

## 搭建一个只属于自己的网盘

所以我决定搭建一个只属于自己的网盘。考察了几种方案（包括买 RAID 或 NAS 等），发现已有人在这方面做了努力，提供了像 [Seafile](https://www.seafile.com/home/) 和 [ownCloud](https://owncloud.org/) 这样的产品。接下来对比了两个软件，我决定选择用 ownCloud，主要出于以下几点考虑：

- 可以设置是否加密，保证数据安全。
- ownCloud 可以用于同步日程、联系人、浏览器书签等，最重要的是**密码管理**，这对于目前有无数密码需要记的我们非常实用。ownCloud 还有个应用商店，大家可以自行发现有用的应用。
- ownCloud 提供网页和各种设备、系统的客户端（Windows、Mac、Linux、iOS、Android 皆有）进行访问你的网盘。
- ownCloud 能将外部存储（如 FTP、WebDAV、Amazon S3，甚至 Dropbox 和 Google Drive）的文件挂载到 ownCloud 上，实现无缝存储和分享。
- 文件支持版本管理，还有回收站，所以不必担心误删。

## 搭建方法

首先你得**先有一个自己的 VPS**。。没错，要不然你的数据往哪放，ownCloud 在哪运行。

有了 VPS 之后，就可以按照官网教程一步一步安装搭建了。然而，步骤相当繁琐，你得先安装 PHP、MySQL、 Apache 等等，所以我们要祭出神器 —— docker（这里就不介绍 docker 的用法了，以下内容默认大家对 docker 的基本使用有所了解）。这样一来，之前冗长的步骤，就化成了三步：

1. 安装 docker、docker-compose，下载 ownCloud 的 image
2. 配置 docker-compose.yml
3. 配置完毕，启动，打开 ownCloud 主界面配置数据库、管理员等

> **下面是对上面三步的详细讲解，嫌太长的话可以不看。只需要把下面用到的两个 docker images （owncloud、postgres）下载好，安装 docker-compose 并拷贝 docker-compose.yml 文件到你想要存储 ownCloud 数据的文件夹，然后运行 `docker-compose up` 就好，一气呵成。**

### 使用 docker

安装好 docker 之后，直接下载 `owncloud` image 运行

```
docker run --name owncloud -p 80:80 owncloud

```

其实就可以看到 ownCloud 已经运行起来了，访问你的 VPS 地址，就可以看到 ownCloud 的界面。

![](https://geekpluxblog.oss-cn-hongkong.aliyuncs.com/docker-owncloud-1.png)

但这时的 ownCloud 还没有数据库，所以我们还需要用 docker --link 来添加一个数据库存储 ownCloud 的数据，这里用到了 `postgres` 这个 image（数据库你可以自己定，不一定要用 postgreSQL）。

```
docker run --name owncloud-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres
docker run --rm --link owncloud-postgres:owncloud-db --name owncloud -p 80:80 owncloud

```

第一条命令会启动一个 postgreSQL 数据库，默认的用户是 postgres，密码设为了 mysecretpassword，host 是 owncloud-db。

但这时我们运行的 docker container 一旦删掉，我们的数据就没有了，所以我们需要用 docker 中的 volumes (或 [docker data volumes](https://docs.docker.com/userguide/dockervolumes/#creating-and-mounting-a-data-volume-container)) 来把 ownCloud 的数据持久化。

### 配置 docker compose

这样一来，我们得启动两个 container 作为 data-only container，然后再启动 owncloud 和 postgres 关联这两个 data-only container，非常繁杂，幸亏我们有 `docker-compose` 帮忙。先安装它：

```
pip install docker-compose

```

然后配置 docker-compose.yml，下面配置中的 `volumes` 就是在配置数据持久化的目录结构。由于我把 docker-compose.yml 存在了 VPS 的`~/owncloud`文件夹下，所以底下 `volumes` 配置中，冒号前面的宿主目录是那样写的，而冒号后面的是 container 中的目录，具体：

- /etc/postgresql 存储数据库的配置
- /var/lib/postgresql 存储数据库中的数据
- /var/www/html/app 存储 ownCloud APP 的数据
- /var/www/html/data 存储 ownCloud 的数据
- /var/www/html/config 存储 ownCloud 的配置

```
postgres-data:
  image: postgres
  command: /bin/true
  volumes:
    - ~/owncloud/etc/postgresql:/etc/postgresql
    - ~/owncloud/var/lib/postgresql:/var/lib/postgresql

owncloud-data:
  image: owncloud
  
  
  command: /bin/bash -c "/bin/chown -R 33 /var/www/html/data && /bin/chown -R 33 /var/www/html/config"
  volumes:
    - ~/owncloud/var/www/html/apps:/var/www/html/apps
    - ~/owncloud/var/www/html/data:/var/www/html/data
    - ~/owncloud/var/www/html/config:/var/www/html/config

owncloud:
  image: owncloud
  ports:
    - 8080:80
  volumes_from:
    - owncloud-data
  links:
    - postgres:postgres
  hostname: cloud
  domainname: cloud.example.org 

postgres:
  image: postgres
  environment:
    - POSTGRES_USER=postgres
    - POSTGRES_PASSWORD=mypostgrespassword
  volumes_from:
    - postgres-data

```

把 docker-compose.yml 配置好之后，只需运行

就可以把 ownCloud 运行起来了，上一步中的很多操作，这里一步就搞定了。不过**切记！`owncloud-data`和`postgres-data`两个 container 和 volume 千万不要删。删之前请备份**。

### ownCloud 配置

访问你 VPS 的 8080 端口（刚才配置文件里写了）打开 ownCloud 主页，需要做两件事

1. 输入管理员的账号和密码
2. 选择数据库用哪个，且输入数据库配置，这里对照我们刚才 docker-compose 里的写的输入就好

点击完成，一切 OK，进入文件页面尽情探索吧！

文章主要讲了为什么要搭建私有网盘，以及如何用 docker + ownCloud 搭建。原文地址：https://geekplux.com/2016/10/25/how-to-setup-a-personal-cloud.html

