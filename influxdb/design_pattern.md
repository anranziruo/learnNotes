### 设计规则如下:
#### 鼓励的设计：
Tags are indexed and fields are not indexed. This means that queries on tags are more performant than those on fields.
In general, your queries should guide what gets stored as a tag and what gets stored as a field:
* 1.Store data in tags if they’re commonly-queried meta data
* 2.Store data in tags if you plan to use them with GROUP BY()
* 3.Store data in fields if you plan to use them with an InfluxQL function
* 4.Store data in fields if you need them to be something other than a string - tag values are always interpreted as strings
#### 不要有太多的series:
tags包含高度可变的信息，如UUID，哈希值和随机字符串，这将导致数据库中的大量measurement，通俗地说是高series cardinality。series cardinality高是许多数据库高内存使用的主要原因。
设计influxdb
tag字段不能用于图表数据显示，tag是利于索引，方便数据的查询.fileds字段只能用于查询
不要把多条信息放到一个tag里面
与上述相似，将具有多条信息的单个tag拆分为多个单独的tag将简化查询并减少对正则表达式的需求。
InfluxDB是一个时间序列数据库。针对这种用例进行优化需要进行一些权衡，主要是以牺牲功能为代价来提高性能。以下列出了一些权衡过的设计见解：
* 1、对于时间序列用例，我们假设如果相同的数据被多次发送，那么认为客户端几次都是同一笔数据。
方法：通过简化的冲突解决增加了写入性能
妥协：不能存储重复数据;可能会在极少数情况下覆盖数据
* 2、删除是罕见的事情。当它们发生时，肯定会阻止大面积的旧数据的写入。
方法：限制对删除，从而增加查询和写入性能
妥协：删除功能受到很大限制
* 3、对现有数据的更新是罕见的事件，持续地更新永远不会发生。时间序列数据就是从未更新的新数据。
方法：限制对更新，从而增加查询和写入性能
妥协：更新功能受到很大限制
* 4、绝大多数写入都是接近当前时间戳的数据，并且数据按时间顺序添加。
方法：按时间顺序添加数据显着地更有效率
妥协：随机时间或时间不按升序写入点的性能要低得多
* 5、 规模至关重要。数据库必须能够处理大量的读取和写入。
+
方法：数据库可以处理大量的读取和写入
妥协：InfluxDB开发团队被迫做出权衡来提高性能
* 6、能够写入和查询数据比具有强一致性更重要。
方法：写入和查询数据库可以由多个客户端和高负载完成
妥协：如果数据库负载较重，查询返回可能不包括最近的点
* 7、许多时间序列都是短暂的。经常是时间序列，只出现了几个小时，然后消失，例如一个新的主机，开机并监控数据被写入一段时间，然后被关闭。
方法：InfluxDB善于管理不连续数据
妥协：无模式设计意味着不支持某些数据库功能，例如没有交叉表连接
* 8、没有数据点太重要了。
方法：InfluxDB具有非常强大的工具来处理聚合数据和大数据集
妥协：数据点没有传统意义上的ID，它们被时间戳和series区分开来