+++
Categories = ["Technology"]
Description = ""
Tags = ["openvpn","linux"]
date = "2017-05-08T12:30:37+08:00"
menu = "linux"
title = "Openvpn最简单的安装方式"
toc = true
+++

## 为什么会用到vpn
- vpn是我们经常会用到的工具，比如有些程序不允许我们在某些地区下载，有些网站我们没有办法看之类的，我们就vpn不管是pptp还是vpn无非改变都是路由问题，也就是你上网的出口的问题

## openvpn
- openvpn是一款开源vpn软件，功能非常强大，用户非常多，遵循的是openvpn协议
- 缺点搭建有些复杂，但是有第三方的脚本协助搭建会非常简单
## openvpn搭建借助于第三方脚本
- 作者git：https://github.com/Nyr/openvpn-install
```
wget https://git.io/vpn -O openvpn-install.sh && bash openvpn-install.sh


```
- 最后生成一个*.ovpn文件
- 下载并安装openvpn gui：https://openvpn.net/community-downloads/
- 用安装好的客户端打开*.ovpn就可以登录远程的vpn服务器了

