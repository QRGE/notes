# Linux 简介

Linux 内核是由芬兰人林纳斯·托瓦兹（Linus Torvalds）编写的。

Linux 是一套免费使用和自由传播的类 Unix 操作系统，是一个基于 POSIX（可移植操作系统接口） 和 UNIX 的**多用户**、**多任务**、**支持多线程**和**多 CPU** 的操作系统。

Linux 能**运行主要的 UNIX 工具软件、应用程序和网络协议**。它支持 32 位和 64 位硬件。Linux 继承了 Unix **以网络为核心**的设计思想，是一个性能稳定的多用户网络操作系统。

Kali Linux : 安全渗透测试使用

redhat 认证工程师

**在 linux 中一切皆文件**

# linux 文件结构

linux 根目录的组成: ![image-20211211174240517](/Users/qr/Library/Application Support/typora-user-images/image-20211211174240517.png)

**各目录的含义：**

- **/bin**：bin是Binary的缩写, 这个目录存放着最经常使用的命令。
- **/boot：** 这里存放的是启动Linux时使用的一些核心文件，包括一些连接文件以及镜像文件。
- **/dev ：** dev是Device(设备)的缩写, 存放的是Linux的外部设备，在Linux中访问设备的方式和访问文件的方式是相同的。
- **/etc：** 这个目录用来存放所有的系统管理所需要的**配置文件**和子目录。
- **/home**：用户的主目录，在Linux中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的。
- **/lib**：这个目录里存放着系统最基本的动态连接共享库，其作用类似于Windows里的DLL文件。
- **/lost+found**：这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。
- **/media**：linux系统会自动识别一些设备，例如U盘、光驱等等，当识别后，linux会把识别的设备挂载到这个目录下。
- **/mnt**：系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在/mnt/上，然后进入该目录就可以查看光驱里的内容了。
- **/opt**：这是给主机额外安装软件所摆放的目录。
  - mac zsh 🤔

- **/proc**：这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。
- **/root**：该目录为系统管理员，也称作超级权限者的用户主目录。
- **/sbin**：s就是Super User的意思，这里存放的是系统管理员使用的系统管理程序。
- **/srv**：该目录存放一些服务启动之后需要提取的数据。
- **/sys**：这是linux2.6内核的一个很大的变化。该目录下安装了2.6内核中新出现的一个文件系统 sysfs 。
- **/tmp**：这个目录是用来存放一些临时文件的。
  - 临时的文件

- **/usr**：这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于windows下的program files目录。

  - **/usr/bin：** 系统用户使用的应用程序。
  - **/usr/sbin：** 超级用户使用的比较高级的管理程序和系统守护程序。
  - **/usr/src：** 内核源代码默认的放置目录。
- **/var**：这个目录中存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件。
- **/run**：是一个临时文件系统，存储系统启动以来的信息。当系统重启时，这个目录下的文件应该被删掉或清除。

- /www: 这个目录不一定会有, 一般用于存放服务器相关的资源, 例如项目, 静态资源等

# Linux 命令

## 操作系统相关

```shell
sync # 将数据从内存同步到硬盘中

shutdonw # 关机

reboot # 重启

halt # 关闭系统, 等同于 shutdown -h now 和 poweroff
```

## 文件相关



# 文件句柄

文件句柄也叫文件描述符, 原因是 Linux 系统的一个线程默认值为1024, 也就是说, 一个进程最多可以接受 1024 个 socket 连接

- 百度了一下, 文件句柄似乎被翻译成 file handle , 个人理解成是用于操作文件的东西
- 查看了下阿里云的默认 file handle 是 65535

在 Linux 下，通过调用 `ulimit` 命令可以看到一个进程能够打开的最大文件句柄数量。

```shell
# 查看一个线程的 file handler
# -n 用于引用或设置当前的文件句柄数量的限制值，Linux系统的默认值为1024, 如果 -n 后不设置值则表示查看当前值
ulimit -n
# -S 表示软性极限值, 软性极限值则是系统发出警告（Warning）的极限值，超过这个极限值，内核会发出警告。
# -H 表示硬性极限值, 硬性极限值是实际的限制，
ulimit -SHn
```

要彻底解除 Linux 系统的最大文件打开数量的限制，可以通过编辑 Linux 的极限配置文件`/etc/security/limits.conf`来做到

# linux 的配置文件

linux 的系统配置文件都是存放在 ``/etc/..`` 中的

## rc.local

`/etc/re.local` 是 linux 的开机启动文件, 可以在里面设置无法永久修改的操作, 例如设置 file handle

## os-release

似乎是云服务器上才有的, 可以获取到自己的当前服务器版本的一些信息

```shell
vim /etc/os-release
NAME="Ubuntu"
VERSION="20.04.1 LTS (Focal Fossa)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 20.04.1 LTS"
VERSION_ID="20.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=focal
UBUNTU_CODENAME=focal
```



## limits.conf

`/etc/security/limits.conf` linux 的限制文件