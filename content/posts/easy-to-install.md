---
title: 每次都要安装的东西
date: 2016-03-19T22:16:49+08:00
---

#homebrew

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

# vim7.4

mac 中自带的 vim 是 7.3 版本的，如果使用 spf13，To make all the plugins work, specifically neocomplete, you need vim with lua.

```bash
brew install vim --with-lua
```

安装报错的话，可能需要 ruby 环境

# spf13

```bash
curl https://j.mp/spf13-vim3 -L > spf13-vim.sh && sh spf13-vim.sh
```

# python2.7

```bash
brew install python
```

更新 pip

```bash
pip install --upgrade pip
```

更新 pip 后/usr/local/bin/pip 会重新创建链接，如果安装了 Python3，pip 的指向会改变，需要重新创建链接

```bash
/usr/local/bin/pip -> ../Cellar/python/2.7.11/bin/pip
```

# python3

```bash
brew install python3
```

更新 pip3

```bash
pip3 install --upgrade pip
```

更新会把系统的 pip 链接指向 pip3，如果有多个 Python 版本的话需要重新关联。

# nodejs

```bash
brew install node
```

执行后会自动安装 npm
