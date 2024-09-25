+++
Categories = ["Technology"]
Description = "how to use ansible to deploy docker to multiple host"
Tags = ["ansible", "docker"]
date = "2020-05-22T12:40:37+08:00"
menu = "sap"
title = "使用ansible在多台客户机安装docker"
toc = true
+++

## Install ansible on server ##

- Ansible是一个开源配置管理工具，可以使用它来自动化任务，部署应用程序实现IT基础架构。Ansible可以用来自动化日常任务，比如，服务器的初始化配置、安全基线配置、更新和打补丁系统，安装软件包等
- Ansible包括控制节点（Control node）也叫主机，受控节点（Managed nodes）也叫客户机,只在控制节点安装就好了

## ansible 优点 ##

- 只需要在主机上安装ansible软件，客户机不需要安装
- 通讯协议SSH协议和SFTP
- 可并行执行程序，默认情况下，forks值为5，可以按需，在配置文件中增大该值

## ansible(centos)安装和配置 ##

- yum install ansible 
  
- 为了使Ansible与客户端通信，需要使用用户帐户配置管理机和客户机。为了方便快捷安全，一般会配置证书方式连接客户机
  ```
  ssh-keygen
  ssh-copy-id ansible@node ip
  
  ```
## 使用ad-hoc 在客户执行命令，相当于命令行 ##
- /etc/ansible/hosts,最好谨慎使用hosts配置，安全问题需要解决
  
  ```
  [docker]

   192.168.25.173 ansible_ssh_port=22 ansible_ssh_user=root ansible_ssh_pass="xxx"

  ```
- 执行以下ping,命令里面的docker对应的上面文件的docker

  ```
  ansible docker -m ping

  ```
- 使用ad-hoc命令管理软件包,安装

  ```
  ansible docker -m yum -a "name=docker state=present" -b

  ```
- 使用ad-hoc命令管理软件包,设置默认启动

  ```
  ansible docker -b -m service -a "name=docker  enabled=yes"

  ```
- 使用ad-hoc命令管理软件包,启动
  
  ```
  ansible docker -b -m service -a "name=httpd state=started"

  ```

##  使用ansible-playbook 部署docker ##
   
- test-ansile.yml 文件内容

  ```
    ---
    - hosts: docker
    remote_user: docker
    become: yes
    become_method: sudo
    vars:
        container_name: "robotic"
        container_image: "test-prod:{{container_version}}"
        registry_url: "ecr.test.amazonaws.com"
        working_dir: "/data/docker_test"

    tasks:
        - name: remove container
        docker_container:
            name: "{{ container_name }}"
            state: absent

        - name: install aws cli
        shell: |
            yum install awscli -y
        - name: rm config
        shell: |
            rm -rf /root/.aws;mkdir /root/.aws
        - name: create config for aws user
        shell: |
            echo "[profile ecr]" >>/root/.aws/config
            echo "region = ap-southeast-1" >>/root/.aws/config
        - name: create credential
        shell: |
            echo "[ecr]" >>/root/.aws/credentials
            echo "aws_access_key_id = xxx"  >>/root/.aws/credentials
            echo "aws_secret_access_key = xxx"  >>/root/.aws/credentials
        - name: docker login
        shell: |
            loginstr=`aws ecr get-login --no-include-email --profile ecr`
            bash $loginstr
        - name: create working_dir directory
        file:
            path: "{{ item }}"
            state: directory
        with_items:
            - "{{ working_dir }}"

        - name: create container
        docker_container:
            name: "{{ container_name }}"
            image: "{{registry_url}}/{{ container_image }}"
            privileged: yes
            restart_policy: always
            ports:
            - "80:80"
  
  ```

- 执行ansible文件
  ```
  ansible-playbook -i /etc/ansible/hosts test-ansible.yml --extra-vars "container_version=${BUILD_NUMBER}"
  ```