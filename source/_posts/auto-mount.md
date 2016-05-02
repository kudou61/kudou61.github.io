title: 树莓派自动挂载U盘
tags: undefined
date: 2016-04-27 15:00:23
---

每次插入U盘后都要ssh登陆上，然后手动挂载非常麻烦的一件事情。
可以在udev设备管理器新建规则。
如果要挂载ntfs格式的U盘，需要安装ntfs-3g。
```bash
$ sudo apt-get install ntfs-3g
```
若是需要挂载exfat格式的U盘，需要安装exfat-utils exfat-fuse
``` bash
$ sudo apt-get install exfat-utils exfat-fuse
```
<!-- more -->
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
保存之后需要reload udev
```bash
udevadm control --reload-rules
```
