---
title: githug小游戏
date: 2016-03-23T18:50:07+08:00
---

# install

githug 是一个用 ruby 写的小游戏，可以用来锻炼常用的 git 命令。
githug 需要 ruby 的版本在 1.8.7 以上。可以用下面的命令来查看 ruby 版本。

```bash
ruby --version
```

如果你没有安装 ruby，可以需要到 ruby 的官网上安装对应的版本。centos 可以用下面的命令安装。

```bash
yum install ruby
```

安装好 ruby 后，用 gem 包管理器来安装 githug。

```bash
gem install githug
```

# 初始化

进入一个你的常用目录，键入 githug，会提示你的 githug 目录不存在，是否新建一个，输入 y 继续。

# githug 命令

githug 一共有四个命令

- play - 检查你是否过关
- hint - 给你一点提示
- reset - 重置当前的关卡或者跳到某关
- levels - 列出所有关卡

# 过关指南

## init

这关的名称是 init，就是初始化一个新的 git 仓库。可以输入 githug hint 来查看过关提示。
输入 git init 可以初始化新仓库，接着输入 githug play 进行过关检测。pass

```bash
git init
```
