### clickHouse集群建表

#### 单集群操作
分区键（partition by  **) 务必使用yyyy-MM-dd格式日期
建表应当按照如下格式建一张原表，一张"_all"后缀的分布式表，数据写入使用原表（不带”_all"后缀的表），数据查询使用分布式表（"_all"后缀的表，否则查询不能将所有节点的数据聚合，导致数据不全）
```
CREATE TABLE database.table on cluster cluster
(
    `datetime` DateTime,
    `date`     Date,
    `year`     UInt32,
    `cmd`      String,
    `action`   String
) ENGINE = MergeTree PARTITION BY date ORDER BY cmd SETTINGS index_granularity = 8192
CREATE TABLE database.table_all on cluster cluster
(
    `datetime` DateTime,
    `date`     Date,
    `year`     UInt32,
    `cmd`      String,
    `action`   String
) ENGINE = Distributed(cluster, database, table, rand())
注意：Distributed（）中的表名称为第一张表的表名称，不要带"_all"
```
说明：上面语句会在单副本集群cluster上的database库下创建名为table，table_all两张表,其中table表为每个节点上的表，“_all"后缀的为分布式表，查询时务必使用分布式表查询数据。【sql中的database、table、cluster均为变量】
#### 双副本集群建表
```
CREATE TABLE database_name.table_name  on    cluster cluster_name
(
    `task_id`     Int64,
    `task_key`    String,
    `details`     String,
    `date`        Date,
    `create_time` DateTime
) ENGINE = ReplicatedMergeTree('/clickhouse/tables/database_name/table_name/{shard}',
           '{replica}') PARTITION BY toDate(date) ORDER BY (create_time) SETTINGS index_granularity = 8192
 
 
CREATE TABLE database_name.table_name_all  on  cluster cluster_name
(
    `task_id`     Int64,
    `task_key`    String,
    `details`     String,
    `date`        Date,
    `create_time` DateTime
) ENGINE = Distributed(cluster_name,database_name,table_name)
注意：Distributed（）中的表名称为第一张表的表名称，不要带"_all"
```
说明：上面语句会在双副本集群上创建table_name ,table_name_all两张表。table_name插入时使用，table_name_all查询时使用。【sql中database_name,table_name ,cluster_name 均为变量】
#### 查询
请使用 "_all"后缀的分布式表进行查询。语法类似mysql。