---
title: "Docker笔记"
date: 2021-11-26T22:49:46+08:00
draft: false
slug: e32c011c
description: "Docker常用命令"
tags:
  - Docker
categories: Docker
---

#### Docker服务相关命令

- 启动Docker服务

  ~~~shell
  systemctl start docker
  ~~~

- 停止Docker服务

  ~~~shell
  systemctl stop docker
  ~~~

- 重启Docker服务

  ~~~shell
  systemctl restart docker
  ~~~

- 查看Docker服务状态

  ~~~shell
  systemctl status docker
  ~~~

- 开机启动Docker服务

  ~~~shell
  systemctl enable docker
  ~~~

#### Docker镜像相关命令

- 查看镜像

  ~~~shell
  docker images
  ~~~

- 搜索镜像

  ~~~shell
  docker search 镜像名称
  ~~~

- 下载镜像

  ~~~shell
  docker pull 镜像名称:版本号
  ~~~

- 删除镜像

  ~~~shell
  docker rmi 镜像名称:版本号/镜像ID
  ~~~

#### Docker容器相关命令

- 查看容器

  ~~~shell
  docker ps		//查看正在运行的容器
  docker ps -a	//查看所有容器(包括已经停止的容器
  ~~~

- 创建容器

	- 交互式容器:创建完这个容器后 容器会立即启动 并且自动进入到容器内部 退出容器后容器会自动关闭-it 代表创建的容器是交互式的 

  ~~~shell
  docker run -it --name=容器的名称 镜像名称:版本号 /bin/bash 
  ~~~
	- 守护式容器: 创建完这个容器后 容器会立即启动 不会自动进入到容器内部 退出容器后容器会不会关        闭-id 代表创建的容器是守护式的
  ~~~shell
  docker run -id --name=容器的名称 镜像名称:版本号
  ~~~
	- 创建式容器:创建完这个容器不会立即启动 也不会进入到容器内部 
  ~~~shell
  docker create --name=容器名称 镜像名称:版本
  ~~~

- 进入容器
  ~~~shell
  docker exec -it 容器名称/容器ID /bin/bash
  ~~~

- 启动容器
  ~~~shell
  docker start 容器名称/容器ID
  ~~~

- 停止容器
  ~~~shell
  docker stop 容器名称/容器ID
  ~~~
  
- 删除容器

  ~~~shell
  docker rm 容器名称/容器ID
  ~~~

- 查看容器信息

  ~~~shell
  docker inspect 容器名称/容器ID
  ~~~

- 容器导出为镜像

  ~~~shell
  docker commit 容器名称 镜像名称
  ~~~

- 镜像导出为压缩文件

  ~~~shell
  docker save –o tar文件名 镜像名
  ~~~

- 导入压缩文件为镜像

  ~~~shell
  docker load -i tar文件名
  ~~~

  