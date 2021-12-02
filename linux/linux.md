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

## rc.local

`/etc/re.local` 是 linux 的开机启动文件, 可以在里面设置无法永久修改的操作, 例如设置 file handle

## limits.conf

`/etc/security/limits.conf` linux 的限制文件