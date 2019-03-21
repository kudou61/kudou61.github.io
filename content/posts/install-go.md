---
title: Go语言
date: 2015-12-23T15:27:52+08:00
tags:
---

Go 语言又名 Golang（官网[link](https://golang.org/)），是 Google 干爹在 2009 年发布的开源编程语言。
Golang 专门针对多处理器系统应用程序的编程进行了优化，使用 Go 编译的程序可以媲美 C 或 C++代码的数独，而且更安全、支持并行进程。
Codis 是豌豆荚发布的一个分布式 Redis 解决方案，使用 Go 和 C 语言开发，以代理的方式实现了一个 Redis 分布式集群解决方案，链接 Codis Proxy 和连接原生的 Redis Proxy 没有明显的区别。要使用 Codis 首先我们需要安装 Go。

## 安装 Go

我们在 Golang 的[官方网站](https://golang.org/dl/)上下载最新的发行版，linux 系统的下载链接是这个*https://storage.googleapis.com/golang/go1.5.2.linux-amd64.tar.gz*
使用 wget 命令下载到本地。

```console
[root@CentOS ~]# wget https://storage.googleapis.com/golang/go1.5.2.linux-amd64.tar.gz
```

下载完后将压缩包解压到/usr/local 目录下，就会在/usr/local/go 目录下建立 go 的目录结构。

```console
[root@CentOS ~]# tar -C /usr/local -xzf go1.5.2.linux-amd64.tar.gz
```

将/usr/local/go/bin 加入到 PATH 环境变量中我们可以编辑/etc/profile（系统级别）或者\$HOME/.bash_profile 文件

```console
export PATH=$PATH:/usr/local/go/bin
```

如果要使用 go get 命令下载源码或者设置 Golang 的工作目录，还需要配置 GOPATH

```console
export GOPATH=$HOME/work
```

## 安装 godep

godep 就是综合了多年第三方包依赖问题的解决方案后的一个趋向统一的方案。
按照官方使用说明的第一步，先下载 godep

```console
$ go get github.com/tools/godep
```

不过 GOPATH 目录并不在环境变量中，不能直接使用，所以还要配置一下

```console
export PATH=$PATH:/usr/local/go/bin:/root/work/bin
```
