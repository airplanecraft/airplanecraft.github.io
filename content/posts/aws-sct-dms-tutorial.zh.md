+++
Categories = ["Technology"]
Description = "Aws sct and dms"
Tags = ["aws", "sap"]
date = "2019-09-03T14:40:37+08:00"
menu = "sap"
title = "aws数据库迁移工具简介"
toc = true
+++

### Aws数据库迁移工具简介 ###

- 越来越多的公司选择把服务迁移到云上,那么数据库的迁移尤其重要，亚马逊提供了数据迁移的工具 sct（schema converstion tool） 和 dms （data migration service）两个工具协作帮助客户从线下数据库导入到线上数据库,source 和 target 的database可以不是一种数据库类型，比如从mysql到oracle，或者mysql 到postgresql
  
- 简单介绍一下sct，简单点来说就是数据库schema转换工具，把本地的数据库的schema转换为aws线上目标数据库的schema，然后在线上导入这个schema生成数据库,然后用DMS帮你把数据导出到aws上

### 把aws mysql RDS 数据库 导出到 aws postgresql RDS ###

- 实验的目标就是把mysql数据库迁移到postgresql，由于在aws cloud上做比较方面，所以选择了都在云上来迁移做实验

- Down load Aws schema convertion tool
- create mysql database on aws 
- create database：test and table myclass with 2 column:id name
  ```
  test 
  id  name
  1   hello
  2   kitty

  ```
- create postgresql database on aws
- Open aws sct
- File ->new project:source :RDS for mysql ,target:RDS postsgresql
- 在菜单上，connection to mysql，依次数据:server name/port/username/password,链接成功后左边位置就显示了mysql
- 在菜单上，connection to postgresql,，依次数据:server name/port/username/password,链接成功后右边位置就显示了postgresql

- 找到左边mysql Schemas -> test :右鍵 convert schema 
- 如果沒有出現問題的話右側就可以看到 test 的schema了，這時候schema 只是在sct裡面，並沒有後到postgresql數據庫

- 在右側點擊 test 右鍵 apply to database ，schema 就在數據庫生成了，可以用toad看一下

- 下一步遷移數據
- 左側 test 數據庫上，右鍵 create dms task
- task name/replication instance（這個需要aws dms 上提前建好）/source/target/migration type 等一一填好，create
- view -> database migration view ，選擇剛建好的 task，點擊 start

- 數據複製完畢後，顯示load complete,檢查 postgresql 發現了數據

  ```
  test 
  id  name
  1   hello
  2   kitty

  ```

