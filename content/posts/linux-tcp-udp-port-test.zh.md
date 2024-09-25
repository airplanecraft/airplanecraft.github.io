+++
Categories = ["Technology"]
Description = "Linux tcp and udp port testing"
Tags = ["linux", ]
date = "2017-06-02T12:40:37+08:00"
menu = "linux"
title = "如何测试linux的tcp和udp端口"
toc = true
+++

## 如何测试linux的tcp和udp端口 ##

- 测试tcp 一般用使用 telnet
   ```
   telnet 192.168.12.10 22

   ```
- telnet 不支持udp协议，所以我们可以使用nc,nc可以支持tcp也可以支持udp
  ```
   yum install -y nc
   nc -z -v 192.168.10.12 22 #tcp
   nc -z -v -u 192.168.10.12 123 # udp

  ```
