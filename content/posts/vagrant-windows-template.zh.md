+++
Categories = ["Technology"]
Description = ""
Tags = ["vagrant", "vm"]
date = "2020-04-22T10:30:37+08:00"
menu = "saa"
title = "vagrant使用介绍"
toc = true
+++

## vagrant 简介

- VirtualBox 是一款开源虚拟机软件,vagrant是一个基于Ruby的工具，用于创建和部署虚拟化开发环境。它使用Oracle的开源VirtualBox虚拟化系统，使用Chef创建自动化虚拟环境。

BBC Vagrant 是基于VirtualBox创建的虚拟机，并通过Vagrant进行打包而得到的VM环境。在虚拟机中部署好开发环境并建立虚拟机和实体机的文件共享，在开发时，可以通过实体机进行文件修改，并经过虚拟机中的环境执行，从而实现不同操作系统的工作环境的轻松部署。

## 安装 vagrant和virtualbox

- 下载并安装VirtualBox（ https://www.virtualbox.org/wiki/Downloads ）。

- VirtualBox 4.3.12下载地址(windows请用此链接)：http://dlc-cdn.sun.com/virtualbox/4.3.12/index.html

- 下载并安装Vagrant（ http://www.vagrantup.com/downloads.html ）。

## 安装和定制box

- 下载windows 10 box文件。
https://app.vagrantup.com/mrlesmithjr/boxes/windows10/versions/1574780096/providers/virtualbox.box 到c:\vagrant\download

- 执行命令

```

vagrant box add win10 c:\vagrant\download\virtualbox.box

cd c:\vagrant

vagrant init win10

vagrant up  2> vagrant.log


```
- 查看模板文件c:\vagrant\Vagrantfile

```

    $script = <<-'SCRIPT'
        echo "starting wechat"
        ipconfig > c:\ip.log
        C:\Users\wechat\WeChat.exe > c:\wechat.log
        netstat > c:\netstat.log
        echo "started wechat"
    SCRIPT
    Vagrant.configure("2") do |config|

    config.vm.provider :virtualbox do |vb|
        vb.gui = true
        vb.memory = 8*1024
    end
    config.vm.hostname = 'wechat'
    config.vm.boot_timeout = 99999999
    config.vm.box = "mrl/windows10"
    config.vm.communicator = "winrm"
    #config.vm.provision :shell, path: "install-wechat.cmd", privileged: true
    config.vm.provision "file", source: "./wechat", destination: "C:\\Users\\wechat"
    config.vm.provision "shell", path: "wechat.cmd"
    
    end
   

```

- 上面的wechat不可能启动起来，只可以启动后台程序，因为winrm无法启动xwindow，所以客户端没有可能启动起来，如果需要启动起来，就需要使用rdp来连接启动

- 下面一些相关的命令

```
vagrant winrm status
vagrant winrm default --command dir  
vagrant box list
vagrant upload xxx.file 

```


