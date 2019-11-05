---
title: Linux_alias
date: 2017-08-21 13:47:03
tags:
categories: 
- Linux
desc: alias使用方法
keyword: alias使用方法，linux，alias
---

## alias使用方法

- 新建别名

  ```
  alias aliasName="xxxx"
  ---------------------
  alias vi='vim'
  ```

- 删除别名

  ```
  unalias aliasName
  ```

## 如何永久生效<!--more-->

- 那就把别名写入环境变量中

  ```
  vi ~/.bashrc  // '~'指的是home目录，可以使用 ls -a 即可看到 .bashrc
  //添加别名
  alias vi='vim'
  ```


- 重新启动配置文件

  ```
  source .bashrc
  ```

## 常用快捷键

ctrl + c   强制终止当前命令
ctrl + l    清屏操作
ctrl + a    光标移动到命令行首
ctrl + e    光标移动到命令行尾
ctrl + u    光标所在位置删除到行首
ctrl + z     把命令放入后台
ctrl + r     在历史命令中搜索