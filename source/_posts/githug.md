title: githug小游戏
tags: undefined
date: 2016-03-23 18:50:07
---
# install
githug是一个用ruby写的小游戏，可以用来锻炼常用的git命令。
githug需要ruby的版本在1.8.7以上。可以用下面的命令来查看ruby版本。
```bash
ruby --version
```
如果你没有安装ruby，可以需要到ruby的官网上安装对应的版本。centos可以用下面的命令安装。
```bash
yum install ruby
```
安装好ruby后，用gem包管理器来安装githug。
```bash
gem install githug
```
# 初始化
进入一个你的常用目录，键入githug，会提示你的githug目录不存在，是否新建一个，输入y继续。
<!--more-->
# githug命令
githug一共有四个命令
* play - 检查你是否过关
* hint - 给你一点提示
* reset - 重置当前的关卡或者跳到某关
* levels - 列出所有关卡

# 过关指南
## init
这关的名称是init，就是初始化一个新的git仓库。可以输入githug hint来查看过关提示。
输入git init可以初始化新仓库，接着输入githug play进行过关检测。pass
```bash
git init
```
