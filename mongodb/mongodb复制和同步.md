### 复制和同步
mongodb复制有副本集和主从复制两种模式，但是现在mongodb3.6以后对于主从复制不再使用了。\
备份节点数据可能会落后于主节点，备份节点默认会拒绝读取请求，防止应用程序拿到过期的数据或者不能及时读取到数据
如果要读取数据，我们需要在对于备份节点的连接进行设置，设置可以读取数据
使用setSlaveOk即可
#### 副本集的模式如下:
![示例图1](https://github.com/zhangchao1/learnNotes/blob/master/assets/mongodb/fuben.png)
副本集是一组服务器，主服务器(Primary)用于处理客户端请求，备份服务器用于保存主服务器的数据副本，主服务器崩溃，备份服务器通过选举成为成为一个新的主服务器。
#### mongodb的格式
MongoDB Oplog是MongoDB Primary和Secondary在复制建立期间和建立完成之后的复制介质，就是Primary中所有的写入操作都会记录到MongoDB Oplog中，然后从库会来主库一直拉取Oplog并应用到自己的数据库中。这里的Oplog是MongoDB local数据库的一个集合，它是Capped collection，通俗意思就是它是固定大小，循环使用的。
可以使用一个demo进行测试
* 1.首先我们在根目录下建立data/db的目录，保证这个目录有读写权限。因为mongo启动的时候，dbpath的默认位置会在data/db
* 2.建立这个以后，我们开始启动mongo
* replicaSet= new ReplSetTest({"nodes":3})//先创建3个副本
* replicaSet.startSet()//启动3个mongodb进程
* replicaSet.initiate()//配置复制功能
这个时候我们要开始测试我们的复制功能
* 重新开始一个shell,
sudo /usr/bin/mongo --nodb
* 然后连接主节点
conn1 = new Mongo("localhost:20000")
pri = conn1.getDB("test")
* 连接对应的测试数据库，查看主节点是不是master节点
pri.isMaster()查看主节点的相关信息
* 然后开始在主节点插入1000条数据，for(i=0;i<1000;i++){pri.coll.insert({count:i})}，接了来去相关备份节点查看数据
* 一般来说备份节点禁止查看数据
conn2 = new Mongo("localhost:20001")连接到备份节点
sec = conn2.getDB("test") 连接备份节点的数据库
* 这个时候备份节点，需要进行设置才可以查看数据
conn2.setSlaveOk() 记住这个设置对于单个连接的
* 然后查看备份节点的数据
sec.coll.count()
* 可以清楚的看到主节点的数据已经同步到备份节点，
配置副本集
增加新的副本或者删除新的副本
rs.Add()//增加新的副本，可以理解成添加从库
rs.remove()//删除成员
* 设置副本集的非常重要的概念就是多数
* mongodb的选举机制
当备份节点和主节点无法联通的时候，然后备份节点会请求其他备份节点将自己设置成为主节点，这个时候必须要说下mongodb的选举机制。
选举机制的成员之一就是选举仲裁者，仲裁者只能那个有一个，在同一个复制的集合里面
仲裁者不会备份数据
* 同步
oplog是mongob复制的日志，但是oplog的大小是固定的，我们在开始进行备份的时候，是需要计算和设置的,副本集的成员启动以后，就会检查自身的状态，确定是否可以从那个成员进行同步，删除所有已存在的数据库
* 全量复制和增量复制
intial sync，可以理解为全量同步。
replication，追同步源的oplog，可以理解为增量同步。
### 判断全量同步及增量同步
* 如果local数据库中的oplog.rs 集合是空的，则做全量同步。
* 如果minValid集合里面存储的是_initialSyncFlag，则做全量同步（用于init sync失败处理）
* 如果initialSyncRequested是true，则做全量同步（用于resync命令，resync命令只用于master/slave架构，副本集
### mongodb副本集的心跳机制
每个成员每隔两秒钟会向其他成员发送一次心跳请求
![示例图1](https://github.com/zhangchao1/learnNotes/blob/master/assets/mongodb/herat.png)
#### 回滚机制
主要是因为主库宕机，但是新写入的数据还没有来得及同步到从库中，这个时候就会发生数据丢失的情况。
那针对这种情况，MongoDB增加了回滚的机制。在主库恢复后重新加入到复制集中，这个时候老主库会与同步源对比oplog信息，这时候分为以下两种情况：
* 1、 在同步源中没有找到比老主库新的oplog信息。
* 2、 同步源最新一条oplog信息跟老主库的optime和oplog的hash内容不同。
#### 针对上述两种情况MongoDB会进行回滚，回滚的过程就是逆向对比oplog的信息，直到在老主库和同步源中找到对应的oplog，然后将这期间的oplog全部记录到rollback目录里的文件中，如果但是出现以下情况会终止回滚：
* 对比老主库的optime和同步源的optime，如果超过了30分钟，那么放弃回滚。
* 在回滚的过程中，如果发现单条oplog超过512M，则放弃回滚。
* 如果有dropDatabase操作，则放弃回滚。
* 最终生成的回滚记录超过300M，也会放弃回滚。