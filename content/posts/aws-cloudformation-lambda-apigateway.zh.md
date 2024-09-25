+++
Categories = ["Technology"]
Description = "Aws cloudformation deploy lambda and apigateway"
Tags = ["aws", "sap"]
date = "2019-09-05T12:40:37+08:00"
menu = "sap"
title = "Aws cloudformation deploy lambda and apigateway"
toc = true
+++

- 用Cloudformation 来部署 java 的lambda 和api gateway,这里要用到spring-boot写java 代码，需要用到s3，存放lambda代码，需要创建lambda function和api agetway，还需要用到sam


### Aws lambda with Spring Boot ###

- 本文用到的git代码来自 [https://github.com/gemerick/spring-boot-lambda](https://github.com/gemerick/spring-boot-lambda)
- 本文的内容来自 [https://keyholesoftware.com/2018/04/26/aws-lambda-with-spring-boot/](https://keyholesoftware.com/2018/04/26/aws-lambda-with-spring-boot/)
 
  

### 本文的步骤 ###

- 安装sam
- git 克隆现有代码
- 创建s3 bucket
- 用cloudformation 上传 代码
- 用cloudformation 部署
- 测试代码

#### 安装sam ####

```
pip3 install --user aws-sam-cli
```
#### git 克隆代码 ####

```
git clone https://github.com/gemerick/spring-boot-lambda -b lambda

mvn clean package

```

#### 创建s3 bucket ####

```
aws s3 mb s3://spring-boot-lambda-20100905

```

#### 拷贝jar 到S3 ，更新sam的template ####

```
aws cloudformation package --template-file sam.yaml --output-template-file target/output-sam.yaml --s3-bucket spring-boot-lambda-20100905
```

#### Deploy a Cloudformation stack from the SAM template ####

```
aws cloudformation deploy --template-file target/output-sam.yaml --stack-name spring-boot-lambda --capabilities CAPABILITY_IAM

aws cloudformation describe-stacks --stack-name spring-boot-lambda
```


#### 测试部署结果 ####

```
curl   https://xxx.execute-api.ap-southeast-1.amazonaws.com/Prod/languages

[{"name":"node"},{"name":"java"},{"name":"python"}]
```




  
