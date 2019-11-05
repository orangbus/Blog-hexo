---
title: windows上启动linux桌面
date: 2017-09-29 19:45:15
tags: linux
categories: 
- Linux
form: https://blog.choldrim.com/2016/10/07/%E5%80%9F%E5%8A%A9bash-on-win%E5%9C%A8windows%E4%B8%8A%E5%90%AF%E5%8A%A8linux%E6%A1%8C%E9%9D%A2/
desc: windows上启动linux桌面,linux
---

![](https://blog.choldrim.com/wp-content/uploads/2016/10/%E6%B7%B1%E5%BA%A6%E6%88%AA%E5%9B%BE20161012215608.png)

> 首先开启window提供的子系统，一般window10最新版带着吧，所以不用考虑什么版本问题，不直接就问问度娘喽【控制面板--程序--启动或关闭window功能】
>
> 或者参考下这个blog：[Weicot]( http://www.weicot.com/win10%E4%BD%BF%E7%94%A8%E8%BF%9B%E9%98%B6-%E5%BC%80%E5%90%AFlinux-bash%E5%91%BD%E4%BB%A4%E8%A1%8C%E8%BF%99%E6%98%AF%E4%B8%AA%E5%AE%8C%E6%95%B4%E7%9A%84-linux-%E5%AD%90%E7%B3%BB%E7%BB%9F/)

![](https://blog.choldrim.com/wp-content/uploads/2016/10/4.png)

安装完成后会提示需要重启系统，重启之后，Bash便在你的系统中安装了，你可以在开始菜单输入Bash，或者在CMD程序中输入Bash也可以启动Bash on ubuntu on windows。<!--more-->

## 启动DBus

当想启动ubuntu桌面或者firefox之类的就需要DBus了
因为Bash on win本身对网络的实现还不是很完善，unix socket还不支持，DBus直接拉不起来，修改一下DBus的配置文件，让其使用tcp启动

```
sudo sed -i 's$<listen>.*</listen>$<listen>tcp:host=localhost,port=0</listen>$' /etc/dbus-1/session.conf

```

（详细可以参考：[[Bash on Windows\] Getting dbus and X server working](https://www.reddit.com/r/Windows10/comments/4rsmzp/bash_on_windows_getting_dbus_and_x_server_working/)）

这个时候可以安装和启动firefox试试

## 启动Ubuntu Unity Desktop

先安装桌面相关组件，在Bash中执行：

```
sudo apt-get install ubuntu-desktop unity compizconfig-settings-manager

```

安装完桌面组件，先对compiz做一些配置

```
ccsm

```

各项配置设置成如下两图：
![img](https://blog.choldrim.com/wp-content/uploads/2016/10/9236a296-468e-11e6-860b-f340f4176df6.png)
![img](https://blog.choldrim.com/wp-content/uploads/2016/10/bcc0f6ec-468e-11e6-811f-ac26804a9ab7.png)

启动compiz窗管，拉起unity桌面

```
compiz

```

![img](https://blog.choldrim.com/wp-content/uploads/2016/10/%E6%B7%B1%E5%BA%A6%E6%88%AA%E5%9B%BE20161012215608.png)

文章来源：https://blog.choldrim.com/2016/10/07/%E5%80%9F%E5%8A%A9bash-on-win%E5%9C%A8windows%E4%B8%8A%E5%90%AF%E5%8A%A8linux%E6%A1%8C%E9%9D%A2/