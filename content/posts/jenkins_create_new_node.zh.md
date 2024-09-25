+++
Categories = ["Technology"]
Description = "how to confige jenkins nodes"
Tags = ["jenkins","pipeline"]
date = "2018-10-13T10:40:37+08:00"
menu = "jenkins"
title = "如何配置jenkins新节点"
toc = true
+++

### jenkins 节点 简介 ###

- 在企业里面使用Jenkins自动部署时，大部分企业更新发布几个软件版本,但是对于一些公司有非常多的业务线或者产品来说，Jenkins就需要同时处理很多的任务，这时候就需要借助Jenkins多个node或者我们所说的Jenkins分布式SLAVE节点，来分开处理多个产品或者业务线的部署，目的就是master节点用来分配任务，slave节点来具体执行部署任务

### 配置 slave节点 ###

- jenkins ->manage jenins -> manage node and cloud ->new node
- node name:test ,选择permanent node
- 填好number of executors:1 ,remote root,launch method:launch agent via ssh,Host,Credentials,	Host Key Verification Strategy,known hosts file verification strategy ,availability:keep agent online as much as possible
- 在slave上安装各种需要软件
- 一定要安装相关的plugin和global tool configuration里面配置相关的工具软件
e
- 创建一个pipeline测试一下

```
pipeline {
    agent any
    stages {
        stage('Example') {
            steps {
                echo 'Hello World'
            }
        }
    }
    post {
        always {
            echo 'I will ........!'
        }
    }
}
```