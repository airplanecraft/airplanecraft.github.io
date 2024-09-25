+++
Categories = ["Technology"]
Description = ""
Tags = ["linux","maxfileopen"]
date = "2016-05-07T10:30:37+08:00"
menu = "linux"
title = "linux最大文件打开数"
toc = true
+++

## 文件最大打开数
- 如果服务器提供TCP服务（TCP层或者HTTP层），在并发访问量持续很高时，容易产生too many open files错误。这时查看netstat可以发现很多TIME_WAIT状态的链接，这说明大量链接处于半开状态，已经完成了请求响应，然后进行下一步操作，如果句柄数(文件打开数)超过了阈值，那就只能等待或者出错

## 解决方案 ##

- 系统内核的修改
  ```
  cat /proc/sys/fs/file-max

  sysctl -a

  sysctl -p
  ```
- 每个用户级别的修改

  ```
  ulimit -n 1024000 (临时修改)
  vim /etc/security/limits.conf

  *  soft nofile 2048

  *  hard nofile 2048
  ```

- 如果阿里云或者aws云里面的centos都是被优化过的，默认的1024都被修改成了65536了，所以对于大部分人来说足够用了