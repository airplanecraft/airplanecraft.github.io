+++
Categories = ["Technology"]
Description = ""
Tags = ["linux", "lvm"]
date = "2016-04-20T10:30:37+08:00"
menu = "saa"
title = "lvm空间扩容"
toc = true
+++
```
[root@localhost ~]# df -H
Filesystem               Size  Used Avail Use% Mounted on
devtmpfs                  17G     0   17G   0% /dev
tmpfs                     17G     0   17G   0% /dev/shm
tmpfs                     17G  9.1M   17G   1% /run
tmpfs                     17G     0   17G   0% /sys/fs/cgroup
/dev/mapper/centos-root   54G  1.4G   53G   3% /
/dev/sda2                1.1G  148M  916M  14% /boot
/dev/sda1                210M   12M  198M   6% /boot/efi
/dev/mapper/centos-home   12T   35M   12T   1% /home
tmpfs                    3.4G     0  3.4G   0% /run/user/0
```
分区不够合理 root 下面空间不够用

```
umount /home

lvreduce -L 5T /dev/mapper/centos-home

lvextend -L +5T /dev/mapper/centos-root

xfs_growfs /dev/mapper/centos-root

mkfs.xfs -f /dev/mapper/centos-home

mount /home

```
