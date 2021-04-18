### 基础概念
#### influxdb是一个时间序列的数据库，它的time字段就是主键。
以下主要是influxdb的主要概念
```
database, field key, field set
field value, measurement, point
retention policy, series, tag key
tag set ,tag value ，timestamp
```
#### tag:
tag不是必需的字段，但是在你的数据中使用tag总是大有裨益，因为不同于field， tag是索引起来的。这意味着对tag的查询更快，tag是存储常用元数据的最佳选择。
#### tsm:
InfluxDB的专用数据存储格式。 TSM可以比现有的B+或LSM树实现更大的压缩和更高的写入和读取吞吐量
#### field:
field是InfluxDB数据结构所必需的一部分——在InfluxDB中不能没有field。还要注意，field是没有索引的。如果使用field value作为过滤条件来查询，则必须扫描其他条件匹配后的所有值。因此，这些查询相对于tag上的查询（下文会介绍tag的查询）性能会低很多。 一般来说，字段不应包含常用来查询的元数据
#### duration:
retention policy中的一个属性，决定InfluxDB中数据保留多长时间。在duration之前的数据会自动从database中删除掉。
#### replication factor:
retention policy的一个参数，决定在集群模式下数据的副本的个数。InfluxDB在N个数据节点上复制数据，其中N就是replication factor。
replication factor在单节点的实例上不起作用
#### retention policy:
InfluxDB数据结构的一部分，描述了InfluxDB保存数据的长短(duration)，数据存在集群里面的副本数(replication factor)，以及shard group的时间范围(shard group duration)。RPs在每个database里面是唯一的，连同measurement和tag set定义一个series
当你创建一个database的时候，InfluxDB会自动创建一个叫做autogen的retention policy，其duration为永远，replication factor为1，shard group的duration设为的七天
#### 创建retention policy指令如下：
CREATE RETENTION POLICY "two_hours" ON "food_data" DURATION 2h REPLICATION 1 DEFAULT //创建默认的retention policy
创建非默认的rp
CREATE RETENTION POLICY "a_year" ON "food_data" DURATION 52w REPLICATION 1
#### 备注：
复制片参数(REPLICATION 1)是必须的，但是对于单个节点的InfluxDB实例，复制片只能设为1
说明：在步骤1里面创建数据库时，InfluxDB会自动生成一个叫做autogen的RP，并作为数据库的默认RP，autogen这个RP会永远保留数据。在输入上面的命令之后，two_hours会取代autogen作为food_data的默认RP。
#### shard:
shard包含实际的编码和压缩数据，并由磁盘上的TSM文件表示。 每个shard都属于唯一的一个shard group。多个shard可能存在于单个shard group中。每个shard包含一组特定的series。给定shard group中的给定series上的所有点将存储在磁盘上的相同shard（TSM文件）中。
#### shard duration:
shard duration决定了每个shard group跨越多少时间。具体间隔由retention policy中的SHARD DURATION决定。
例如，如果retention policy的SHARD DURATION设置为1w，则每个shard group将跨越一周，并包含时间戳在该周内的所有点。
#### shard group:
shard group是shard的逻辑组合。shard group由时间和retention policy组织。包含数据的每个retention policy至少包含一个关联的shard group。给定的shard group包含shard group覆盖的间隔的数据的所有shard。每个shard group跨越的间隔是shard的持续时间。
#### series:
influxDB数据结构的集合，一个特定的series由measurement，tag set和retention policy组成。
continuous query（CQ）:
这个一个在数据库中自动周期运行的InfluxQL的查询。Continuous query在select语句里需要一个函数，并且一定会包含一个GROUP BY time()的语法。
#### 创建指令
CREATE CONTINUOUS QUERY <cq_name> ON <database_name>
RESAMPLE EVERY <interval> FOR <interval>
BEGIN
  <cq_query>
END
#### 示例1:
CREATE CONTINUOUS QUERY "cq_advanced_every" ON "transportation"
RESAMPLE EVERY 30m
BEGIN
  SELECT mean("passengers") INTO "average_passengers" FROM "bus_data" GROUP BY time(1h)
END
这个CQ是半个小时运行一次，间隔时30m
#### 示例2:
REATE CONTINUOUS QUERY "cq_advanced_for" ON "transportation"
RESAMPLE FOR 1h
BEGIN
  SELECT mean("passengers") INTO "average_passengers" FROM "bus_data" GROUP BY time(30m)
END
这个CQ指的是，假如现在时间是8:00,这个sql会查7点到8点的数据，这个1h指的是当前向前的时间间隔，也可以理解成计算起始的时间
#### 示例三：
CREATE CONTINUOUS QUERY "cq_advanced_every_for" ON "transportation"
RESAMPLE EVERY 1h FOR 90mmeasurement
BEGIN
  SELECT mean("passengers") INTO "average_passengers" FROM "bus_data" GROUP BY time(30m)
END
这个CQ的执行间隔是30m,开始时间到当前的时间是90m
#### point:
influxDB数据结构的一部分由series中的的一堆field组成。 每个点由其series和timestamp唯一标识。
你不能在同一series中存储多个具有相同timestamp的点。 相反，当你使用与该series中现有点相同的timestamp记将新point写入同一series时，该field set将成为旧field set和新field set的并集。
时间查询语法的概念：

这个是查询的基本的时间单位

influxdb支持文本

