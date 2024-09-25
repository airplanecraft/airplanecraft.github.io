+++
Categories = ["Technology"]
Description = "Ansible brief introduction"
tags = ["devops", "jenkins"]
date = "2015-05-06T11:40:00+08:00"
menu = "jenkins"
title = "jenkins简介"
toc = true
+++

## 什么是CI和CD ##

    CI(Continuous integration，中文意思是持续集成)/  CD(Continuous Delivery， 中文意思持续交付)
    CI/CD 是一种通过在应用开发阶段引入自动化来频繁向客户交付应用的方法。
    CI/CD 的核心概念是持续集成、持续交付和持续部署。它是作为一个面向开发和运营团队的解决方案，主要针对在集成新代码时所引发的问题（也称为：“集成地狱”）。
    CI/CD 可让持续自动化和持续监控贯穿于应用的整个生命周期（从集成和测试阶段，到交付和部署）。

## jenkins简介 ##

        Jenkins是一个开源的、提供友好操作界面的持续集成(CI)工具，起源于Hudson（Hudson是商用的），主要用于持续、自动的构建/测试软件项目、监控外部任务的运行（这个比较抽象，暂且写上，不做解释）。Jenkins用Java语言编写，可在Tomcat等流行的servlet容器中运行，也可独立运行。通常与版本管理工具(SCM)、构建工具结合使用。可集成使用版本控制工具有SVN、GIT，以及构建工具有Maven、Ant、Gradle。


## 特点 ##

1. 易于安装-只要把jenkins.war部署到servlet容器，不需要数据库支持
2. 易于配置-所有配置都是通过其提供的web界面实现；
3. 有大量的插件可以配置使用,比如email通知结果,git,maven
4. 附带了很多其他的功能比如用户密码的加密
5. 可以支持备份,迁移,升级
6. 还可以通过完全编程的方式Jenkinsfile,devops来实现纯代码的方式完全CI/CD
7. 可以支持分布式的多个slave agent
8. 可以保存每次build日志方便查阅

## CI/CD 优势 ##

    1. 持续集成中的任何一个环节都是自动完成的，无需太多的人工干预，有利于减少重复过程以节省时间、费用和工作量
    2. 持续集成保障了每个时间点上团队成员提交的代码是能成功集成的。换言之，任何时间点都能第一时间发现软件的集成问题，使任意时间发布可部署的软件成为了可能
    3. CI/CD 具有高度的自动化 就是减少人工操作的事务，可以通过预先准备的脚本一次性运行，是我们内部或者外部用户，得到用户对于新版本的快速反馈，并且可以迅速处理任何明显的缺陷.

## 放几张图 ##


![cicd](/images/cicd.png)


![jenkins](/images/jenkins.png)
