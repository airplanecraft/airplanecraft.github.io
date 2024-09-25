+++
Categories = ["Technology"]
Description = ""
Tags = ["aws", "saa"]
date = "2019-10-03T10:30:37+08:00"
menu = "saa"
title = "SAA-CO1考试之路"
toc = true
+++

## 考试心得 ##

### 考试成绩 ###


  - 852

### 背景 ###

  - 工作用到aws不到1年
  - 有多年java开发经验
  - linux管理员,持有RHCE

### 准备周期 ###

 - 3周從準備到考試
 - 每天8-12小时

### 做过实验的知识点列表 ###

- VPN endpoint/nat gateway/internet gateway/vpn/subnet/ACL
- RDS/Aurora/Reshift/Aurora/DynamoDB (snaphost copy az or cross region,ebs type change ,autoscaling ,entryption ,backup/replica ,Multi A-z,read/write unit)

- Cloudfront (OAI,SSL certificat ,validation TTL ,pre-signed URL)
- S3 (static websit ,ACL ,bucket policy,lifecyle) 
- ec2 (Cloudwath agent install,ebs mount/unmount,snapshot,elasticip binding)
- Cloud53 (A/alias .routing policy)
- Athena on S3 SQL QUERY 
- Kenisis stream /firehouse/analytic
- Cloudwatch custom metric(ec2 momory/disk usage),alarm config ->sns/log export to s3
- CloudTrail 
- AWS config custom rule
- IAM user/role/policy/permission
- storagegateway(user ec2 as gateway,volume)
- Api gateway+lambda +cognito (hello world example) access dynamodb 
- elasticsearch + cognito for Kibana user/password
- organization
- ELB+autoscaling
- Cloudformation (using aws existing exmaple template  LAMP )
- aws mq + cognito for mq console user/password
- Application load balancer with multiple certificate configuration
  
### 体会 ###

- SAA主要是考知识面的,不够深入,學一些表面的東西，有個大概的了解，可以合理規劃一些service
- 對軟件工程師來說容易掌握，比如kafka->kenisis ,Active MQ->aws MQ, K8S->ECs ,LINUX->EC2 , Linux HA-> ELB  ，Elasticsearch -> Athena   ,mysql/replica/backup ->RDS 這些技術都有對應的地方,所以对于丰富开发经验的人来说就简单一些，容易理解

- 考试的时候有几个新的知识点没有复习到，这也正常大概有三个服务

- 三週時間很短,但是學到不少東西,每天8-12小時學習,確實讓人很崩潰的感覺,N多年沒有這樣學習了
- 每個實驗花費了很多時間，有的實驗開始不太懂，做了很多遍才體會到了
- 刷題很有必要，題目裡面的知識點是主要的，挨個弄懂就行了，答案無所謂記不記得住
-  标记了8道题，最终应该错误了10道题.

### 建議 ###

- 想考sap必須從saa開始每個知識點都做實驗，光看題庫是沒有用的
- 有些架構圖最好自己畫一下，可以更好的理解
- 10月中旬考SAP，又要繼續拼了，有同行一起討論...



