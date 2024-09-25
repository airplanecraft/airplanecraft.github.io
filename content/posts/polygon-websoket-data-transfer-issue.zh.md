+++
Categories = ["Technology"]
Description = "how to use privatelink to connect the serivce between 2 vpc without public IP adress"
Tags = ["kafka","polygon","websocket","producer"]
date = "2021-11-20T11:40:30+08:00"
menu = "terraform"
title = "Polygon在websoket的连接串数据的经常中断"
toc = true
+++

## 本来使用工具或者服务

- Pologon 美股数据供应商
- Kafka
- Nettty
- Centos8.2

## 架构图

![polygon](/images/photo_2022-02-03_22-18-05.jpg)

## 架构说明

- Polygon是美股的数据供应商,提供美股tick级别的数据服务
- 美股开盘数据非常的大，平时可以达到6M/秒的传输速度，也就是说可以达到每秒6万条数据的传输速度
- 数据的传输是基于internet的传输，所以要保障数据在互联网上的快速传输必须基于长连接来实现，因此polygon提供了websocket服务
- 我们的app收到polygon websocket推送过来的数据要进行简单的解析和格式转换，然后发送到本地的kafka cluster中间件用来分发数据，给其他的应用来做大数据处理包括flink等大数据中间件

## 出现的问题

- Polygon数据传输出现了频繁的中断1：Slow consumer 
- slow consumer:https://polygon.io/docs/stocks/ws_getting-started 查看了一下polygon的技術文檔: if a client is consuming messages too slowly for too long, Polygon.io's server-side buffer may get too large. If that happens, Polygon.io will terminate the WebSocket connection. You can check your account dashboard to see if a connection was terminated as a slow consumer. If this happens to you consistently, consider subscribing to fewer symbols or channels. 就是你的輸出消費的太慢了，導致polygon server的buffer已經滿了，消費不掉

## 找出問題

- 肯定我們消費者太慢出現的問題
- 屏蔽掉任何發送kafka，直接打印日志就不會出現這個問題，定位到kafka client 發送速度慢的問題
- 發送慢的原因可能是1：文件句柄數夠不夠?2:cpu和内存的使用率是否太高？3:java jvm的gc是否很慢？4：kafka連接池的數量是否足夠5：kafka客戶端發送是否正確配置批量？

## 具體去找問題
  1. 
   ```
   查看進程的當前的連接數
   ls  /proc/`jps |awk '{print $1}'`/fd/ |wc -l
   ```
  2. 由於我們使用prometheus監控系統，發現cpu利用率低於20%，内存低於30%，同時使用jconsole鏈接查看jvm也沒有發現問題
  3. 查看jconsole發現大量的kafka客戶端鏈接池出現block等待的情況，從這裏可以發現應該發送kafka的速度太慢導致的

## 具體解決

```

spring.kafka.producer.buffer-memory=33554432
spring.kafka.producer.batch-size=51200
spring.kafka.producer.properties.linger.ms=200


```
加大batch-size到50K，linger.ms到200ms ，測試兩天晚上數據，一切正常

