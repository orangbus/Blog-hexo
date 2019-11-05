---
title: docker-mirrors
date: 2019-06-08 19:14:32
tags: docker
categories:
- Linux
---

# 配置Docker镜像源，加速国内拉取Docker镜像

## Step 1 : 配置Docker参数文件

首先，我们编辑 `/etc/docker/daemon.json` 这个文件，并加入以下内容：

```
{
  "registry-mirrors": ["https://registry.docker-cn.com"]
}
```

之后保存退出。<!--more-->

## Step 2 : 重启Docker Deamon

执行命令：

```
systemctl restart docker
```

或者：

```
service docker restart
```

来将Docker的Deamon重启，使得改动生效。

**注意：这个动作会导致当前所有Docker容器停止。请在继续操作前保存工作。**

**重启的过程需要数十秒的时间，请耐心等待！**

## Step 3 : 验证配置

在重启完成后，我们试着拉个镜像：

```
docker pull ubuntu
```

是不是发现，速度相较于之前，要快了很多？

## 随便补充几句

这个镜像源是由Docker官方部署在中国的反代仓库。其实很多家云服务提供商都在提供反代加速，比如：

- 阿里云 [(官方文档 >>)](https://blog.ilemonrain.com/go/aHR0cHM6Ly95cS5hbGl5dW4uY29tL2FydGljbGVzLzI5OTQx)
- DaoCloud [(官方文档 >>)](https://blog.ilemonrain.com/go/aHR0cHM6Ly93d3cuZGFvY2xvdWQuaW8vbWlycm9yI2FjY2VsZXJhdG9yLWRvYw==) 