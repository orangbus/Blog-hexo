---
title: Linux虚拟主机配置
date: 2017-07-26 20:47:08
tags: Linux
categories: 
- Linux
from: 
desc: Linux下虚拟主机配置
keyword: Linux下虚拟主机配置
---

# Linux配置Apache虚拟主机

> 简单说就是自定义一个域名绑定一个目录，通过域名访问一个www工作目录

我使用的deepin系统，web环境是xampp

**首先我们找到Apache目录的 httpd.conf 文件**：/opt/lampp/apache2/conf/httpd.conf

```
#我们看到的是这样的结构，和网上的有很多不同之处，但是都是一样的

Alias /bitnami/ "/opt/lampp/apache2/htdocs/"
Alias /bitnami "/opt/lampp/apache2/htdocs"

<Directory "/opt/lampp/apache2/htdocs">
    Options Indexes FollowSymLinks
    AllowOverride All
    Order allow,deny
    Allow from all
</Directory>
```

我们在这个文件的下面添加如下代码：<!--more-->

```
NameVirtualHost *:80
 
<VirtualHost *:80>
    DocumentRoot /var/www/html1（自行配置）
    ServerName www.tp5.com
</VirtualHost>
 
<Directory "/var/www/html1（自行配置）">
    Options Indexes FollowSymLinks
    AllowOverride all
    Order Deny,Allow
    Deny from none
    Allow from all
</Directory>
```

如果有多个www环境就多复制几份，然后自行修改下。

**接下来我们需要修改 hosts 文件：/etc/hosts**

​	注意了，有些小伙伴到 /etc/ 下面找了好久也没有找到 hosts 文件，别担心，没有的话自行 `touch hosts` 一个文件就可以了，或者找旁边window系统里面复制一份 hosts   文件到 /etc/ 目录下。

```
127.0.0.1 localhost
127.0.0.1 www.tp5.com 你设置的域名(对应上面ServerName www.tp5.com)
```

**验证：** 

​	上诉配置修改完成后, 使用 /usr/local/apache/bin/apachectl  -t来测试配置文件是否正确。 调试成功后，启动配置文件。   

重启apache服务： /usr/local/apache/bin/apachectl restart 

平滑重启服务命令：/usr/local/apached/bin/apachectl reload

如果没有反应就百度一下：修改 hosts 后怎么生效，我试过好多都不行了，所以我就不列在这里了，百度一下你就知道了，Google一下你就知道的太多了，哈哈。。。

参考博客：http://xushaojie.blog.51cto.com/6205370/1765353