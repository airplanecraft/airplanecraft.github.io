+++
Categories = ["Technology"]
Description = ""
Tags = ["vpn","linux"]
date = "2016-05-04T10:30:37+08:00"
menu = "saa"
title = "vpn客户端工具pptp介绍"
toc = true
+++


## pptp 安装
 -  yum install ppp pptp pptp-setup -y
## 创建 链接文件
 - /etc/ppp/peers/pptp
  ```
    pty "pptp 039.9966.org  --nolaunchpppd --debug"
    name hk789
    password ff800800
    remotename PPTP
    require-mppe-128
    require-mschap-v2
    refuse-eap
    refuse-pap
    refuse-chap
    refuse-mschap
    noauth
    debug
    persist
    maxfail 0
    defaultroute
    #replacedefaultroute
    #usepeerdns
  ```
## 执行命令

```
 modprobe nf_conntrack_pptp
 pppd call pptp
```