### mongo知识总结
* 1.mongo追加元素$push
```
db.getCollection('WFCModule').update({key:"LZARCC"},{ $push: { fields: {'name':'FamilyId','type':'custom','label':'机构id','searchable':true} } })
db.getCollection('WFCModule').update({key:"LZARCC"},{ $push: { fields: {'name':'FamilyName','type':'custom','label':'机构名称','searchable':true} } })
```
* 2.mongo查询时候匹配字段类型的问题
当mongodb查询的时候，如果字段不存在的时候，使用$set方法会建立新的字段，如果原来的字段存在，那么这个字段，就不会新建。只会update
db.WFCProcess.update({"id":1,{$set:{'fileds.nickname.value':1}}}),这个时候，如果这个字段不存在，那么才会重新建立这个字段
mongodb的一系列的操作
操作文档的路径：
https://docs.mongodb.com/manual/reference/operator/update/push/
* 3.mongo更改数组某个字段的数值
```
db.WFCProcess.find({'fields.nickname.value': {$exists: true}}).forEach(function(data) {
                   data.fields.nickname.value = data.fields.nickname.value.toString()
                  db.WFCProcess.save(data)
 })
 ```
* 4.php-mongodb的查询
对于mongodb的模糊查询
https://docs.mongodb.com/manual/reference/sql-comparison/ 这一块对于的mysql到mongo的查询的映射的关系有了不错的了解
还有对于mysql的对应关系如何进行应对
```
eg1:
select * from user where (uid =1 or name ="chao") AND loginTime = "2016-06-14"
转化为mongodb的查询
$where = [
'$or' => [
[“uid” => 1],
["name" => "chao"]
],
"loginTime " => "2016-06-14"
```
];查询条件就像上边的这个样子的
eg2:
* mongodb的in操作
select * from user where uid in (1,2,3);
转化为mongodb的查询
$where = [
'uid' => [
“$in” => [1,2,3]
]
];
* eg3:
mongodb的like查询
select * from user where uid like "%bc%"
转化为mongodb的查询
$where = [
'uid' => [
“$regex” =>"bc"
]
];