+++
Categories = ["Technology"]
Description = "Why openshift"
Tags = ["service mesh","PAAS","kubenetes"]
date = "2022-11-23T12:40:37+08:00"
menu = "openshit"
title = "企业为什么选择使用openshift"
toc = true
+++

## 什么是openshift
OpenShift是红帽的云开发平台即服务（PaaS）。自由和开放源码的云计算平台使开发人员能够创建、测试和运行他们的应用程序，并且可以把它们部署到云中。OpenShift广泛支持多种编程语言和框架，如Java，Ruby和PHP等。另外它还提供了多种集成开发工具如Eclipse integration，JBoss Developer Studio和 Jenkins等。OpenShift 基于一个开源生态系统为移动应用，数据库服务等，提供支持。
OpenShift Online服务构建在Red Hat Enterprise Linux上。Red Hat Enterprise Linux提供集成应用程序，运行库和一个配置可伸缩的多用户单实例的操作系统，以满足企业级应用的各种需求。


## 架构图

![PrivateLink](/images/kubernetes-architecture.png)

## 核心的概念

 https://zhuanlan.zhihu.com/p/635160126?utm_id=0

 容器（Container）
 镜像（Image）
 用户（User）
 项目（Project）
 容器沙箱（Pod）
 部署（Deployment）
 服务（Service）
 路由（Router）
 持久化存储（Persistent Storage）
 模板（Template）
 构建（Build）和镜像流（ImageStream）

## 为什么要使用openshift

OpenShift的持续集成和持续部署（CI/CD）功能可以帮助开发者快速地将代码更改合并到主分支，并通过自动化的方式将其部署到生产环境，这不仅可以提高开发效率，还可以减少人为错误，提高软件的质量。

OpenShift的多租户支持功能使得不同的团队或者公司可以在同一个平台上共享资源，而不需要担心数据安全问题，每个租户都可以拥有自己的独立的应用程序、服务和数据库，这样就可以满足不同团队的需求。

OpenShift的应用市场功能提供了一个方便的平台，让开发者可以找到并使用各种预先构建的应用程序和服务，而不需要自己从头开始编写，这样不仅可以节省时间，还可以提高开发效率。

OpenShift的自动扩展功能可以根据应用程序的负载情况自动调整资源的分配，以确保应用程序的性能和可用性，这样可以避免因为资源不足而导致的应用程序崩溃，也可以在需求增加时快速地扩展应用程序的能力。

OpenShift的优势在于其简单易用、功能强大和灵活可扩展的特性，它可以帮助开发者快速地部署和管理应用程序，提高开发效率，降低运维成本。    






