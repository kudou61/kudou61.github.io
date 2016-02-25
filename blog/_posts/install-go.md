title: Go语言
date: 2015-12-23 15:27:52
tags:
---
Go语言又名Golang（官网[link](https://golang.org/)），是Google干爹在2009年发布的开源编程语言。
Golang专门针对多处理器系统应用程序的编程进行了优化，使用Go编译的程序可以媲美C或C++代码的数独，而且更安全、支持并行进程。
Codis是豌豆荚发布的一个分布式Redis解决方案，使用Go和C语言开发，以代理的方式实现了一个Redis分布式集群解决方案，链接Codis Proxy和连接原生的Redis Proxy没有明显的区别。要使用Codis首先我们需要安装Go。
## 安装Go
我们在Golang的[官方网站](https://golang.org/dl/)上下载最新的发行版，linux系统的下载链接是这个*https://storage.googleapis.com/golang/go1.5.2.linux-amd64.tar.gz*
使用wget命令下载到本地。
```console
[root@CentOS ~]# wget https://storage.googleapis.com/golang/go1.5.2.linux-amd64.tar.gz
```
<!--more-->

下载完后将压缩包解压到/usr/local目录下，就会在/usr/local/go目录下建立go的目录结构。
```console
[root@CentOS ~]# tar -C /usr/local -xzf go1.5.2.linux-amd64.tar.gz
```
将/usr/local/go/bin加入到PATH环境变量中我们可以编辑/etc/profile（系统级别）或者$HOME/.bash_profile文件
```console
export PATH=$PATH:/usr/local/go/bin
```
如果要使用go get命令下载源码或者设置Golang的工作目录，还需要配置GOPATH
```console
export GOPATH=$HOME/work
```
## 安装godep
godep就是综合了多年第三方包依赖问题的解决方案后的一个趋向统一的方案。
按照官方使用说明的第一步，先下载godep
```console
$ go get github.com/tools/godep
```
不过GOPATH目录并不在环境变量中，不能直接使用，所以还要配置一下
```console
export PATH=$PATH:/usr/local/go/bin:/root/work/bin
```

