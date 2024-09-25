+++
Categories = ["Technology"]
Description = "Aws congnito"
Tags = ["aws", "sap"]
date = "2019-09-06T12:40:37+08:00"
menu = "sap"
title = "Difference between user pool and identity pool(Federated Identities)"
toc = true
+++

- 剛開始使用cognito服務的時候特別讓人困惑，有user pool 和 identity pool(Federated Identities)，user pool裡面還有federation，federation裡面有identity provider.
- identity pool 裡面有autenticated provider裡面也有user pool ID!
- 納尼?這是什麼鬼?

#### let's forget the connection between them ####

  - user pool 簡單來說就是提供一個用戶驗證的服務，比如用戶自定義用戶，用fb,GOOGLE，twitter等賬戶登錄，登錄後獲取一個token，用戶的每次請求帶著這個token就可以了，用戶登錄後獲取的是你個人應用的resource! ,比如你自己做了一個網站，登錄後可以管理自己用戶，權限，圖片等等，你有權限去訪問這些資源
  - identity pool 也是提供一個用戶認證的服務，用戶可以在裡面設置aws 的role，也就說用戶登錄後得到的aws的resource 的訪問權限，比如你可以訪問s3.只是這個登錄可以跟 user pool提供的服務去綁定，也可以不用user pool的服務直接用 identity pool裡面的provider去對接，也就說，如果你有google的id也可以登錄後獲取aws resource 資源的訪問權限，這個是登錄後aws通過sts生成臨時credential 來做到的


#### summary  ####

  - user pool 只是負責authentication，沒有authorization，即便有也是用戶本身應用的服務
  - identity pool，既可以authentication，又可以authoriztion，授權的是aws的resource

#### 上幾張圖  #####

![https://docs.aws.amazon.com/cognito/latest/developerguide/images/amazon-cognito-ext-auth-basic-flow.png](https://docs.aws.amazon.com/cognito/latest/developerguide/images/amazon-cognito-ext-auth-basic-flow.png)

![https://gorillalogic.com/wp-content/uploads/2018/09/Sign-Up-Implementation-7--768x312.png](https://gorillalogic.com/wp-content/uploads/2018/09/Sign-Up-Implementation-7--768x312.png)

![https://d33wubrfki0l68.cloudfront.net/4602d3b127c9b3f1dbe49f9fc77e8d8a4aff20a6/9c3a1/assets/cognito-user-pool-vs-identity-pool.png](https://d33wubrfki0l68.cloudfront.net/4602d3b127c9b3f1dbe49f9fc77e8d8a4aff20a6/9c3a1/assets/cognito-user-pool-vs-identity-pool.png)





#### 幾個非常有價值的link ####

[https://serverless-stack.com/chapters/cognito-user-pool-vs-identity-pool.html](https://serverless-stack.com/chapters/cognito-user-pool-vs-identity-pool.html)
[https://gorillalogic.com/blog/java-integration-with-amazon-cognito/](https://gorillalogic.com/blog/java-integration-with-amazon-cognito/)

