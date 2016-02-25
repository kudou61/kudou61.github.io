title: 小技巧
tags:
  - 笔记
categories:
  - 小技巧
---
# Sublime
在命令行中使用Sublime打开文件
可以使用软连接方式：
````
sudo ln -s "/Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl" /usr/bin/subl
````
执行后会在/usr/bin下建立指向Sublime的软连接
````
ll /usr/bin|grep subl
lrwxr-xr-x   1 root   wheel    63B  6  1 11:38 subl -> /Applications/Sublime Text.app/Contents//SharedSupport/bin/subl
````
还可以使用alias
````
alias subl="'/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl'"
````