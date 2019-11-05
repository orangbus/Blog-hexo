---
title: Linux环境变量学习笔记
date: 2017-11-17 16:03:02
tags: 
categories:
- Linux
desc: Linux是一个多用户多任务的操作系统，可以在Linux中为不同的用户设置不同的运行环境，具体做法是设置不同用户的环境变量。
keyword: Linux环境变量
---

Linux是一个多用户多任务的操作系统，可以在Linux中为不同的用户设置不同的运行环境，具体做法是设置不同用户的环境变量。

## Linux环境变量分类

一、按照生命周期来分，Linux环境变量可以分为两类：
​	1、永久的：需要用户修改相关的配置文件，变量永久生效。
​	2、临时的：用户利用export命令，在当前终端下声明环境变量，关闭Shell终端失效。

二、按照作用域来分，Linux环境变量可以分为：
​	1、系统环境变量：系统环境变量对该系统中所有用户都有效。
​	2、用户环境变量：顾名思义，这种类型的环境变量只对特定的用户有效。<!--more-->

## Linux设置环境变量的方法

一、在`/etc/profile`文件中添加变量 **对所有用户生效（永久的）**

- `/etc/profile`文件中增加变量，该变量将会对Linux下所有用户有效，并且是“永久的”。

例如：编辑/etc/profile文件，添加CLASSPATH变量

```
  vim /etc/profile    
  export CLASSPATH=./JAVA_HOME/lib;$JAVA_HOME/jre/lib
```

注：修改文件后要想马上生效还要运行`source /etc/profile`不然只能在下次重进此用户时生效。
二、在用户目录下的.bash_profile文件中增加变量 **【对单一用户生效（永久的）】**

- `vim ~/.bash_profile`文件中增加变量，改变量仅会对**当前用户有效**，并且是“永久的”。

```
export 变量名=变量值
-------------------------
vim ~/.bash.profile
export CLASSPATH=./JAVA_HOME/lib;$JAVA_HOME/jre/lib
```

注：修改文件后要想马上生效还要运行$ source ~/.bash_profile不然只能在下次重进此用户时生效。
三、直接运行export命令定义变量 **【只对当前shell（BASH）有效（临时的）】**
在shell的命令行下直接使用`export 变量名=变量值`
定义变量，该变量只在当前的shell（BASH）或其子shell（BASH）下是有效的，shell关闭了，变量也就失效了，再打开新shell时就没有这个变量，需要使用的话还需要重新定义。

## Linux环境变量使用实例

假如我们在自己的电脑上安装了Atom，我想直接使用atom命令打开Atom软件

- 首先我们找到atom的安装位置：C:\Users\OrangeStudio\AppData\Local\atom

- 在`~/.bashrc` 添加一条语句

  ```
  PATH="$PATH":/mnt/c/Users\OrangeStudio\AppData\Local\atom
  ```

当我们直接执行`atom` 就可以直接打开这个软件。（装逼必备，哈哈。。。）

## 查看环境变量

- `set` 查看所有变量  `unset`  删除变量
- `env`  查看环境变量  

## PS1变量

跳转到到相应的配置文件中，修改`PS1` 的值就可以更改系统的默认提示符

```
cd /user/local/src/
PS1="[\u@\A \w]\$ " //[]里面的参数可以自定义，记得用 \ 转义
```

参数：

d ：代表日期，格式为weekday month date，例如：”Mon Aug 1″
　　H ：完整的主机名称。例如：我的机器名称为：fc4.linux，则这个名称就是fc4.linux
　　h ：仅取主机的第一个名字，如上例，则为fc4，.linux则被省略
　　t ：显示时间为24小时格式，如：HH：MM：SS
　　T ：显示时间为12小时格式
　　A ：显示时间为24小时格式：HH：MM
　　u ：当前用户的账号名称
　　v ：BASH的版本信息
　　w ：完整的工作目录名称。家目录会以 ~代替
　　W ：利用basename取得工作目录名称，所以只会列出最后一个目录

参考文章：http://www.jianshu.com/p/ac2bc0ad3d74

慕课网视频教程：http://www.imooc.com/learn/336