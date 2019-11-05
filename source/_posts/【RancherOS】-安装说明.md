title: 【RancherOS】 安装说明
author: OrangBus
tags:
  - Docker
categories:
  - Docker
date: 2019-11-05 16:43:00
---
### 一、环境准备

- 准备好 xshell 和 xftp 客户端（安装 xManager 即可）.
- 将 rancheros.iso 刻录为光盘或 U 盘、做成系统盘.<!--more-->

### 二、安装 RancherOS

#### 1. 启动

使用系统盘的用户将系统盘插入主机，打开电源安装。启动后，在命令行中设置临时账户密码，命令如下：

sudo passwd rancher

在提示符出现时输入两次新密码，例如：supermap

#### 2. 设置网络

检查网络：

一、通过 ifconfig 查看主机 ip，如果可以看到 eth0 项则继续进行下一步，否则请联系技术支持人员。

二、检查主机是否已分配到 ip，如下图 eth0 中没有 inet addr 项，则需要配置 ip，如果主机已经分配到 ip，则不用配置。

![](http://imgr.supermap.io/imanager/help/image10.gif)

添加 ip 命令如下：（{ip} 为将配置的临时 ip，可联系网络管理员获取）

sudo ip addr add {ip} dev eth0

![](http://imgr.supermap.io/imanager/help/image11.gif)

设置成功后通过 ifconfig 查看结果：

![](http://imgr.supermap.io/imanager/help/image12.gif)

修改 rancheros_config.yml 文件：

从附件中找到 rancheros_config.yml 文件打开，如果使用 DHCP 分配 ip，只需设置 dhcp 为 true，示例如下：

![](http://imgr.supermap.io/imanager/help/image13.gif)

如果不使用 DHCP，用户需要自己设置 ip（可联系网络管理员获取），示例如下：

![](http://imgr.supermap.io/imanager/help/image14.gif)

#### 3. 上传 yml 文件

使用 xshell 登录主机，ip 和密码参见第 1、2 步

在 xshell 命令行中输入 ssh rancher@{ip}，弹出的 “ssh 用户身份验证” 中选择 “password”，填写登录密码，点击确定。（{ip} 为主机 ip）

此时，就能通过 xshell 连接到主机。

![](http://imgr.supermap.io/imanager/help/image137.jpg)

登录后点击右上角新建文件传输，通过 xftp 将本地的 rancheros_config.yml 文件上传到 Docker 主机

![](http://imgr.supermap.io/imanager/help/image138.jpg)

#### 4. 查看本地磁盘

执行如下命令查看本地磁盘：

sudo fdisk ‐l

选择最大的一块磁盘安装系统，如下示例中，选择 / dev/sda 来安装系统。

![](http://imgr.supermap.io/imanager/help/image15.gif)

#### 5. 安装操作系统

使用以下命令安装系统：（其中 / dev/sda 为上一步选择的最大磁盘名，为登录密码）

sudo ros install ‐c rancheros_config.yml ‐d /dev/sda ‐‐append 'rancher.password='

示例如下：

![](http://imgr.supermap.io/imanager/help/image16.gif)

安装过程中，会出现两次提示，均选择 y 即可，安装完成之后会重启系统.

#### 6. 登录

系统重启后，第 1 步设置的临时账户密码会失效，使用第 5 步中设置的登录密码在 xshell 或者直接在主机登录。

主机登录：

在 superMap login 中填写用户名为 “rancher”，Password 填写密码

![](http://imgr.supermap.io/imanager/help/image17.gif)

运行 docker -v，可以看到 Docker 已经内置、安装好。

docker ‐v

【特别注意】：默认开放的 Docker 远程端口为 2375，可运行以下命令进行验证

docker ‐H :2375 info

### 三、安装 docker-compose

用 xftp 将 docker-compose.sh 和 docker-compose.tar 上传至 Docker 主机;

在命令行运行：

sudo docker load ‐i ./docker‐compose.tar

sudo chmod +x ./docker‐compose.sh

DockerCompose 即安装完成，运行如下命令验证:

./docker‐compose.sh ‐v

【特别注意】这里要用./docker-compse.sh 来代替 docker-compose 命令进行执行。

### 四、FAQ

Q: rancheros_config.yml 文件中配置的 ip 不能连接网络怎么办？

A: 使用如下命令修改网络配置:

修改 ip:（{ip} 改为你的真实 ip）

sudo ros config set rancher.network.interfaces.eth0.address {ip}

修改网关:({getway} 改为你的真实网关)

sudo ros config set rancher.network.interfaces.eth0.gateway {getway}

示例：

![](http://imgr.supermap.io/imanager/help/image18.gif)

然后重启主机，新的 ip 配置便会生效。