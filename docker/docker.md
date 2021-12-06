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

查看容器内部进程信息

```shell
docker top container
```

查看镜像的源数据(详细信息)

```shell
# 查看一个镜像的详细信息
docker inspect container
[
    {
    		# 平常见的 id 只是源 id 的一部分
        "Id": "9ec935c01b3ef765f1ab13389aad435c7254e1bdeeab9fdfda129741affd30c3",
        "Created": "2021-12-05T13:05:54.903115771Z",
        "Path": "/docker-entrypoint.sh",
        "Args": [
            "nginx",
            "-g",
            "daemon off;"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 154222,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2021-12-05T13:05:55.218862322Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:ea335eea17ab984571cd4a3bcf90a0413773b559c75ef4cda07d0ce952b00291",
        "ResolvConfPath": "/var/lib/docker/containers/9ec935c01b3ef765f1ab13389aad435c7254e1bdeeab9fdfda129741affd30c3/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/9ec935c01b3ef765f1ab13389aad435c7254e1bdeeab9fdfda129741affd30c3/hostname",
        "HostsPath": "/var/lib/docker/containers/9ec935c01b3ef765f1ab13389aad435c7254e1bdeeab9fdfda129741affd30c3/hosts",
        "LogPath": "/var/lib/docker/containers/9ec935c01b3ef765f1ab13389aad435c7254e1bdeeab9fdfda129741affd30c3/9ec935c01b3ef765f1ab13389aad435c7254e1bdeeab9fdfda129741affd30c3-json.log",
        "Name": "/elated_khorana",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "docker-default",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/2879ec739fdb61186c372f2720091925cf3f8144f2344e0cf2fe6dc68b6ce4dd-init/diff:/var/lib/docker/overlay2/bd1950851dc8a212b1543d9dc8c67d9cfc68146f40320b694453c0782aea347b/diff:/var/lib/docker/overlay2/36b05a86e9ca554bec111058c0b6d109aceb4f20e8ed86c46c1467d2f4fd7ef0/diff:/var/lib/docker/overlay2/70647e75cf7fa88a22333358919d506c379d4e9cf86f9282e3f70b97a9e37f96/diff:/var/lib/docker/overlay2/f39febd1ca24eb7e32ec4f265772f7deb719aee1ec3c2e076c54a7bbeffc1cdb/diff:/var/lib/docker/overlay2/345d190b9d8bccc7b5bb53f632564351d1a0d244a7bca41dcc7bf03425392e2d/diff:/var/lib/docker/overlay2/638831b8bc920f0d6adadcf4c38d4efbf0adba46900c2824903a1349e3efa200/diff",
                "MergedDir": "/var/lib/docker/overlay2/2879ec739fdb61186c372f2720091925cf3f8144f2344e0cf2fe6dc68b6ce4dd/merged",
                "UpperDir": "/var/lib/docker/overlay2/2879ec739fdb61186c372f2720091925cf3f8144f2344e0cf2fe6dc68b6ce4dd/diff",
                "WorkDir": "/var/lib/docker/overlay2/2879ec739fdb61186c372f2720091925cf3f8144f2344e0cf2fe6dc68b6ce4dd/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "9ec935c01b3e",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.21.4",
                "NJS_VERSION=0.7.0",
                "PKG_RELEASE=1~bullseye"
            ],
            "Cmd": [
                "nginx",
                "-g",
                "daemon off;"
            ],
            "Image": "nginx",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": [
                "/docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
            },
            "StopSignal": "SIGQUIT"
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "b16131c77a15f9942bc894dc86a748922900a184705f748605885d543489f8fc",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {
                "80/tcp": null
            },
            "SandboxKey": "/var/run/docker/netns/b16131c77a15",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "d28fbd3a3c16858cf39d37486c0a16010ca9947771428509379980df98494572",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "2d1e18bd767915be0a222d1f92ff6726750d4d3a6825164b81eff9003ba24413",
                    "EndpointID": "d28fbd3a3c16858cf39d37486c0a16010ca9947771428509379980df98494572",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

### 进入容器

docker exec

```shell
docker exec -it container /bin/bash

# docker exec 进入容器后会开启一个新的终端, 可以执行各种操作
# 相对于 docker attach 比较常用
```

docker attach 

```shell
docker attach container

# docker 进入容器正在执行的终端, 你可以通过这种方式查看运行时打印在终端的日志
```

### 操作文件

docker cp

```shell
# 将容器内的文件拷贝到主机中
docker cp container:path 主机路径
```

