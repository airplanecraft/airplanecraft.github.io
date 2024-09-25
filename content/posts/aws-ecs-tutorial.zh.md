+++
Categories = ["Technology"]
Description = "Aws ecs tutorial"
Tags = ["aws", "sap"]
date = "2019-09-04T9:40:37+08:00"
menu = "sap"
title = "Aws ecs tutorial"
toc = true
+++
- Aws ecs 简单来说就是host docker container，跟K8S类似，如果用过k8s，那么ecs非常的好理解
- 本文主要按照 [Gentle Introduction to How AWS ECS Works with Example Tutorial](https://medium.com/boltops/gentle-introduction-to-how-aws-ecs-works-with-example-tutorial-cea3d27ce63d) 搭建

### 关于ECS的专业的词汇 ###

- Task Definition ：实际就是要给launch configration,比如暴露端口号，用什么docker image，cpu 内存需要多少，运行docker 的command，环境变量

- Task  ：简单来说就是一个running instance
- Service ：一组task
- Cluster ：一组task 跑在一个或者多个 constainer 里面
- Container Instance  ：容器实例里面跑的是多个 task

![看图](https://miro.medium.com/max/668/1*k29gxIwwhDaP-Ge-G-yXCQ.png)

### 按照此图搭建一组ecs的服务 ###

- 创建一个ecs cluster
- 创建一个ecsServiceRole
- 创建Task Definition
- 创建elb和 target group
- 创建 service，里面只有一个task
- 检查运行情况
- 把service里面的task 改为4
  

#### 创建一个ecs cluster ####

- 创建secrutiry group my-ecs-sg
  ```
  aws ec2 create-security-group --group-name my-ecs-sg --description my-ecs-sg
  ```
- 创建ecs cluster：选择vpc subnet多个
  

#### 创建 ecsServiceRole ####

- attach policy ：AmazonEC2ContainerServiceRole 
- trusted relationship：
```
  {
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ecs.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

#### 创建 task defination ####


```
task-definition.json
{
  "family": "sinatra-hi",
  "containerDefinitions": [
    {
      "name": "web",
      "image": "tongueroo/sinatra:latest",
      "cpu": 128,
      "memoryReservation": 128,
      "portMappings": [
        {
          "containerPort": 4567,
          "protocol": "tcp"
        }
      ],
      "command": [
        "ruby", "hi.rb"
      ],
      "essential": true
    }
  ]
}

```
```
    aws ec2 authorize-security-group-ingress --group-name my-ecs-sg --protocol tcp --port 1-65535 --source-group my-elb-sg  --vpc-id vpc-xxxmyid

```

#### 创建elb 和 target group ####

- create： my-elb  with a HTTP protocol and Port 80
- 为elb 配置 security group： my-elb-sg ，inbound allowed port 80 and source 0.0.0.0/0
- 为my-ecs-sg 配置 inbound security group,允许来自elb的请求  aws ec2 authorize-security-group-ingress --group-id sg-xxxyyy --protocol tcp --port 1-65535 --source-group sg-xxxxx
  
#### 创建 service ####

  ```
  ecs-service.json
  {
    "cluster": "my-cluster",
    "serviceName": "my-service",
    "taskDefinition": "sinatra-hi",
    "loadBalancers": [
        {
            "targetGroupArn": "FILL-IN-YOUR-TARGET-GROUP",
            "containerName": "web",
            "containerPort": 4567
        }
    ],
    "desiredCount": 1,
    "role": "ecsServiceRole"
  }

   aws ecs create-service --cli-input-json file://ecs-service.json

 ```


#### 检查运行情况 ####

- 找到elb的dns
- 执行curl ：dns address

#### 把service的节点扩展为4个 ####

- 找到cluster 的pulibc dns
  ```
  ssh -i xxx.perm cluster-public-dns-address
  ```
- 进入ecs 的container 
- 执行docker ps -a 发现有4个容器在running





