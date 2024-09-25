+++
Categories = ["Technology"]
Description = ""
Tags = ["aws", "sap"]
date = "2019-08-25T16:30:37+08:00"
menu = "sap"
title = " 使用Aws System Manager 统一管理 aws resource "
toc = true
+++

## 使用Aws System Manager 统一管理 aws resource ##

### Aws system Manager 简介 ###

- AWS Systems Manager gives you visibility and control of your infrastructure on AWS. Systems Manager provides a unified use:qr interface so you can view operational data from multiple AWS services and allows you to automate operational tasks across your AWS resources

- Aws system manager AWS  让您能够查看和控制 AWS 上的基础设施。Systems Manager 可以提供一个统一的用户界面，供您查看多种 AWS 服务的运行数据，并在 AWS 资源上自动执行操作任务。

### 使用场景 ###

- 比如我有个20台 linux ec2，不管什么类型的，只要安装centyos，那么都要给他安装 ，cloudwatch agent 来对系统的disk和memory进行监控


### 实验的前置条件 ###

- 你的系统上必须安装SSM Agent
- 你的EC2的role 必须具有 AmazonEC2RoleforSSM 的policy attach上去

### 安装SSM agent ###



```
Intel (x86_64) 64 位实例：

sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm

ARM (arm64) 64 位实例：

sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_arm64/amazon-ssm-agent.rpm
Intel (x86) 32 位实例：

sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_386/amazon-ssm-agent.rpm


sudo systemctl start amazon-ssm-agent

```


### 给EC2 创建一个role具有 AmazonEC2RoleforSSM 的policy

- 创建role  ssmrole，选择policy AmazonEC2RoleforSSM

- attach ssmrole to Ec2 ，这个不具体说了


### 用System manager 执行 command 安装 aws cloud watch agent

- 打开https://ap-southeast-1.console.aws.amazon.com/systems-manager

- 点击左侧run command

- command document 里面搜索 AWS-RunShellScript

- 下方出现command parameter ，输入 


```
  sudo yum install https://s3.amazonaws.com/amazoncloudwatch-agent/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm -y;

  sudo systemctl start amazon-cloudwatch-agent;


```

- Targes 里面 --> choose instance manually,批量选择20个要安装的ec2 instance

- 执行 RUN

- 查看执行结果就成功了。





