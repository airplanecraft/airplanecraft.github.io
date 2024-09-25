+++
Categories = ["Technology"]
Description = "how to use terraform to deploy docker to ecs"
Tags = ["ansible", "terraform"]
date = "2020-05-29T12:40:37+08:00"
menu = "terraform"
title = "使用terraform创建阿里ecs用ansible完成主机配置"
toc = true
+++

## 使用terraform创建ecs用ansible完成ecs的provision

- 本文目标就是是用terraform创建ecs包括security group,disk,vpc,vswtich,然后用ansible来初始化和配置创建好的ecs
- 本文只是创建了一个单机ecs，后续的文章会有load balance出现

## terrorm 创建ecs ##

- 什么是terraform
   1. terraform是云工具，也就是针对云平台的
   2. terraform是在云平台上管理资源的，就是一个云资源编排工具
   3. terraform目标是"Write, Plan, and create Infrastructure as Code", 基础架构即代码。具体的说就是可以用代码来管理维护 IT 资源，把之前需要手动操作的一部分任务通过程序来自动化的完成，这样的做的结果非常明显：高效、不易出错。
- Terraform 核心功能
   1. 基础架构即代码(Infrastructure as Code)
   2. 执行计划(Execution Plans)
   3. 资源图(Resource Graph)
   4. 自动化变更(Change Automation)
   
- terraform安装
   1. 下载https://www.terraform.io/downloads.html
   2. 设置环境变量（省略）
   
- 创建terraform 配置文件
   1. main.tf
   ```
        provider "alicloud" {
        access_key = "xxx"
        secret_key = "xxx"
        region     = "ap-southeast-1"
        #version    = "~> 1.5.0"
        }
        data "alicloud_instance_types" "instance_type" {
        instance_type_family = "ecs.n1"
        cpu_core_count       = "1"
        memory_size          = "2"
        }

        resource "alicloud_security_group" "group" {
        name        = var.short_name
        description = "New security group"
        vpc_id      = alicloud_vpc.vpc.id
        }

        resource "alicloud_key_pair" "alicloud_key_pair" {
        key_name   = "terraform_test"
        public_key = "${file(var.ssh_key_public)}"
        }
        resource "alicloud_security_group_rule" "allow_http_80" {
        type              = "ingress"
        ip_protocol       = "tcp"
        nic_type          = var.nic_type
        policy            = "accept"
        port_range        = "80/80"
        priority          = 1
        security_group_id = alicloud_security_group.group.id
        cidr_ip           = "0.0.0.0/0"
        }

        resource "alicloud_security_group_rule" "allow_https_443" {
        type              = "ingress"
        ip_protocol       = "tcp"
        nic_type          = var.nic_type
        policy            = "accept"
        port_range        = "443/443"
        priority          = 1
        security_group_id = alicloud_security_group.group.id
        cidr_ip           = "0.0.0.0/0"
        }
        resource "alicloud_security_group_rule" "allow_ssh_22" {
        type              = "ingress"
        ip_protocol       = "tcp"
        nic_type          = var.nic_type
        policy            = "accept"
        port_range        = "22/22"
        priority          = 1
        security_group_id = alicloud_security_group.group.id
        cidr_ip           = "0.0.0.0/0"
        }


        resource "alicloud_disk" "disk" {
        availability_zone = alicloud_instance.instance[0].availability_zone
        category          = var.disk_category
        size              = var.disk_size
        count             = var.number
        }

        resource "alicloud_vpc" "vpc" {
        cidr_block = "172.16.0.0/12"
        }

        data "alicloud_zones" "zones_ds" {
        available_instance_type = data.alicloud_instance_types.instance_type.instance_types[0].id
        }

        resource "alicloud_vswitch" "vswitch" {
        vpc_id            = alicloud_vpc.vpc.id
        cidr_block        = "172.16.0.0/24"
        availability_zone = data.alicloud_zones.zones_ds.zones[0].id
        }
        resource "alicloud_instance" "instance" {
        instance_name   = "${var.short_name}-${var.role}-${format(var.count_format, count.index + 1)}"
        host_name       = "${var.short_name}-${var.role}-${format(var.count_format, count.index + 1)}"
        image_id        = var.image_id
        instance_type   = data.alicloud_instance_types.instance_type.instance_types[0].id
        count           = var.number
        security_groups = alicloud_security_group.group.*.id
        vswitch_id      = alicloud_vswitch.vswitch.id

        internet_charge_type       = var.internet_charge_type
        internet_max_bandwidth_out = var.internet_max_bandwidth_out

        password = var.ecs_password

        instance_charge_type          = "PostPaid"
        system_disk_category          = "cloud_efficiency"
        security_enhancement_strategy = "Deactive"
        key_name = alicloud_key_pair.alicloud_key_pair.key_name
        data_disks {
            name        = "disk1"
            size        = "20"
            category    = "cloud_efficiency"
            description = "disk1"
        }
        tags = {
            role = var.role
            dc   = var.datacenter
        }

        resource "alicloud_disk_attachment" "instance-attachment" {
        count       = var.number
        disk_id     = alicloud_disk.disk.*.id[count.index]
        instance_id = alicloud_instance.instance.*.id[count.index]
        }

   ```
   2. outputs.tf

   ```
    output "hostname_list" {
    value = join(",", alicloud_instance.instance.*.instance_name)
    }

    output "ecs_ids" {
    value = join(",", alicloud_instance.instance.*.id)
    }

    output "ecs_public_ip" {
    value = join(",", alicloud_instance.instance.*.public_ip)
    }

    output "tags" {
    value = jsonencode(alicloud_instance.instance.*.tags)
    }

   ```
   3. variables.tf
   ```
    variable "number" {
    default = "1"
    }

    variable "count_format" {
    default = "%02d"
    }

    variable "image_id" {
    default = "ubuntu_18_04_64_20G_alibase_20190624.vhd"
    }

    variable "role" {
    default = "work"
    }

    variable "datacenter" {
    default = "beijing"
    }

    variable "short_name" {
    default = "hi"
    }

    variable "ecs_type" {
    default = "ecs.n4.small"
    }

    variable "ecs_password" {
    default = "Test12345"
    }

    variable "internet_charge_type" {
    default = "PayByTraffic"
    }

    variable "internet_max_bandwidth_out" {
    default = 5
    }

    variable "disk_category" {
    default = "cloud_efficiency"
    }

    variable "disk_size" {
    default = "40"
    }

    variable "nic_type" {
    default = "intranet"
    }
    variable "ssh_key_public" {
    default     = "~/.ssh/id_rsa.pub"
    description = "Path to the SSH public key for accessing cloud instances. Used for creating AWS keypair."
    }

    variable "ssh_key_private" {
    default     = "~/.ssh/id_rsa"
    description = "Path to the SSH public key for accessing cloud instances. Used for creating AWS keypair."
    }

   ```

   4. versions.tf
   ```
    terraform {
    required_version = ">= 0.12"
    }

   ```
## 用ansible部署docker容器 ##
- ansible介绍
   1. Ansible是一个开源配置管理工具，可以使用它来自动化任务，部署应用程序实现IT基础架构。Ansible可以用来自动化日常任务，比如，服务器的初始化配置、安全基线配置、更新和打补丁系统，安装软件包等。Ansible架构相对比较简单，仅需通过SSH连接客户机执行任务即可
- ansible安装
   ```
    yum install ansible -y

   ```
- 更新配置
  ```
   vi /etc/ansible/ansible.cfg

   host_key_checking = False
  ```
- 创建ansible playbook 模板代码
  ```
    ---
      - hosts: docker
        remote_user: root
        become: yes
        become_method: sudo
        vars:
          container_name: "nginx"
          container_image: "nginx:latest"
          registry_url: "docker.io/library"
          working_dir: "/data/nginx"

        tasks:
          - name: remove container
            docker_container:
              name: "{{ container_name }}"
              state: absent
              timeout: 600
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
              timeout: 600
              restart_policy: always
              ports:
                - "80:80"


  ```
- 在terraform的ecs的instance 中使用provisoner 来支持ansible

  ```
    provisioner "remote-exec" {
        # Install Python for Ansible
        inline = ["apt update;apt install python -y;rm -rf /usr/bin/python;apt install python3-pip -y;rm -rf /usr/local/bin/pip;ln -s /usr/bin/pip3 /usr/local/bin/pip;pip install docker;apt install docker.io -y;"]
        connection {
        host = "${self.public_ip}"
        type        = "ssh"
        user        = "root"
        private_key = "${file(var.ssh_key_private)}"
        }
    }
    provisioner "local-exec" {
    command = "echo '[docker]' > ./myinventory;echo '${self.public_ip}' >> ./myinventory"
    }
    provisioner "local-exec" {
        command = "ansible-playbook -u root -i myinventory --private-key ${var.ssh_key_private} -T 300 provision.yml"
    }

  ```


## 执行 terraform ##

  ```
    terraform int
    terraform plan
    terraform apply
  ```


   