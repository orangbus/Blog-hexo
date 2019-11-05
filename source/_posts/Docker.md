---
title: Docker笔记
date: {{date}}
tags: Linux
categories: 
- Linux
desc: Docker笔记
---

# Docker笔记

## 1、Docker

```
docker systemtcl start/stop/restart
```

## 2、镜像

- 搜索 、下载 、 删除镜像

  ```
  docker search/pull/rmi imageName
  ```

- 重命名

  ```
  docker tag oldImage newImage
  ```

  

- 导入、导出

  ```
  docker save imageName > /home/saveImageName.tar.gz
  docker load < /home/saveImageName.tar.gz
  ```

## 3、容器

- 创建容器

  **-p**:端口映射 ， **-v** : 文件夹映射 ， **-it** : 进入docker容器交互页面 ， **--privileged** : 权限参数

  ```
  docker run -it --name containerName -v localPath:containerPath imageName bash
  ```

- 查看容器

  ```
  docker ps [-a]
  ```

- 退出、进去容器

  ```
  ctrl + p+q
  ```

  ```
  docker exec containerID/containerName
  ```

- 停止、暂停

  ```
  docker container stop/start -i/pause/unpause containerID/containerName
  ```

- 删除容器

  ```
  docker container rm containerID
  ```

## 卷：volume

- 创建、查看、删除

```
docker volume create/imspect/rm volumeName
```

- 继承

  ```
  docker run -it --volume-from containerName imageName
  ```

  

- 启动

  ```
  docker run -d -p --name web 
  # -v my-vol:webapp
  --mount source=my-vol,target=webapp
  training/webapp python app.py
  ```

## 5、网络

- 创建、查看、删除内部网络

  ```
  docker network create/inspect/rm netName
  ---
  docker network create --subnet=127.18.0.0/24(子网掩码) netName
  ```

  

## 6、DockerFile





## PXC集群：percona/percona-xtradb-cluster

- 创建PXC卷

  ```
  docker run -d -p 3306:3360
  -v columeName:/var/lib/mysql
  -e MYSQL_ROOT_PASSWORD=
  -e CLUSTER_NAME=PXC
  -e XTRABACKUP_PASSWORD=
  -privileged --name=pxcName --net=net1 --ip 127.18.0.2 PXC
  --------------
  docker run -d -p 3306:3360
  -v columeName:/var/lib/mysql
  -e MYSQL_ROOT_PASSWORD=
  -e CLUSTER_NAME=PXC
  -e XTRABACKUP_PASSWORD=
  -privileged --name=pxcName1 --net=net1 --ip 127.18.0.2 PXC
  
  docker run -d -p 3307:3360（注意）
  -v columeName2:/var/lib/mysql
  -e MYSQL_ROOT_PASSWORD=
  -e CLUSTER_NAME=PXC
  -e XTRABACKUP_PASSWORD=
  -e CLUSTER_JOIN=pxcName1(第一个创建的节点)
  -privileged --name=pxcName2（注意） --net=net1(同一个网络) --ip 127.18.0.3 PXC
  ```





































































