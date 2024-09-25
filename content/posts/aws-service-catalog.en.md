+++
Categories = ["Technology"]
Description = "service catalog"
Tags = ["aws", "sap"]
date = "2019-08-29T10:40:37+08:00"
menu = "sap"
title = "service catalog 简介"
toc = true
+++

### Aws service catalog 简介 ###

- aws service catalog 从字面上看就是服务目录,也就是说一些服务放入一组，进行统一的规划，那里一些可以进行统一的规划呢？

### 几个关键词 ###

- portfolio
- product
- constraint

### 傳統資源創建存在問題

- 從亞馬遜的blog上盜圖

- ![傳統資源創建](https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2018/10/11/usecase-example.png)

- 上圖存在的幾個問題非常明顯
- 創建了一組資源，資源與資源之間無法互訪
- EC2要訪問s3 ，那麼需要管理員授權
- 管理授權就要破壞兩個原則：service self-sufficient不滿足，最小權利原則(那就要定義允許ec2訪問s3的policy，所有的實例都可以訪問)


### 如何做到產品內部服務可以互訪，而產品外部的服務無權訪問呢？

- 從亞馬遜的blog上盜圖

- ![service catalog 創建產品](https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2018/10/11/concept.png)

- A和B是兩個portfolio ，組內可以互訪，但是A和B之間無法訪問

### 一個使用場景介紹 ###


- 從亞馬遜的blog上盜圖

- ![service catalog 創建產品](https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2018/10/03/policy_and_role_as_partition_border.png)

- 比如產品添加了一個s3-08的一個bucket，如果做到允許EC2_04訪問，而不允許其他產品或者本產品內部其他EC2訪問的呢？這就要用到 service catalog來解決


- 具體例子請看 (https://aws.amazon.com/blogs/mt/create-a-security-partition-for-your-applications-using-aws-service-catalog-and-aws-lambda/)





