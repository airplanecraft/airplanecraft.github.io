+++
Categories = ["Technology"]
Description = "how to use privatelink to connect the serivce between 2 vpc without public IP adress"
Tags = ["vpc","privatelink"]
date = "2021-07-28T12:40:37+08:00"
menu = "terraform"
title = "阿里云架构-使用privatelink让不同的vpc的服务可以访问"
toc = true
+++

## 本来使用工具或者服务

- Alicloud
- vpc
- SLB
- PrivateLink endpoit zone and service
- ECS

## 架构图

![PrivateLink](/images/privatelink.png)

## 架构说明

- 对于两个网络（VPC-A，VPC-B）的ECS提供的服务，要是A访问B，可以提供以下方案
    1. B通过EIP或者IP或者绑定SLB，通过SLB的IP把B的服务暴露在公网上
    2. A网络和B网络互联
    3. 通过privatelink来单向向让A网络的访问B网络的服务

- 分析上面优缺点
    1. 安全性不好，把服务暴露在公网
    2. A和B网络直接互联创建路由成本太高，第二就是可能会造成网络冲突
    3. 通过privatelink私有网络连接成本低廉，单向的内网服务，不改变各自的网络配置

## Privatelink

- 如何创建privatelink服务

    1. privatelind endpoint service：需要关联一个SLB，此slb必须支持privatelink的SLB，slb后面就是ECS或者其他的rds等都可以

    2. privatelink endpoint 创建的是时候需要跟endpoint service 连接 一旦创建并且链接成功，那么privatelink就可以生效了

    3. privatelink endpoint 有一个DNS地址，我们可以通过地址直接访问，此地址是aliyun内网地址


