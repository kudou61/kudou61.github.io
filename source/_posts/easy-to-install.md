title: 每次都要安装的东西
tags: undefined
date: 2016-03-19 22:16:49
---

#homebrew

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
# vim7.4

mac中自带的vim是7.3版本的，如果使用spf13，To make all the plugins work, specifically neocomplete, you need vim with lua.

```bash
brew install vim --with-lua
```
安装报错的话，可能需要ruby环境

# spf13

```bash
curl https://j.mp/spf13-vim3 -L > spf13-vim.sh && sh spf13-vim.sh
```
# python2.7

```bash
brew install python
```
更新pip

```bash
pip install --upgrade pip
```
更新pip后/usr/local/bin/pip会重新创建链接，如果安装了Python3，pip的指向会改变，需要重新创建链接

```bash
/usr/local/bin/pip -> ../Cellar/python/2.7.11/bin/pip
```
# python3

```bash
brew install python3
```
更新pip3

```bash
pip3 install --upgrade pip
```
更新会把系统的pip链接指向pip3，如果有多个Python版本的话需要重新关联。

# nodejs

```bash
brew install node
```
执行后会自动安装npm

