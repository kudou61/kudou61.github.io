---
title: raspberry的一些配置
date: 2016-04-26T23:49:29+08:00
---

# Aria2

Aria2 是一个多线程下载工具，支持 HTTP/HTPPS，FTP，SFTP，BT 和 Metalink。

## 下载

可以跨平台使用，在 Linux 上可以使用自带的包管理器下载安装。

```bash
$ apt-get install aria2
```


## 配置

新建 aria2.conf 配置文件，dir 是下载文件保存路径，可以根据需要修改。

```
#用户名
#rpc-user=user
#密码
#rpc-passwd=passwd
#上面的认证方式不建议使用,建议使用下面的token方式
#设置加密的密钥
#rpc-secret=token
#允许rpc
enable-rpc=true
#允许所有来源, web界面跨域权限需要
rpc-allow-origin-all=true
#允许外部访问，false的话只监听本地端口
rpc-listen-all=true
#RPC端口, 仅当默认端口被占用时修改
#rpc-listen-port=6800
#最大同时下载数(任务数), 路由建议值: 3
max-concurrent-downloads=5
#断点续传
continue=true
#同服务器连接数
max-connection-per-server=5
#最小文件分片大小, 下载线程数上限取决于能分出多少片, 对于小文件重要
min-split-size=10M
#单文件最大线程数, 路由建议值: 5
split=10
#下载速度限制
max-overall-download-limit=0
#单文件速度限制
max-download-limit=0
#上传速度限制
max-overall-upload-limit=0
#单文件速度限制
max-upload-limit=0
#断开速度过慢的连接
#lowest-speed-limit=0
#验证用，需要1.16.1之后的release版本
#referer=*
#文件保存路径, 默认为当前启动位置
dir=/Users/Aaron/Downloads
#文件缓存, 使用内置的文件缓存, 如果你不相信Linux内核文件缓存和磁盘内置缓存时使用, 需要1.16及以上版本
#disk-cache=0
#另一种Linux文件缓存方式, 使用前确保您使用的内核支持此选项, 需要1.15及以上版本(?)
#enable-mmap=true
#文件预分配, 能有效降低文件碎片, 提高磁盘性能. 缺点是预分配时间较长
#所需时间 none < falloc ? trunc « prealloc, falloc和trunc需要文件系统和内核支持
file-allocation=prealloc
```

为了启动方便，可以新建一个 run.sh 脚本来启动 Aria2。

```bash
$ aria2c --conf-path="./aria2.conf" -D
```

注意 aria2.conf 和 run.sh 要放到同一个目录下。

# Samba 服务

Samba 是在 Linux 和 Unix 上实现 SMB 协议的一个免费软件，它可以为局域网中的不同计算机提供文件和打印机等资源的共享服务。

## 下载

```bash
$ apt-get install samba
```

## 配置

首先先将 smb 的配置文件进行一下备份，方便出了问题之后还原。

```bash
$ cp /etc/samba/smb.conf /etc/samba/smb.conf.bk
```

在 smb.conf 的末尾加入自己需要共享的目录

```
[media]
    comment = media on pi
    path = /media
    writable = yes
```

Samba 中的用户必须是系统中存在的用户，可以使用下面的命令授权系统用户访问 Samba。

```bash
$ smbpasswd -a pi #添加pi用户
```

输完命令后，会提示为用户 pi 设置访问密码，设置完成后需要重启 smb 服务。
配置完 Samba 后，还需要注意 Linux 安全机制：iptables 和 selinux。
在对待 iptables 的问题上：
普通青年：直接在命令行敲…
service iptables stop。
文艺青年：依次在命令行敲…
iptables -I RH-Firewall-1-INPUT 5 -m state --state NEW -m tcp -p tcp --dport 139 -j ACCEPT
iptables -I RH-Firewall-1-INPUT 5 -m state --state NEW -m tcp -p tcp --dport 445 -j ACCEPT
iptables -I RH-Firewall-1-INPUT 5 -p udp -m udp --dport 137 -j ACCEPT
iptables -I RH-Firewall-1-INPUT 5 -p udp -m udp --dport 138-j ACCEPT
iptables-save
service iptables restart
同样，在对在 selinux 的问题上：（这丫的把我坑惨了呀）
普通青年：直接在命令行敲…
setenforce 0
vi /etc/selinux/config
将 SELINUX=enforcing 改为 SELINUX=disabled 为开机重启后不再执行 setenfore 节约光阴。
文艺青年：依次在命令行敲…
setsebool -Psamba_enable_home_dirs on
setsebool -Psamba_export_all_rw on
完事儿之后再：getsebool -a | grep samba 一把，你懂得…

## 启动

```bash
$ sudo /etc/init.d/samba start
```
