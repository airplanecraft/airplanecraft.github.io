+++
Categories = ["Technology"]
Description = ""
Tags = ["linux", "lvm"]
date = "2016-05-06T10:30:37+08:00"
menu = "linux"
title = "linux的tree命令"
toc = true
+++


## linux tree command ##

- Centos 是默认不带tree 命令的，如果要使用首先要install tree command
```
yum install -y tree

```

- 使用tree 显示目录的深度为2级

```
 tree -L 2
.
├── easy-rsa-old-2.3.3
│   ├── configure.ac
│   ├── COPYING
│   ├── COPYRIGHT.GPL
│   ├── distro
│   ├── doc
│   ├── easy-rsa
│   └── Makefile.am
├── frgs.log
├── frp_0.32.1_linux_amd64
│   ├── frgs.log
│   ├── frpc
│   ├── frpc_full.ini
│   ├── frpc.ini
│   ├── frps
│   ├── frps_full.ini
│   ├── frps.ini
│   ├── frps.log
│   ├── LICENSE
│   ├── nohup.out
│   └── systemd
├── frp_0.32.1_linux_amd64.tar.gz
```

