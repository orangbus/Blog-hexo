---
title: linux更换源
date: 2017-09-29 20:32:58
tags: linux
categories: 
- Linux
desc: linux如何更换源
keyword: linux如何更换源
---

## 我们就直接手动修改了

- 先备份改一下我们原来的

  ```
  sudo cp /etc/apt/sources.list /etc/apt/sources.list.old
  ```

- 然后修改

  ```
  sudo gedit /etc/apt/sources.list
  ----反正就是这个地址，也可以用 vi，但是加上 sudo------
  sudo vi /etc/apt/sources.list
  ```

- 把`sources.list`里面的文件替换成中国提供的源就可以，比如 中科大<!--more-->

  ```
  # 默认注释了源码仓库，如有需要可自行取消注释
  deb https://mirrors.ustc.edu.cn/ubuntu/ xenial main restricted universe multiverse
  # deb-src https://mirrors.ustc.edu.cn/ubuntu/ xenial main restricted universe multiverse
  deb https://mirrors.ustc.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
  # deb-src https://mirrors.ustc.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
  deb https://mirrors.ustc.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
  # deb-src https://mirrors.ustc.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
  deb https://mirrors.ustc.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
  # deb-src https://mirrors.ustc.edu.cn/ubuntu/ xenial-security main restricted universe multiverse

  # 预发布软件源，不建议启用
  # deb https://mirrors.ustc.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
  # deb-src https://mirrors.ustc.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
  ```

ps：其他的百度一下吧