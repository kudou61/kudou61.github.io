---
title: 树莓派自动挂载U盘
date: 2016-04-27T15:00:23+08:00
---

每次插入 U 盘后都要 ssh 登陆上，然后手动挂载非常麻烦的一件事情。
可以在 udev 设备管理器新建规则。
如果要挂载 ntfs 格式的 U 盘，需要安装 ntfs-3g。

```bash
$ sudo apt-get install ntfs-3g
```

若是需要挂载 exfat 格式的 U 盘，需要安装 exfat-utils exfat-fuse

```bash
$ sudo apt-get install exfat-utils exfat-fuse
```

然后创建文件/etc/udev/rules.d/11-media-by-label-auto-mount.rules，内容如下：

```
KERNEL!="sd[a-z][0-9]", GOTO="media_by_label_auto_mount_end"
# Import FS infos
IMPORT{program}="/sbin/blkid -o udev -p %N"
# Get a label if present, otherwise specify one
ENV{ID_FS_LABEL}!="", ENV{dir_name}="%E{ID_FS_LABEL}"
ENV{ID_FS_LABEL}=="", ENV{dir_name}="usbhd-%k"
# Global mount options
ACTION=="add", ENV{mount_options}="relatime"
# Filesystem-specific mount options
ACTION=="add", ENV{ID_FS_TYPE}=="vfat|ntfs", ENV{mount_options}="$env{mount_options},utf8,gid=100,umask=002"
# Mount the device
ACTION=="add", RUN+="/bin/mkdir -p /media/%E{dir_name}", RUN+="/bin/mount -o $env{mount_options} /dev/%k /media/%E{dir_name}"
# Clean up after removal
ACTION=="remove", ENV{dir_name}!="", RUN+="/bin/umount -l /media/%E{dir_name}", RUN+="/bin/rmdir /media/%E{dir_name}"
# Exit
LABEL="media_by_label_auto_mount_end"
```

保存之后需要 reload udev

```bash
udevadm control --reload-rules
```
