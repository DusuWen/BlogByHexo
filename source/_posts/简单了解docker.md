---
title: 简单了解docker
date: 2024-08-12 14:01:40
tags:
  - Docker
---

## 什么是 Docker

Docker 是一个可以把程序和环境打包并运行的工具软件

<!-- more -->
## 什么是 Docker Image

Docker 镜像是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。镜像 不包含 任何动态数据，其内容在构建之后也不会被改变。

## 什么是 Docker Container

Docker 容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。容器的实质是进程，但与直接在宿主执行的进程不同，容器进程运行于属于自己的独立的 命名空间。

## 什么是 Dockerfile

Dockerfile 是一个文本文件，其内包含了一条条的 指令(Instruction)，每一条指令构建一层，因此每一条指令的内容，就是描述该层应当如何构建。

## 常用命令
### 构建镜像
```bash
docker build -t image-name .
```

### 列出所有镜像
```bash
docker images
```

### 指定导出的路径和镜像名称
```bash
docker save -o /path/to/save/image-name.tar image-name
```

### 导入镜像
```bash
docker load -i /path/to/load/image-name.tar
```

### 启动一个容器
```bash
docker run -dp port:port image-name
```
一些常见的启动参数
*  -p 本机端口:容器端口 映射本地端口到容器
* -P 将容器端口映射为本机随机端口
* -v 本地路径或卷名:容器路径 将本地路径或者数据卷挂载到容器的指定位置
* -it 作为交互式命令启动
* -d 将容器放在后台运行
* --rm 容器退出后清除资源

### 查看所有容器
```bash
docker ps -a
```

### 删除容器
```bash
docker rm container_id
```

### 停止容器
```bash
docker stop container_id
```

### 启动容器
```bash
docker start container_id
```

### 清理镜像缓存
```bash
docker image prune -a
```

### 清理系统缓存
```bash
docker system prune -a
```
