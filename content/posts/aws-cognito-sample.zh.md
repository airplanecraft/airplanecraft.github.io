+++
Categories = ["Technology"]
Description = "Aws cognito sample"
tags = ["aws", "sap"]
date = "2019-09-06T10:40:37+08:00"
menu = "sap"
title = "Aws cognito sample"
toc = true
+++


- 这是一个aws官方的文档，我认为最好的cognito的例子，如果理解这个sample，那么cognito就没有任何问题
- [https://aws.amazon.com/getting-started/projects/build-serverless-web-app-lambda-apigateway-s3-dynamodb-cognito/](https://aws.amazon.com/getting-started/projects/build-serverless-web-app-lambda-apigateway-s3-dynamodb-cognito/)


#### 详解 ####

- 廢話少說直接上圖
  
  ![https://d1.awsstatic.com/Test%20Images/Kate%20Test%20Images/Serverless_Web_App_LP_assets-16.7cbed9781201a79b9efa761807c4312e68b23485.png](https://d1.awsstatic.com/Test%20Images/Kate%20Test%20Images/Serverless_Web_App_LP_assets-16.7cbed9781201a79b9efa761807c4312e68b23485.png)

#### 程序組成部分 ####

    - 靜態的代碼存放s3
    - 動態的代碼存放lambda
    - 代碼的訪問入口通過api-gateway
    - 數據的存放時dynamoDB(程序登錄後有個點擊頁面位置生成的數據,與用戶驗證沒有太多關係)
  
#### Cognito user pool 在user login 流程 ####

   ![https://docs.aws.amazon.com/cognito/latest/developerguide/images/scenario-authentication-cup.png](https://docs.aws.amazon.com/cognito/latest/developerguide/images/scenario-authentication-cup.png)

    - 用戶用用戶名+密碼(或mfa，這取決於cognito user pool的配置)請求登錄
    - aws cognito user pool去驗證用戶
    - 如果用戶通過驗證返回一個token
    - 下一次用戶用這個token來請求訪問
    - aws cognito user pool 可以基本满足用户登录，如果涉及到权限可以跟自己数据进行绑定

    - 这个例子里面特别要注意的是，api gateway 直接设置了 Authorizers ：里面可以指定cognito 来对用戶進行驗證，不用額外寫程序來驗證，也就說每次訪問一個服務/ride.html,api-getway通過Authorizers直接去用cognito去驗證用戶的token

    - 這個例子分了兩個步驟，第一個步驟是直接登錄，登錄後獲得一個toke
    - 第二步驟是用api-gateway 自動驗證用戶的登錄，也就是authentication



  

