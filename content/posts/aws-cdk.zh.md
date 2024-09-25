+++
Categories = ["Technology"]
Description = "AWS cdk introduction"
Tags = ["aws", "cdk"]
date = "2021-02-26T12:40:37+08:00"
menu = "AWS"
title = "aws cdk introduction"
toc = true
+++

## What is the AWS CDK?

  The AWS Cloud Development Kit (CDK) ,a framework for defining cloud infrastructure in code and provisioning it through AWS CloudFormation.

## CDK stack

![cdkstatck](/images/awscdk.png)

## How does CDK work

  1. Build with high-level constructs that automatically provide sensible, secure defaults for your AWS resources, defining more infrastructure with less code.
  2. Use programming idioms like parameters, conditionals, loops, composition, and inheritance to model your system design from building blocks provided by AWS and others.
  3. Put your infrastructure, application code, and configuration all in one place, ensuring that at every milestone you have a complete, cloud-deployable system.
  4. Employ software engineering practices such as code reviews, unit tests, and source control to make your infrastructure more robust.
  5. Connect your AWS resources together (even across stacks) and grant permissions using simple, intent-oriented APIs.
  6. Import existing AWS CloudFormation templates to give your resources a CDK API.
  7. Use the power of AWS CloudFormation to perform infrastructure deployments predictably and repeatedly, with rollback on error.
  8. Easily share infrastructure design patterns among teams within your organization or even with the public.

## What does CDK benefit

    The AWS CDK supports TypeScript, JavaScript, Python, Java, C#/.Net, and (in developer preview) Go. Developers can use one of these supported programming languages to define reusable cloud components known as Constructs. You compose these together into Stacks and Apps.

 1. CDK提供了多种开发语言的支持
 2. CDK提供了大量的Best Practice的sample,并且都是可以运行和部署的
 3. CDK对开发者非常的友好，他是通过编程的方式去管理资源，而terrform和cloudformation是通过声明的方式json或者yaml格式,声明式缺点就是不够灵活

 4. CDK 可以写更少的code,cloudformation和terraform非常繁琐的配置

## One Best practise form AWS

    https://github.com/aws-samples/amazon-msk-java-app-cdk

## 实例解析

   1. 这个示例的架构图 ![架构图](https://raw.githubusercontent.com/aws-samples/amazon-msk-java-app-cdk/main/doc/architecture.png)

   2. 具体代码结构

      1. 用CDK创建VPC,Kafka,kafka topic ,lambda producer,Fragate consumer(spring boot docker),DynamoDB
      2. 本示例采用CDK的typescript，所以要安装npm,建议使用java8以上，我使用的ec2 yum 安装的openjdk17 
      3. 要提前安装docker和maven,我的docker Docker version 20.10.7,maven：apache-maven-3.8.4，因为用到fargate所以会创建docker容器以及用maven 编译java 代码，打工docker 容易给fargate使用
      4. 要提前初始化aws configure 配置以及cdk 初始化配置
      5. 本示例很容易运行成功，代码也非常简单，kafka的producer用typescript写的lambda服务,consumer是spring boot，所以devops代码都是typescript，代码非常的简洁，如果使用terraform的写这些devops至少需要3-4倍的代码
  3. 具体部署安装不做赘述，readme文档已经包含
      



  
