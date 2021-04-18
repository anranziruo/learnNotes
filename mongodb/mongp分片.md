### 分片
建立分片
分片：是指将数据进行切分，将其分散到不同机器上的过程。mongodb自动处理数据在分片上的分布，容易添加和删除分片。
使用分片的连接，
![示例图1](https://github.com/zhangchao1/learnNotes/blob/master/assets/mongodb/fenpian_connect.png)
mongos处理客户端到达的请求，然后获取将请求均匀到达分片。使用集群架构可以使用程序获取了更大的数据处理能力。
#### 建立简单的集群
cluseter = new ShardingTest({"shard":3,"chunksize":1})//创建三个分片的mongos的集群
一般来说，主分片是随机选择的
* 1.如果要对结合分片，首先要对数据库分片
sh.enableSharding("test")
* 2.然后选择字段进行创建索引
* 3.然后选择集合进行分片
#### 分片一句片键范围将集合拆分为多个数据快。有时候分片查询定为到集群分片的子集，这是定向查询。
有些查询必须发送到所有分片，这种叫做分散-聚集查询
配置分片
分片增加了可用RAM,增加了单个mongodb无法承受的吞吐量，减少了单台服务器的负载
#### 创建集群
* 1.启动所有所需进程
* 2.建立mongos和分片
* 3.配置对应的服务器
#### 如何配置服务器呢？
首先确保配置服务器先于任何mongos启动，配置服务器使用的是独立的mongos进程
备注：复制服务器不是副本集成员
当确认三台配置服务器处于运行状态以后，我们可以启用 一个mongos进行程序连接，然后mongos负责分发请求到分片
如果存在副本集，可以将副本集转换成分片，使用sh.addShard()将副本集转换成分片。
#### 增加集群容量
使用sh.addShard()指令增加集群容量。
mongo如何追踪集群数据
#### 块的使用
mongo可以通过给定的片键找到文档的存放位置，mongo将文档分为块(chunk).数组是不可以作为片键的
![示例图1](https://github.com/zhangchao1/learnNotes/blob/master/assets/mongodb/chunk.png)
chunk的容量都是会有默认值的，系统默认的是64MB
#### 拆分块
mongo在每个快插入数据，一旦到达阀值，就会拆分数据
![示例图1](https://github.com/zhangchao1/learnNotes/blob/master/assets/mongodb/insert_chunk.png)
如图所示
mongos负责在服务器更新这个块的元信息，块拆分的时候只需要改变块的元数据即可，进行拆分的时候，配置服务器会创建新的文档
#### 均衡器
mongos成为均衡器，会检查每个集合的分块表，检查分片是否到达了均阀值。不均衡的表现是一个分片比其他分片拥有更多的快。然后均衡器会块进行均匀分布，使得每个分片拥有更数量相同的块
#### 选择片键
 对集合进行分片的时候，要选择一个或者两个字段用于拆分数据，这个键就是片键 
拆分数据最通用的数据方式有三种，升序片键，随机分发的片键，基于位置的片键。
#### 升序片键
升序片键根据某种排序，比如说时间进行排序,根据片键的顺序进行排序，然后mongodb根据不同的分片范围进行拆快
![示例图1](https://github.com/zhangchao1/learnNotes/blob/master/assets/mongodb/sort_key.png)
#### 随机分发的片键
随机分发的片键比如说用户名，邮箱地址，MD5散列值。随机分发的片键是没有规律的键值
#### 基于位置的片键
基于位置的片键是用户IP,经纬度，或者地址
![示例图1](https://github.com/zhangchao1/learnNotes/blob/master/assets/mongodb/pos_key.png)
如果希望特定范围的块出现在特定的分片，可以给分片添加tag。然后给块指定相应的tag
sh.addShardTag("shard0000", "NYC")
请求均衡器实现该指令，然后创建区域范围
sh.addTagRange("records.users", { zipcode: "10001" }, { zipcode: "10281" }, "NYC")
#### 散列片键
散列片键没有办法进行进行范围查询，散咧片键是数据加载的速度的极致
### 手动分片
手动分片需要关闭均衡器，使用moveChunk()
分片管理
* 1.可以使用sh.status()检查分片的状态
* 2.集群相关的配置信息在config数据库当中。使用db.shards.findone()
* 3.config.databases;记录了所有的数据库的信息
* 4.changelog可以记录跟踪记录集群的操作
如何查看mongos和mongod的连接信息
![示例图1](https://github.com/zhangchao1/learnNotes/blob/master/assets/mongodb/con_status.png)
#### 我们可以限制mongos到mongod的连接数量，设置最大的连接数
最大的连接数=mongos进程数×3-副本集数量x3-其他mongos的进程的数量
服务器管理
#### 添加服务器
sh.addShard( "rs1/mongodb0.example.net:27018" )
修改分片的服务器
修改分片的服务器：直接连到服务器上，不用通过mongos,然后对副本集，集群配置会自动检测更改。然后将其更新到config.shards
#### 删除分片
db.adminCommand( { removeShard: "mongodb0" } )
