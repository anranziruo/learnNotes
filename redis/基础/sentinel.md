## 内容

### 概念
```
Redis Sentinel是一个分布式架构，其中包含若干个Sentinel节点和Redis数据节点,每个Sentinel节点会对数据节点和其余Sentinel节点进行监控，
当它发现节点不可达时，会对节点做下线标识。如果被标识的是主节点，它还 会和其他Sentinel节点进行“协商”，当大多数Sentinel节点都认为主节点不可达时，它们会选举出一个Sentinel节点来完成自动故障转移的工作，同时会将这个变化实时通知给Redis应用方。整个过程完全是自动的
```
### 功能
```
1.监控:Sentinel节点会定期检测Redis数据节点、其余Sentinel节点是否可达。
2.通知:Sentinel节点会将故障转移的结果通知给应用方。
3.主节点故障转移:实现从节点晋升为主节点并维护后续正确的主从关 系。
4.配置提供者:在Redis Sentinel结构中，客户端在初始化的时候连接的 是Sentinel节点集合，从中获取主节点信息。同时看到，Redis Sentinel包含了若个Sentinel节点，这样做也带来了两个好处:
·对于节点的故障判断是由多个Sentinel节点共同完成，这样可以有效地防止误判。
·Sentinel节点集合是由若干个Sentinel节点组成的，这样即使个别Sentinel节点不可用，整个Sentinel节点集合依然是健壮的
```
### 哨兵实现原理

#### 三个监控任务
```
监控任务1:每隔10秒，每个Sentinel节点会向主节点和从节点发送info命令获取最新的拓扑结构,每当有新的节点加入的话,都可以感知出来,同时节点故障或者节点不可用,
就可以通过info命令进行更新节点信息.
监控任务2:每隔2秒，每个Sentinel节点会向Redis数据节点的__sentinel__:hello频道上发送该Sentinel节点对于主节点的判断以及当前Sentinel节点的信息,同时每个Sentinel节点也会订阅该频道，来了解其他Sentinel节点以及它们对主节点的判断.
主要功能:
1.发现新的Sentinel节点:通过订阅主节点的__sentinel__:hello了解其他的Sentinel节点信息，如果是新加入的Sentinel节点，将该Sentinel节点信息保存起来，并与该Sentinel节点创建连接
2.Sentinel节点之间交换主节点的状态，作为后面客观下线以及领导者选举的依据
监控任务3:每隔1秒,每个Sentinel节点会向主节点、从节点、其余Sentinel节点发送一条ping命令做一次心跳检测，来确认这些节点当前是否可达。如图9- 28所示。通过上面的定时任务，Sentinel节点对主节点、从节点、其余Sentinel节点都建立起连接，实现了对每个节点的监控，这个定时任务是节点失败判定的重要依据
```
#### 主观下线和客观下线
```
主观下线:每个Sentinel节点会每隔1秒对主节 点、从节点、其他Sentinel节点发送ping命令做心跳检测，当这些节点超过 down-after-milliseconds没有进行有效回复，Sentinel节点就会对该节点做失败判定，这个行为叫做主观下线
客观下线:当Sentinel主观下线的节点是主节点时，该Sentinel节点会通过sentinel is- master-down-by-addr命令向其他Sentinel节点询问对主节点的判断，当超过 <quorum>个数，Sentinel节点认为主节点确实有问题，这时该Sentinel节点会做出客观下线的决定，这样客观下线的含义是比较明显了，也就是大部分Sentinel节点都对主节点的下线做了同意的判定,这种就是客观下线
```
#### 领导者Sentinel节点选举
```
Sentinel节点之间会做一个领导者选举的工作，选出 一个Sentinel节点作为领导者进行故障转移的工作。Redis使用了Raft算法实现领导者选举
节点选举的思路:
1.每个在线的Sentinel节点都有资格成为领导者，当它确认主节点主观 下线时候，会向其他Sentinel节点发送sentinel is-master-down-by-addr命令， 要求将自己设置为领导者。
2.收到命令的Sentinel节点，如果没有同意过其他Sentinel节点的sentinel is-master-down-by-addr命令，将同意该请求，否则拒绝。
3.如果该Sentinel节点发现自己的票数已经大于等于max(quorum， num(sentinels)/2+1)，那么它将成为领导者。
4.如果此过程没有选举出领导者，将进入下一次选举。
5.每个Sentinel节点只有一票，所以当s2向s1和s3索要投票时，只能获取一票
```
#### 故障转移
```
1.过滤:“不健康”(主观下线、断线)、5秒内没有回复过Sentinel节 点ping响应、与主节点失联超过down-after-milliseconds*10秒
2.选择slave-priority(从节点优先级)最高的从节点列表，如果存在则返回,不存在则继续
3.选择复制偏移量最大的从节点(复制的最完整)，如果存在则返回，不存在则继续
4.选择runid最小的从节点
5.sentinel领导者节点会对第一步选出来的从节点执行slaveof no one命令让其成为主节点
6.Sentinel领导者节点会向剩余的从节点发送命令，让它们成为新主节点的从节点，复制规则和parallel-syncs参数有关
7.Sentinel节点集合会将原来的主节点更新为从节点，并保持着对其关注，当其恢复后命令它去复制新的主节点
```



