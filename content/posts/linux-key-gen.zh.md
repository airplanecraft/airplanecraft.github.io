+++
Categories = ["Technology"]
Description = ""
Tags = ["linux", "sshkey"]
date = "2016-04-03T10:30:37+08:00"
menu = "saa"
title = "centos使用ssh-key免密登录"
toc = true
+++

## centos ssh login 免密码登录

- 修改linux主机sshd配置
  
  ```
  vi /etc/ssh/sshd_config 
    RSAAuthentication yes 
    PubkeyAuthentication yes 
    AuthorizedKeysFile .ssh/authorized_keys 
  /sbin/service sshd restart 
 
  ```
- 生成密钥,包括 id_rsa私钥，id_rsa.pub公钥
  
  ```
   ssh-keygen -t rsa 

  ```
- 更改文件名

  ```
   mv ~/.ssh/id_rsa.pub ~/.ssh/authorized_keys

  ```
- 拷贝id_rsa文件内容到任何一个linux客户端,remote.pem,使用ssh登录

  ```
  ssh -i remote.pem user@host

  ```
  