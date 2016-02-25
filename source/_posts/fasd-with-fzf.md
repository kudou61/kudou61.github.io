title: 终端下快速打开编辑文件、进入目录、键入历史命令
date: 2016-01-04 14:35:46
tags: 
---
如果你经常使用终端，那么你必然会经常切换目录。如果频繁的使用cd命令，那么会非常浪费时间。

有没有比较高效的方法呢，答案是肯定的。

如果只需要解决切换目录的问题的话，用autojump就可以实现，使用j键加目录可以快速的递归查找到该目录，但并不是最好的方法。

这里推荐两个工具，[fasd](https://github.com/clvv/fasd) and [fzf](https://github.com/junegunn/fzf)，可以满足上面的三个需求。

fasd通过记录用户经常使用的目录，给每个目录分配一定的权重，频繁访问的目录权重越大。而当你使用fasd模糊查找一个目录时，它会查询符合条件的权重最大的目录。

## 安装
Mac下推荐使用brew安装。

```console
brew install fasd fzf
```

<!--more-->
~~Mac安装完fasd后并不能直接使用，zsh会提示commad not found。
这里使用的是zsh，在终端中键入如下命令：（和fzf一起使用的时候会有冲突，这里就删除了）~~

```bash
echo 'eval "$(fasd --init auto)"' >> ~/.zshrc
```

## 配置
安装完后可以安装官方的配置，在.bashrc中加入如下代码可以配置fasd。

```bash
alias a='fasd -a'        # any
alias s='fasd -si'       # show / search / select
alias d='fasd -d'        # directory
alias f='fasd -f'        # file
alias sd='fasd -sid'     # interactive directory selection
alias sf='fasd -sif'     # interactive file selection
alias zz='fasd_cd -d -i' # cd with interactive selection
```
fzf要和fasd配合使用的话需要在加入如下代码。

```bash
v() {
  local file
  file="$(fasd -Rfl "$1" | fzf -1 -0 --no-sort +m)" && vi "${file}" || return 1
} 

z() {
  local dir
  dir="$(fasd -Rdl "$1" | fzf -1 -0 --no-sort +m)" && cd "${dir}" || return 1
}
 
fh() {
  eval $( ([ -n "$ZSH_NAME" ] && fc -l 1 || history) | fzf +s --tac | sed 's/ *[0-9]* *//')
}
```
配置完后需要重启生效，或者执行：

```console
source .bashrc
```

## 使用

fasd会记录你使用命令的历史和频率，按照权重排列出来。

通过和fzf配合可以实现高亮和动态模糊匹配。

比如你曾经这样切换目录

```console
cd Project/blog
```
然后你使用z回车后，这个路径就会出现在你的候选中。
![screen](/img/2016-01-04 下午3.02.08.png)

同样的，使用v也可以快速打开历史编辑过的文件。

使用fh可以打开历史输入命令。

你使用的频率越高，候选的命令就越准确。

还有很多高效的用法，大家可以自己探索。


