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

```shell
# 切换目录
cd XX # 切换到某个目录下面
cd / # 切换到根目录
cd .. # 返回到上一级

# 显示当前路径内容
ls # 查看当前目录下的内容
ls -a # 列出所有内容, 包括隐藏内容
ls -l # 显示内容的属性和权限等内容

ll # 查看当前目录内容(相对于 ls 显示的内容比较详细), 相当于 ls -l

pwd # 查看当前位置路径

# 创建目录
mkdir [-mp] dirName # 创建一个目录
	-m : 配置目录的权限
	-p : 递归创建目录
mkdir -p /dir1/dir2/dir3 # 需要创建层级目录需要参数 -p

# 删除目录
rmdir [-p] dir1 # 删除 dir1 的空目录
rmdir -p dir1/dir2/dir3 # 递归删除多级目录
# rmdir 只能删除空目录, 不能删除包含文件的目录

# 删除文件
rm -rf XX # -f 强制删除 -r 递归删除, 删除文件夹下面所有的内容
	 -i # 互动, 询问是否删除 

# 复制文件
cp source target # 复制文件 source 到 target 目录下 
	-a : 相当於 -pdr 的意思，至於 pdr 请参考下列说明
		-p : 连同文件的属性一起复制过去，而非使用默认属性(备份常用)
		-d : 若来源档为连结档的属性(link file)，则复制连结档属性而非文件本身
		-r : 递归持续复制，用於目录的复制行为(常用)
	-f : 为强制(force)的意思，若目标文件已经存在且无法开启，则移除后再尝试一次
	-i : 若目标档(destination)已经存在时，在覆盖时会先询问动作的进行(常用)
	-l : 进行硬式连结(hard link)的连结档创建，而非复制文件本身
	-s : 复制成为符号连结档 (symbolic link)，亦即『捷径』文件
	-u : 若 destination 比 source 旧才升级 destination

# 移动文件或目录, 也可以用过移动到同一目录下达成重命名的效果
mv 
```

# 文件属性

Linux系统是一种典型的多用户系统，不同的用户处于不同的地位，拥有不同的权限。为了保护系统的安全性，Linux系统对不同的用户访问同一文件（包括目录文件）的权限做了不同的规定。

在Linux中我们可以使用`ll`或者`ls –l`命令来显示一个文件的属性以及文件所属的用户和组，如：

![image-20211212221231026](/Users/qr/note/linux/image-20211212221231026.png)

例如，boot文件的第一个属性用"d"表示。"d"在Linux中代表该文件是一个目录文件。

在Linux中第一个字符代表这个文件是目录、文件或链接文件等等：

- *当为[ **d** ]则是目录
- *当为[ **-** ]则是文件
- *若是[ **l** ]则表示为链接文档 ( link file )
  - 链接类似于 windows 的快捷方式, 上图的 bin ~> usr/bin/ 则指明了指向文件本体

- 若是[ **b** ]则表示为装置文件里面的可供储存的接口设备 ( 可随机存取装置 )
- 若是[ **c** ]则表示为装置文件里面的串行端口设备，例如键盘、鼠标 ( 一次性读取装置 )

接下来的字符中，**以三个为一组**，且均为 [rwx] 的三个参数的组合。

- [ r ]代表可读(read)
- [ w ]代表可写(write)
- [ x ]代表可执行(execute)。

这三个权限的位置不会改变，如果没有权限，就会出现减号[ - ]

每个文件的属性由左边第一部分的10个字符来确定（如下图）：

![image-20211212215717567](/Users/qr/Library/Application Support/typora-user-images/image-20211212215717567.png)

从左至右用0-9这些数字来表示。

第0位确定文件类型，第1-3位确定属主（该文件的**所有者**）拥有该文件的权限。第4-6位确定属组（所有者的**同组**用户）拥有该文件的权限，第7-9位确定**其他用户**拥有该文件的权限。

其中：

第1、4、7位表示读权限，如果用"r"字符表示，则有读权限，如果用"-"字符表示，则没有读权限；

第2、5、8位表示写权限，如果用"w"字符表示，则有写权限，如果用"-"字符表示没有写权限；

第3、6、9位表示可执行权限，如果用"x"字符表示，则有执行权限，如果用"-"字符表示，则没有执行权限。

对于文件来说，它都有一个特定的所有者，也就是对该文件具有所有权的用户。

同时，在Linux系统中，用户是按组分类的，一个用户属于一个或多个组。

文件所有者以外的用户又可以分为文件所有者的同组用户和其他用户。

因此，Linux系统按文件所有者、文件所有者同组用户和其他用户来规定了不同的文件访问权限。

在以上实例中，boot 文件是一个目录文件，属主和属组都为 root

## 修改文件属性

```bash
# 更改文件组, 比较少用
chgrp [-R] 组名 文件名

# 修改文件的所属用户
chown [-R] 用户 文件名

# 修改文件属性
chmod [-R] path
# 通过数字表示对应的权限, r:4 w:2 x:1, 没有权限就是0, 三位权限的和代表设置某一项的权限
chmod 777 path # 设置全部权限, 相当于 rwxrwxrwx
chmod 700 path # rwx------
```

# 查看文件内容

查看文件内容的命令

```shell
# 第一行开始显示文件内容
cat 

# tac 最后一行开始显示, cat tac ...
tac

# 现实的时候会输出行号
nl

# 分页显示内容
# 空格代表翻页, enter向下一行, 参数 :f 显示行号
# 通过 /xx 向下搜索关键词 xx 的位置, 通过 ?xx 向上搜索关键词 xx 的位置
# n: 向下寻找下一个关键词, N: 向上寻找
more

# 分页显示内容, 可以实现前后翻页, 通过上下箭头进行翻页
less

# 查看内容的头几行
head -n 20 # 查看头 20 行命令

# 查看内容的尾几行
tail
```

# 链接

Linux 中链接分为两种:

- 硬连接: A ~> B, 假设 B 是 A 的硬链接, 那么他们指向了同一个文件
  - linux 允许一个文件拥有多个路径, 即如果一个文件有 A 和 B 的硬链接, A 被删除了还可以通过 B 访问文件
  - 可以通过建立硬链接到一些重要文件上防止误删文件
- 软链接: 类似 windows 的快捷方式, 删除了源文件也访问不了

```bash
# 生成 f1 的硬连接 f2
ln f1 f2
# 生成 f2 的软链接
ln -s f1 f2
```

创建一个硬链接只是将一个文件创建多一份"链接", 如果一个文件有两个链接 A , B, 删除 B 是不会硬盘中的文件的, 此时如果把 A 也删除了我也并不确定硬盘中的文件是否会被删除

- 相对于 cp , ln 只是创建链接, 而 cp 会再硬盘中创建多一份内容

# 网络

```shell
# 查看网络信息, 注意和 windows 的 ipconfig 的区别
ifconfig
```

# Vim

vim 主要记住各种命令就行, 怎么方便怎么来

## Vim 模式

Vim 有三种模式:

- 命令模式 command mode
- 输入模式 insert mode
- 底线命令模式 last line mode

刚进入 vim 时就是命令模式, 通过按键进行各种操作:

- i 切换到输入模式
- x 删除当前光标所处的字符
- : 进入底线命令模式
- Esc: 退出输入模式

常用命令:

```shell
# 到文本的头一行
gg
# 到文本的最后一行
G
# 移动到屏幕的第一行
H
# 移动到屏幕的中间行
M
# 移动到屏幕最下方的一行
L
# 删除当前行
dd
# n 为数字, 表示向下移动 n 行
n+enter
```

底线命令

```shell
# w 是保存, q是退出, wq即保存并退出
:wq
# 删除第 n 行
:nd

# 显示行号
:set nu
# 不显示行号
:set nonu
```

# 账号管理

设置不同的账号可以更好的进行资源的控制和安全管理

```shell
# 添加用户
useradd -[] username
-m : 自动创建主目录 即 /home/username
-c : comment 添加注释性
-d : 目录 指定用户主目录路径，如果此目录不存在，则同时使用 -m 选项，可以创建主目录
-g : 用户组 指定用户所属的用户组
-G : 用户组，用户组 指定用户所属的附加组
-s : Shell文件 指定用户的登录Shell
-u : 用户号 指定用户的用户号，如果同时有-o选项，则可以重复使用其他用户的标识号

# 删除用户
# 删除用户即将 /etc/passwd 用户信息删除掉
userdel -r username
-r : 删除用户的同时也删除用户的主目录

# 修改用户
# usermod 的选项同添加用户
usermod -[] username
usermod -d /xx # 切换用户主目录
# 给用户设置密码
passwd username 

# 切换用户
su user # 切换到 user 账户, 想要退出就 exit
```

# Ubuntu 修改 hostname

查看主机名

```shell
# 查看主机名
hostname
```

设置 /etc/cloud/cloud.cfg 的 preserve_hostname 属性设置为 true

![image-20211217222930464](/Users/qr/note/linux/image-20211217222930464.png)

通过 `hostnamectl set-hostname` 主机名设置新的主机名

```shell
hostnamectl set-hostname newHostname
```



# Ubuntu 安装 mysql

ubuntu 本机安装软件 mysql

```shell
# 更新软件源
sudo apt-get update
# 安装mysql
sudo apt-get install mysql-server
```

安装后默认是没有密码的, 可以在 "mysql" 库的 user 表中设置密码

```shell
# 查看信息
select Host, authentication_string, user from user;
# 修改密码
ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'newPassword';  
```



# 文件句柄

文件句柄也叫文件描述符, 原因是 Linux 系统的一个线程默认值为1024, 也就是说, 默认情况下一个线程最多可以接受 1024 个 socket 连接

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

# Screen

## screen 的常用命令

常用命令: 

```shell
# 新建一个叫 screenName 的session
screen -S screenName
# 列出所有创建的 screen
screen -ls 
# 进入到 screenName 这个session
screen -r screenName 
# 远程 detach 某个 session screenName
screen -d screenName
# 结束当前 session 并回到 screenName 这个 session
screen -d -r screenName
```



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

## passwd

/etc/passwd : 保存用户信息

## limits.conf

`/etc/security/limits.conf` linux 的限制文件

# 二进制文件

运行二进制文件方式: 直接运行指定目录了

```shell
# 二进制文件一般都会放到 bin 里面, 直接运行就行了
/opt/terraria/bin/XX 
# 如果是当前文件可以加上 ./ 
./XX
```



# Script

## 基础知识

终端以开头 $ 表示普通用户，# 表示管理员用户 root

## HelloWorld

第一个脚本

```shell
#! /bin/bash
## author: qr
## This is my first script
date +"%Y-%m-%d %H:%M:%S" # date 格式化输出当前时间
echo 'My first script'
```



# 泰拉瑞亚配置

## 命令

为了方便取的别名:

```shell
# 直接到泰拉瑞亚的根目录
alias terraria='cd /opt/terraria/bin'
# 根据配置文件运行游戏
alias terraria-start='/opt/terraria/bin/TerrariaServer.bin.x86_64 -config /opt/terraria/config/terraria.conf'
```



```
world=/opt/terraria/worlds/QR的世界.wld
autocreate=3
worldname=QR的世界
difficulty=1
maxplayers=8
password=123456
worldpath=/opt/terraria/worlds

./TerrariaServer.bin.x86_64 -config /opt/terraria/config/terreria.conf
```

```shell
help            Displays a list of commands.
playing         Shows the list of players.
clear           Clear the console window.
exit            Shutdown the server and save.
exit-nosave     Shutdown the server without saving.
save            Save the game world.
kick <player>   Kicks a player from the server.
ban <player>    Bans a player from the server.
password        Show password.
password <pass> Change password.
version         Print version number.
time            Display game time.
port            Print the listening port.
maxplayers      Print the max number of players.
say <words>     Send a message.
motd            Print MOTD.
motd <words>    Change MOTD.
dawn            Change time to dawn.
noon            Change time to noon.
dusk            Change time to dusk.
midnight        Change time to midnight.
settle          Settle all water.
seed            Displays the world seed.
```

