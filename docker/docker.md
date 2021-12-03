[docker doc](https://docs.docker.com/docker-hub/)

## 容器技术

容器技术可以充分利用宿主机的内核, 只需要把需要运行的程序和环境打包成一个容器, 容器在宿主机的内核上运行 

- 容器没有自己的内核, 这点和虚拟机不同

# Docker 简介

docker 可以帮助你快速设置环境😁, 解决不同设备(运行环境)的配置环境问题

- 有了 docker 可以实现 DevOps

docker 是 go 开发的

docker 的三个重要概念: 

- image
- container
  - docker 可以通过容器技术, 独立运行一个或一组应用, 容器通过镜像创建

- repository

对于 Docker 镜像来说，如果不显式指定TAG，则默认会选择latest标签，这会下载仓库中最新版本的镜像。

```shell
# 如果不显示指定 tag, 会下载最新的 latest 版本, 即 ubuntu:latest
# 同时这个命令是从默认的镜像仓库中拉取镜像
docker pull ubuntu
```

# docker 命令

## image 相关

镜像操作的基本命令, 例如获取、查看、搜索、删除、创建、存出、载入、上传等需要知道, 只要要有一个比较好的笔记到时候可以复制粘贴🤪

### 查看

使用 image 子命令获取本地镜像信息查看

```shell
# 两者的效果相同
docker images
docker image ls
```

inspect [image] 获取某个镜像的详细信息

```shell
# 获取 ubuntu:latest 镜像的所有信息
docker inspect ubuntu:latest
# -f 可以用于筛选需要获取的信息
docker inspect -f {{".Architecture"}} ubuntu:latest
```

docker history [image] 用于获取某个镜像的修改历史, 类似于 git log

```shell
# 查看本地安装的 ubuntu 镜像的修改历史
docker history ubuntu:latest --no-trunc
```

### 搜索

docker search [option] keywword 在默认的仓库中搜索你想要的镜像, 支持的选项有:

- -f, --filter=condition：条件过滤后输出内容；
- --format string：格式化输出内容；
- --limit int：限制输出结果个数，默认为25个；
- --no-trunc：不截断输出结果;

docker rmi或docker image rm命令可以删除镜像，命令格式为docker rmi IMAGE [IMAGE...]，其中IMAGE可以为标签或ID。

## container 相关

