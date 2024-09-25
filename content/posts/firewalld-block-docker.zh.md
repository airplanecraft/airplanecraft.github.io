+++
Categories = ["Technology"]
Description = "裸金属服务器遇到的docker容器访问问题"
tags = ["baremetal", "cloud"]
date = "2017-09-06T10:40:37+08:00"
menu = "cloud"
title = "裸金属服务器遇到的docker容器访问问题"
toc = true
+++


## 问题描述 ##

    我们本地数据中心服务器处理与客户的接口的时候发现网络有严重的延迟，然后我们又有数据合规方面的问题，我们就采用云端的裸金属服务器，安装了docker后发现docker无法访问到主机。

## 问题发现 ##

   - docker exec -ti docker-id bash 进入主机
   - 在docker主机内部telnet 主机IP 端口
   - 发现无法访问
   - 退出虚拟主机在主机执行telnet 主机IP 端口 ，是可以访问的
   - 检查网络 ![pic](firewall-docker.png) 
   - 原来是docker0 与主机eth0 之间通信问题被firewall屏蔽了

   ```
    firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.17.0.1/16" accept"
    firewall-cmd --reload 
   ```






  

