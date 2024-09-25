+++
Categories = ["Technology"]
Description = "NAT instance setup"
Tags = ["aws", "sap"]
date = "2019-08-20T14:40:37+08:00"
menu = "sap"
title = "NAT instance setup 简介"
toc = true
+++

### AWS NAT instance setup 简介 ###

- NAT（Network Address Translation，网络地址转换）是1994年提出的。当在专用网内部的一些主机本来已经分配到了本地IP地址（即仅在本专用网内使用的专用地址），但现在又想和因特网上的主机通信（并不需要加密）时，可使用NAT方法。

### AWS NAT gateway ###

- Aws 有專門的nat gateway,並且是HA的，只要創建gateway然後更改一下，subnet的路由就可以了，所以自己搭建一個nat gateway實際並不是aws的範疇，實際是一個linux系統的問題

### 手動搭建一個nat instance ###


 - private subnet1/ instance1 / sg1(secrutiry group) / internal IP1  ->local node1
 - public  subnet2/ instance2 / sg2(sercirity group) / internal IP2 /public ip2 -> proxy node2

### 配置proxy node ###

```
vi /etc/sysctl.conf
net.ipv4.ip_forward=1

iptables -t nat -A POSTROUTING -o eth0 -s 192.168.1.0/24 -j MASQUERADE

```

- 192.168.1.0/24 為本地網絡CIDR

- EC2 頁面，選中proxy node2 ->Action ->Networking ->Disable source/Desk Check

### 配置 local node 1

- 選擇 subnet1  ->Route Table  
- 點擊route table id  ->Routes  
- Edit routes ：add 0.0.0.0/0  ->target 選中instance ->選中 proxy node2
- ssh 進入node1 :ping 8.8.8.8 -> 沒有為子網配置igw，有沒有配置 nat gateway, 但是可以訪問 internet 


