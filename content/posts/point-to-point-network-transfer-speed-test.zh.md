+++
Categories = ["Technology"]
Description = "point to point data transfer"
Tags = ["linux"]
date = "2021-06-02T10:00:30+08:00"
menu = "linux"
title = "網絡服務器點對點的網速測試"
toc = true
+++

## 本文简介

- 我們跟數據供應商之間數據傳輸一直存在很多的問題，我們的網絡經過測速后非常的快，供應商也反復說他們的網速也非常快，那麽問題來了?既然大家都快，是不是在某個路由環節之間出現了問題呢?

## iperf 簡介

- iperf是一种命令行工具，用于通过测量服务器之間可以处理的最大网络吞吐量来诊断网络速度问题。

## iperf信息
    iperf版本： iperf 3.1.3
    官网地址： https://iperf.fr
## 安裝和運行

server
```

rpm -ivh https://iperf.fr/download/fedora/iperf3-3.1.3-1.fc24.x86_64.rpm
iperf3 -s

 ```

client

```
rpm -ivh https://iperf.fr/download/fedora/iperf3-3.1.3-1.fc24.x86_64.rpm
iperf3 -c serverip -p 5201

```

## 運行結果

```
[root@PC-231 ~]# iperf3 -c 172.16.10.101 -p 5201
Connecting to host 172.16.10.101, port 5201
[  4] local 192.168.25.231 port 59398 connected to 172.16.10.101 port 5201
[ ID] Interval           Transfer     Bandwidth       Retr  Cwnd
[  4]   0.00-1.00   sec   110 MBytes   926 Mbits/sec   80    175 KBytes
[  4]   1.00-2.00   sec   110 MBytes   920 Mbits/sec   65    223 KBytes
[  4]   2.00-3.00   sec   110 MBytes   920 Mbits/sec   65    240 KBytes
[  4]   3.00-4.00   sec   110 MBytes   919 Mbits/sec   80    211 KBytes
[  4]   4.00-5.00   sec   110 MBytes   920 Mbits/sec   65    225 KBytes
[  4]   5.00-6.00   sec   109 MBytes   918 Mbits/sec   78    206 KBytes
[  4]   6.00-7.00   sec   109 MBytes   917 Mbits/sec   68    236 KBytes
[  4]   7.00-8.00   sec   109 MBytes   918 Mbits/sec   84    206 KBytes
[  4]   8.00-9.00   sec   110 MBytes   920 Mbits/sec   65    242 KBytes
[  4]   9.00-10.00  sec   109 MBytes   916 Mbits/sec   88    211 KBytes
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth       Retr
[  4]   0.00-10.00  sec  1.07 GBytes   919 Mbits/sec  738             sender
[  4]   0.00-10.00  sec  1.07 GBytes   919 Mbits/sec                  receive
```


## 結論

1. 數據供應商宣稱我們的帶寬不夠，實際從他們哪裏測試到我們這裏，每秒穩定在大概70M/s的傳輸速度，所以我們的帶寬是足夠的，問題是他們自己的程序出現了問題，對IO模型的處理遠遠不夠造成了阻塞