+++
Categories = ["Technology"]
Description = "Ansible brief introduction"
tags = ["devops", "ansible"]
date = "2017-03-06T11:40:00+08:00"
menu = "ansible"
title = "Ansible简介"
toc = true
+++

## Ansible 简介 ##

     ansible是基于Python开发，集合了众多运维工具（puppet、chef）的优点，实现了批量系统配置、批量程序部署、批量运行命令等功能。最大的特点就是ansible不需要在远程主机上安装client/agents，因为它们是基于ssh来和远程主机通讯的。ansible目前已经已经被红帽官方收购，是自动化运维工具中大家认可度最高的，并且上手容易，学习简单。是每位运维工程师必须掌握的技能之一。

 ## 特点 ##

1. 部署简单，只需在主控端部署Ansible环境，被控端无需做任何操作；
2. 默认使用SSH协议对设备进行管理；
3. 有大量常规运维操作模块，可实现日常绝大部分操作；
4. 配置简单、功能强大、扩展性强；
5. 支持API及自定义模块，可通过Python轻松扩展；
6. 通过Playbooks来定制强大的配置、状态管理；
7. 轻量级，无需在客户端安装agent，更新时，只需在操作机上进行一次更新即可；
8. 提供一个功能强大、操作性强的Web管理界面和REST API接口——AWX平台。

## 任务执行方式 ##

1. ad-hoc模式(点对点模式)

    使用单个模块，支持批量执行单条命令。ad-hoc 命令是一种可以快速输入的命令，而且不需要保存起来的命令。就相当于bash中的一句话shell。
2. playbook模式

   是Ansible主要管理方式，也是Ansible功能强大的关键所在。playbook通过多个task集合完成一类功能.比如我们拷贝一个文件到主机，然后授予权限，然后启动程序这几个步骤结合在一起

## Aansbile 文件和命令 ##

1. ansible.cfg
```
  inventory = /etc/ansible/hosts		#这个参数表示资源清单inventory文件的位置
	library = /usr/share/ansible		#指向存放Ansible模块的目录，支持多个目录方式，只要用冒号（：）隔开就可以
	forks = 5		#并发连接数，默认为5
	sudo_user = root		#设置默认执行命令的用户
	remote_port = 22		#指定连接被管节点的管理端口，默认为22端口，建议修改，能够更加安全
	host_key_checking = False		#设置是否检查SSH主机的密钥，值为True/False。关闭后第一次连接不会提示配置实例
	timeout = 60		#设置SSH连接的超时时间，单位为秒
	log_path = /var/log/ansible.log		#指定一个存储ansible日志的文件（默认不记录日志）
```

2. inventory 文件

不分组
```
IP1
IP2

```
OR 主机组

```
[HOSTGRUPXX]
IP1
IP2
```

3. 命令


```
/usr/bin/ansible　　Ansibe AD-Hoc 临时命令执行工具，常用于临时命令的执行
/usr/bin/ansible-doc 　　Ansible 模块功能查看工具
/usr/bin/ansible-galaxy　　下载/上传优秀代码或Roles模块 的官网平台，基于网络的
/usr/bin/ansible-playbook　　Ansible 定制自动化的任务集编排工具
/usr/bin/ansible-pull　　Ansible远程执行命令的工具，拉取配置而非推送配置（使用较少，海量机器时使用，对运维的架构能力要求较高）
/usr/bin/ansible-vault　　Ansible 文件加密工具
/usr/bin/ansible-console　　Ansible基于Linux Consoble界面可与用户交互的命令执行工具

其中，我们比较常用的是/usr/bin/ansible和/usr/bin/ansible-playbook。
```

4. 例子

这个例子非常的简单，就拷贝ssl证书和nginx配置文件到nginx目录下面，然后启动nginx

命令
```
ansible-playbook -u root -i myinventory --private-key ${var.ssh_key_private} -T 300 provision.yml

```
myinventory 文件

```
[hostgroup]
192.168.25.10
192.168.25.11
```
provision.yml 文件
```
---
  - hosts: docker
    remote_user: root
    become: yes
    become_method: sudo
    vars:
 
    tasks:
      - name: mkdir 
        shell: |
          rm -rf /opt/cert/hoyimall
          rm -rf /etc/nginx/nginx.conf
          mkdir -p /opt/cert/xxx
          mkdir -p /opt/nginx/logs
      - name: copy nginx conf to remote
        copy:
          src: /opt/nginx_test/xxx/nginx.conf
          dest: /etc/nginx
      - name: copy key to remote
        copy:
          src: /opt/cert/xxx/5529587__xxx.com.key
          dest: /opt/cert/xxx
      - name: copy pem to remote
        copy:
          src: /opt/cert/xxx/5529587__xxx.com.pem
          dest: /opt/cert/xxx
      - name: restart 
        shell: 
          systemctl restart nginx
```
shell/copy 就是ansible 的module


## 总结 ##

    1: 优势比我们自己写shell更加简洁和方便,ansible提供了非常多的模块帮助我们不用再写shell
    2: 对用户和主机组的管理非常简单
    3: 不需要在客户端安装agent,所有的操作就是基于SSH
    4: ansible tower这样的可视化后台的支持,非常简洁
    
