+++
Categories = ["Technology"]
Description = "sts"
Tags = ["aws", "sap"]
date = "2019-08-26T8:30:37+08:00"
menu = "sap"
title = "使用Aws sts创建临时credential"
toc = true
+++
### Aws sts 简介

- 简单来说就是aws通过sts服务生成临时的credential给用户使用，他们可以设置有效期，自动失效,这也是amazon best practice 里面建议的方式

### 实验准备 ###

- 创建 User
- 创建 Role
- 创建 s3 作为测试
- 为User创建sts assumeRole 的访问策略
- 在EC2上使用Aws cli 创建 历史的credential
- 访问s3

### Create an IAM user ###

-  https://console.aws.amazon.com/iam/home?region=ap-southeast-1#/users
-  input name： mystsuser 
-  access type programmatic access
-  其他保持默认

### Create role for antoher aws account ####

- https://console.aws.amazon.com/iam/home?region=ap-southeast-1#/roles
- 选择：another aws account
- Account ID: 找到上面的user 的arn，XXX就是ID  (arn:aws:iam::XXXX:user/mystsuser)
- Attach plicy
- 搜索s3，选择 AmanzonS3ReadOnlyAccess,next
- reviews and create Role，input role name：sts-s3-read-only
- 创建完毕后，Update/Modify Trust Relationships
- replace (this is the arn of the user what you created) with
 arn:aws:iam::XXXX:user/mystsuser

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "this is the arn of the user what you created"
      },
      "Action": "sts:AssumeRole",
      "Condition": {}
    }
  ]
}
```

- 创建完毕

### 为User创建sts assumeRole 的访问策略

- user页面， 选择刚才创建的用户
- add inline policy
- service :sts
- Action:write-AssumeRole
- Resource:arn xxxx(input the arn of the role you created in earlier step)
- done

### Ec2 配置刚才创建的用户的profile ###

```
 aws configure --profile stsgeneratedprofile
AWS Access Key ID [****************QL74]:
AWS Secret Access Key [****************/soA]:
Default region name [ap-southeast-1]:
Default output format [json]:

aws sts assume-role --role-arn  arn:aws:iam::xxxx:role/sts-s3-read-only --role-session-name "mytestsession"  --profile ststestprofile --DurationSeconds 3600

```
- 生成用户的有效时间为3600秒，生成以下文件

```
{
    "AssumedRoleUser": {
        "AssumedRoleId": "AROAWNQJOZK2KCBLXLQZW:mytestsession",
        "Arn": "arn:aws:sts::xxx:assumed-role/sts-s3-read-only/mytestsession"
    },
    "Credentials": {
        "SecretAccessKey": "sss",
        "SessionToken": "sss",
        "Expiration": "2019-08-26T04:26:04Z",
        "AccessKeyId": "sss"
    }
}
```
- 根据上面的文件创建新的profile

```
aws configure --profile stsgeneratedprofile 
--测出略去
--最终.aws/credential 里面

[stsgeneratedprofile]
aws_access_key_id = sss
aws_secret_access_key = sss
aws_session_token = ssss

```

- 最后测试

```

 aws s3 ls --profile stsgeneratedprofile

--output
2019-08-11 22:02:41 www.xxx.com


 aws s3 cp abc.txt s3://xxxxx --profile stsgeneratedprofile
upload failed: ./abc.txt to s3://xxxxx/abc.txt An error occurred (AccessDenied) when calling the PutObject operation: Access Denied

```








