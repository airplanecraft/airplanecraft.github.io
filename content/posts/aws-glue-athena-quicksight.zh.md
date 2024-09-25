+++
Categories = ["Technology"]
Description = "online analytical processing (or OLAP)"
Tags = ["aws", "sap"]
date = "2019-09-10T14:40:37+08:00"
menu = "sap"
title = "aws online analytical processing (or OLAP)"
toc = true
+++

- OLAP在線分析工具越來越來成熟，從開始的數據倉庫，到強大的elk，再到雲，比如aws的 gaq(glue-athena-quicksight)，當然aws也有elk在線服務
  
- gaq vs elk,實際上是一類的產品，glue提供了crawler去爬去數據，類似 logstash，athena提供查詢工具類似elasticsearch ,quicksight 跟 kibana一樣ui.gaq是aws雲端的服務，沒有辦法部署在線下，elk既可以部署在op也可以部署在cloud上

- 這篇文章來自 [https://medium.com/localz-engineering/serverless-big-data-start-here-aws-glue-athena-quicksite-4c70ecac9fe3](https://medium.com/localz-engineering/serverless-big-data-start-here-aws-glue-athena-quicksite-4c70ecac9fe3)

- 有一個圖片 ![https://miro.medium.com/max/1171/1*MiZKdlJBIC8nTTeEsPomLQ.png](https://miro.medium.com/max/1171/1*MiZKdlJBIC8nTTeEsPomLQ.png)


#### 實驗  ####

- cloudwatch agent 或者 application discovery agent 到ec2

- cloud watch agent 或者 discovery agent  sync log 到 s3

- glue裡面創建crawler ，指定s3
  
- glue裡面創建database 和table

- athena 寫sql語句查詢想要的數據

- 如果想要類似kinana那樣可視化的工具，那麼需要付費quicksight
- 完

