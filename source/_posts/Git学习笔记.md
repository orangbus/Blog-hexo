---
title: Git学习笔记
date: 2017-07-25 23:46:32
tags: Linux
categories: 
- Linux
desc: Git笔记手册
---

```
git config --global user.name "orange"
git config --global user,email "135.qq.com"
```

### 1、创建Git仓库-查看-推送

- git init  建立一个当前的git仓库
- git status
- git add .
- git commit -m "本次提交文件变化的描述信息"
- git remote add githuburl  | git remote add origin gihubur l  git push -u oringin master  |  git push githuburl  master <!--more-->

### 2、分支的创建于合并删除

- git branch 查看所有分支
- git branch 新分支[分支起点]
- git checkout  切换分支
- git checkout -b  新分支[分支起点]
- git checkout mastergit merge
- git branch -d 分支名
- git branch -D 删除分支名
- Git merge dev 合并分支（切换到主分支合并，dev是需要合并的分支）

### 3、查看提交历史与版本回溯

- git log
- git refig id     (id可以用 head@{数字} 来表示对应的ID )
- git checkout --filename  |  git reset HEAD
- git checkout 分支名或标签名
- 先通过git reflog找到某个版本的 commit id
  然后用git reset --hard commit-ID （git reset --hard ebb5358 ）将工作目录的文件恢复到指定的版本
- 恢复被提交到仓库被删的文件
  git checkout --filename  | git checkout -f  |  git ls-files -d |  xargs git checkout

### 4、 标签的添加、删除、查看

- git tag
- git tag  标签名
- git tag -a 标签名 -m "附加信息"
- git show 标签名
- git tag -d 标签名

### 5. 查看工作目录/暂存区/仓库之间的差异

- git diff 	是比较工作目录与暂存区的差异
- git diff HEAD   则是比较工作目录与仓库中最近一次的提交间的差异
- git diff --cached  比较了暂存区与仓库中最近一次的提交间的差异。

### 6、与远程仓库交互

- git remote  查看远程仓库
- Git remote –v 查看远程仓库地址
- git remote add 仓库名 仓库地址
- git remote rm 远程仓库名
- git remote rename  原名 新名
- git clone githuburl
- git fetch 远程仓库名
- git pull 远程仓库名 本地需要合并的仓库名
- git push 远程仓库名 本地仓库名
- git remote show 远程仓库名
- git push 远程仓库名 标签名

### 7、配置公共钥匙

- 配置远程仓库
  git remote add 远程仓库名 远程仓库地址
- 创建shh key
  shh-keygen -t rsa -C "邮件地址"   一直回车
- 把 id_rsa.pub放在GitHub仓库公钥中

备份

垃圾回收

上游同步

后续慢慢跟新。。。。。

```
详细笔记链接
```

