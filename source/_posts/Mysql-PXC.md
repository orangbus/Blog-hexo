---
title: Docker-Mysql-PXC集群
date: 2019-06-04 18:56:22
tags: Docker
categories:
- Linux
keyword: mysql-pxc集群方案
---

# PXC集群安装

percona/percona-xtradb-cluster: <https://hub.docker.com/r/percona/percona-xtradb-cluster/>

```
docker pull percona/percona-xtradb-cluster
```

修改tag

```
docker tag oldName NewName  //docker tag docker.io/percona/percona-xtradb-cluster pxc
```

# 创建内部网络

```
docker network create net1
docker network inspect net1
docker network rm net1
-----------
docker network create --subnet=192.168.3.0（myIP）/24 net1
```

##创建数据卷

```
docker volume create v1
```

## 创建PXC容器

```
docker run -d -p 3306:3306
-v v1:/var/lib/mysql
-e MYSQL_ROOT_PASSWORD=123456
-e CLUSTER_NAME=PXC  //集群的名字
-e XTRABACKUP_PASSWORD=123456  //数据库同步需要用到的密码,自己在这里设置的
--privileged  //最高权限
--name=node1 //集群别名
--net=net1 
--ip 192.168.3.1  //规定内部网段
pxc
```

# Mysql集群步骤

```
docker network create --subnet=192.168.3.0/24 net1

docker volume create --name v1
docker volume create --name v2
docker volume create --name v3

docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -e CLUSTER_NAME=PXC -e XTRABACKUP_PASSWORD=a123456 -v v1:/var/lib/mysql --privileged --name=node1 --net=net1 --ip 192.168.3.2 pxc

docker run -d -p 3307:3306 -e MYSQL_ROOT_PASSWORD=123456 -e CLUSTER_NAME=PXC -e XTRABACKUP_PASSWORD=a123456 -e CLUSTER_JOIN=node1 -v v2:/var/lib/mysql --privileged --name=node2 --net=net1 --ip 192.168.3.3 pxc

docker run -d -p 3308:3306 -e MYSQL_ROOT_PASSWORD=123456 -e CLUSTER_NAME=PXC -e XTRABACKUP_PASSWORD=a123456 -e CLUSTER_JOIN=node1 -v v3:/var/lib/mysql --privileged --name=node3 --net=net1 --ip 192.168.3.4 pxc
```

以上注意ip地址不要冲突，数据库映射端口，网段是一致的，数据卷不同，pxc集群的ip地址不冲突,数据库链接使用主机的ip地址，不是虚拟的ip地址

# 数据库负载均衡-Haproxy

安装 haproxy

```
docker pull haproxy
```

创建haproxy配置文件：`touch /home/haproxy.cfg` 【https://orangbus.gitee.io 搜索：haproxy】









