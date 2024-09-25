+++
Categories = ["Technology"]
Description = "multi eni on ec2"
Tags = ["aws", "sap"]
date = "2019-09-02T8:40:37+08:00"
menu = "sap"
title = "一个ec2 instance上配置多个eni"
toc = true
+++

### 一个ec2 instance 配置多网卡 ###

- 首先要确定你需要几个公网IP，以2个为例
- 首先要确定你需要几个网卡，以2个为例
- 申请两个 elastic ip：IP1 ip2
- 申请两个eni(network interface)：eni1，eni2
- 创建一个ec2
- 把ip1 绑定到eni1,elastic ip->选中IP->associate->resource type:network interface -> private ip 自动选择
- 把ip2 绑定到eni2,此处省略步骤
- 把eni1绑定到ec2，network interface 页面->选择eni1->attach-> 选择 ec2 instance id
- 把eni2绑定到ec2,此处省略步骤
- 在ec2 instance 页面,看到iP里面公网IP只有一个，private ip 有2个
- 为什么少了一个?用putty connnect 这个两个ip，都是没有问题，ec2 的console不显示而已
  
```
[ec2-user@ip-172-31-1-176 ~]$ ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 9001
        inet 172.31.1.176  netmask 255.255.255.0  broadcast 172.31.1.255
        inet6 fe80::8a:93ff:fefd:ef5c  prefixlen 64  scopeid 0x20<link>
        ether 02:8a:93:fd:ef:5c  txqueuelen 1000  (Ethernet)
        RX packets 4115218  bytes 1170825027 (1.0 GiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 3592874  bytes 8202330562 (7.6 GiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 9001
        inet 172.31.1.87  netmask 255.255.255.0  broadcast 172.31.1.255
        inet6 fe80::f1:16ff:fefa:1bba  prefixlen 64  scopeid 0x20<link>
        ether 02:f1:16:fa:1b:ba  txqueuelen 1000  (Ethernet)
        RX packets 333  bytes 26643 (26.0 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 286  bytes 33695 (32.9 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 10549  bytes 8263339 (7.8 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 10549  bytes 8263339 (7.8 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0 collisions 0


```



