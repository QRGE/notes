[docker doc](https://docs.docker.com/docker-hub/)

[docker-hub](https://registry.hub.docker.com)

## 容器技术

容器技术可以充分利用宿主机的内核, 只需要把需要运行的程序和环境打包成一个容器, 容器在宿主机的内核上运行 

- docker 没有自己的内核, 直接创建一个 docker deamon 在主机的内核中, 创建的容器也是运行在 docker 进程中, 所以 docker 比 vm 要快

# Docker 简介

docker 可以帮助你快速设置环境😁, 解决不同设备(运行环境)的配置环境问题

- 有了 docker 可以实现 DevOps

docker 是 go 开发的

docker 的三个重要概念: 

- image
- container
  - docker 可以通过容器技术, 独立运行一个或一组应用, 容器通过镜像创建
- repository
  - 存放镜像的仓库

对于 Docker 镜像来说，如果不显式指定TAG，则默认会选择latest标签，这会下载仓库中最新版本的镜像。

```shell
# 如果不显示指定 tag, 会下载最新的 latest 版本, 即 ubuntu:latest
# 同时这个命令是从默认的镜像仓库中拉取镜像
docker pull ubuntu
```

# 配置 aliyun 镜像加速

控制台 ~> 左侧栏产品与服务 ~> 容器服务 ~> 容器镜像服务

<img src="/Users/qr/Library/Application Support/typora-user-images/image-20211204003449159.png" alt="image-20211204003449159" style="zoom:50%;" />

镜像工具 ~> 镜像加速器

<img src="/Users/qr/Library/Application Support/typora-user-images/image-20211204003556521.png" alt="image-20211204003556521" style="zoom:50%;" />

# docker 命令

## docker 帮助命令

```shell
# 查看 docker 版本信息
docker version
# 显示 docker 的系统信息
docker info
# 查看某个命令的帮助
docker 命令 --help
```

## image 相关命令

镜像操作的基本命令, 例如获取、查看、搜索、删除、创建、存出、载入、上传等需要知道, 只要要有一个比较好的笔记到时候可以复制粘贴🤪

### docker images

查看当前主机上的所有镜像信息

```shell
# 显示所有的镜像信息
docker images 
# 参数
-a, --all             Show all images (default hides intermediate images)
		--digests         Show digests
-f, --filter filter   Filter output based on conditions provided
		--format string   Pretty-print images using a Go template
		--no-trunc        Don't truncate output
-q, --quiet           Only show image IDs
# 列出所有镜像的 id
docker images -aq

# 指定格式输出: id\t名字
docker images --format "{{.ID}}\t{{.Repository}}"
```

| 符号          | 含义                                     |
| ------------- | ---------------------------------------- |
| .ID           | Image ID                                 |
| .Repository   | Image repository                         |
| .Tag          | Image tag                                |
| .Digest       | Image digest                             |
| .CreatedSince | Elapsed time since the image was created |
| .CreatedAt    | Time when the image was created          |
| .Size         | Image disk size                          |

### docker search

搜索某个镜像的信息

```shell
# 搜索指定镜像名字的信息
docker search imageName
-f, --filter filter   Filter output based on conditions provided
		--format string   Pretty-print search using a Go template
		--limit int       Max number of search results (default 25)
		--no-trunc        Don't truncate output
# 搜索包含名字包含 mysql 且 STARS >= 3000 的镜像 	
docker search mysql --filter="STARS=3000"
```

### docker pull 

 拉取镜像, 如果不指定 tag 则默认拉取最新版本的镜像

```shell
# 拉取 mysql:8.0.27 版本的 mysql
docker pull mysql:8.0.27
```

具体镜像的 tag 可以到 docker hub 里面搜索

### docker rmi

删除镜像

```shell
# 删除指定id的镜像, 可以指定多个镜像id
docker rmi -f imageId1 imagesId2 ...
# 删除所有的镜像... 危险‼️
docker rmi -f ${docker images -aq}
```

### 查看

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

## container 相关

需要有 image 才能启动 container

### docker run

```shell
docker run [参数] image
--name="name"	设置容器的名字
-d						后台运行方式, 有坑🥶
-it						使用交互模式运行, 可以进入容器查看内容
-p						指定容器端口, 一般的格式为 -p 主机端口:容器端口
-P						指定随机端口

docker run -it centos /bin/bash

# 退出容器
exit
```

### docker ps

查看正在运行的容器

```shell
# 查看 docker 正在运行的容器
docker ps 
# 可选参数
-a	# 查看正在运行的容器 + 历史运行的容器
-n  # 显示最近创建的几个容器, 例如 -n=2 即最近两个容器
```

### quit

退出容器

```shell
exit 							# 容器直接停止并退出
command + p + q 	# 容器退出不停止
```

### docker rm

```shell
# remove container by id
# 不能删除正在运行的容器
docker rm containerId
# remove all containers
docker rm ${docker ps -aq}
docker ps -aq | xargs docker rm # 借助 linux 命令删除
```

### docker start

```shell
docker start containerId
docker restart containerId
docker stop containerId
docker kill containerId 			# 强制停止
```

## 其他常用命令

后台启动容器:

```shell
docker run -d centos # -d 表示在后台运行程序
```

查看日志

```shell
docker logs [OPTIONS] container
      --details        Show extra details provided to logs
  -f, --follow         Follow log output
      --since string   Show logs since timestamp (e.g. 2013-01-02T13:23:37Z) or relative     												(e.g. 42m for 42 minutes)
  -n, --tail string    Number of lines to show from the end of the logs (default "all")
  -t, --timestamps     Show timestamps
      --until string   Show logs before a timestamp (e.g. 2013-01-02T13:23:37Z) or relative 											 (e.g. 42m for 42 minutes)
# 查看某个容器的最近10条日志
docker logs -tf --tail 10 container
```

