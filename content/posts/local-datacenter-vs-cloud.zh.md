+++
Categories = ["Technology"]
Description = "Local data center vs cloud"
Tags = ["Datacenter","cloud"]
date = "2021-02-02T10:00:30+08:00"
menu = "terraform"
title = "本地数据中心和云的对比"
toc = true
+++

## 本文简介

- 本来主要介绍云和本地的数据中心的对比，主要用阿里云来对比，作为aws和alicloud的认证工程师，我对云上的和自有数据中心架构系统做个详细的对比，本文，仅仅用alicloud

## 本系统简介

- 本系统主要是做美股和港股的数据处理，美股数据来源是polygon（基于websocket数据推送和restful查询api）,港股的数据来源是港交所（UDP广播）

- 本系统功能：接收美股和港股的数据，并作本地的消息存储，用消息中间件来存储交易所数据，然用以一部分数据用来做大数据实时计算，一部数据用来做持久化的存储

## 本系统的架构图

![架構圖](/images/photo_2022-02-05_22-04-06.jpg)


## 本系統本地使用硬件


| 軟件               | 狀態   |   服務器   | 數量 | 存儲 | 數量 |
| ----------------- | ------ | ------ | ------ |------ | ------ |
|Kafka              | cluster | centos8.2 | 5 |  ssd | 4 raid5 |
|Elatic search      | cluster | centos8.2 | 3 |  ssd | 4 raid5 |
|Flink              | cluster | centos8.2 | 3 |  ssd | 4 raid5 |
|Polygon subscriber | single | centos8.2  | 1 |  ssd | 4 raid5 |
|APP                | single | centos8.2  | 1 |  ssd | 4 raid5 |


## 本地數據中心存在的主要問題

### 硬件的問題
     1. 硬盤如果寫滿，可擴展性非常的差，或者説磁盤出現損壞，需要更新磁盤都會有數據丟失的風險，并且服務會中斷
     2. 網絡設備沒有redundency
     3. 如果内存不夠用都需要定制内存，然後安裝，需要消耗大量的人力物力

### 軟件的問題

     1. centos的系統的優化，包括swap,硬盤的分區，文件句柄數等等
     2. kafka flink elasticsearch 等軟件的優化需要非常專業的知識
     3. 如果要遷移kafka 或者es裏面的數據只能停止服務，然後進行數據的遷移

## 如果選擇阿里雲或者aws（這裏拿阿里雲來説）

    1. 定制vpc網絡，阿里雲網絡外網基於bgp的，vpc本身就保障HA，redundency.
    2. ECS的磁盤是scalable的，多大都是可以彈性擴展
    3. network bandwith 可以 custome，多大的可以
    4. ecs 的 cpu+mem 可以隨時升級
    5. 不需要搭建kafka es 和flink 軟件cluster平臺，阿里雲有相關的服務，都是優化過的paas，性能非常的高，還可以按需隨時升級
    6 .數據不用擔心丟失和遷移困難

## 總結
    1. 作爲云架構師，很不幸的是我目前既要寫大量的code，還要管理綫下30多台服務器，花費了大量的時間去處理很多問題
    2. 下一步要遷移服務到aws或者阿里雲上面












 
