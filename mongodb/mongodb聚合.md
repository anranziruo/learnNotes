### 聚合

#### 管道
* $match用于条件过滤，
* $group用户分组
#### 支持算术操作
* $sum,$avg
#### 还有极值操作
* $max,$min,$first,$last
#### 对于操作数组
* $addToSet,$push
* $project可以提取字段，还可以重命名字段(应该在修改字段之前,使用索引)
$project可以应用于一些操作
* 1.数学表达式
* 2.日期表达式
* 3.字符串表达式
* 4.逻辑表达式
#### 管道的相关操作
* https://docs.mongodb.com/manual/reference/operator/aggregation/
```
$unwind用于讲文档中的数组切分成多个文档
示例:
{ "_id" : 1, "item" : "ABC1", sizes: [ "S", "M", "L"] }
db.inventory.aggregate( [ { $unwind : "$sizes" } ] )进行拆分以后的结果
{ "_id" : 1, "item" : "ABC1", "sizes" : "S" }
{ "_id" : 1, "item" : "ABC1", "sizes" : "M" }
{ "_id" : 1, "item" : "ABC1", "sizes" : "L" }
$sort用于排序
备注:在管道的开始阶段就应该讲更多的文档和字段过滤掉
```
#### mapReduce的操作
![示例图1](https://github.com/zhangchao1/learnNotes/blob/master/assets/mongodb/mapReduce.png)
这个图片很实用
* map ：映射函数 (生成键值对序列,作为 reduce 函数参数)。
* reduce 统计函数，reduce函数的任务就是将key-values变成key-value，也就是把values数组变成一个单一的值value。。
* out 统计结果存放集合 (不指定则使用临时集合,在客户端断开后自动删除)。
* query 一个筛选条件，只有满足条件的文档才会调用map函数。（query。limit，sort可以随意组合）
* sort 和limit结合的sort排序参数（也是在发往map函数前给文档排序），可以优化分组机制
* limit 发往map函数的文档数量的上限（要是没有limit，单独使用sort的用处不大