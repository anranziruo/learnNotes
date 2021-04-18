### mongo索引

#### 1.复合索引和普通索引
我们首先插入200万条数据，作为样本数据测试。
脚本是：
for(var i=0;i<2000000;i++){ db.users.insert({ "i":i, "username":"user"+i, "age":i, "created":new Date() } );}
首先我们不加索引的运行时间分析:
![索引示例图1](https://github.com/zhangchao1/learnNotes/blob/master/assets/mongodb/mongoindex.png)
会发现运行比较慢
db.users.ensureIndex({"age":1})
建立索引之后，会发现运行速度提升了很多。
![索引示例图2](https://github.com/zhangchao1/learnNotes/blob/master/assets/mongodb/mongoindex2.png)
但是我们如果对两个字段进行排序的话，就会有不同。
```
db.users.find().sort({"age":1,"username":1}).limit(20)
会发现特别慢，这个时候就需要使用联合索引了。
db.users.ensureIndex({"username":1,"age":1})指令就可以建立索引了。
db.users.find().sort({"age":1,"username":1}).limit(20)会发现这个查询并没有命中。
db.users.find().sort({"username":1,"age":1}).limit(20) 这个查询句就命中了
说明复合索引的查询是和方向相关的，多键排序和方向有关。
```
#### 3.TTL索引
TTL索引是有生命周期的
可以创建ttl索引，制定创建的索引的周期
#### 4.全文本索引
全文本索引的成本很高，比普通索引更严重的性能问题
全文本只对字符串有效
#### 5.地理位置索引
* 1.2d索引同时支持平面和球面几何
* 2.2dsphere 索引只能支持球面几何
在 2dsphere索引上使用球面几何的查询将会更高效和准确，因此我们应该在地理空间字段上使用 2dsphere索引。
使用示例：
```
db.mapinfo.insert({"address" : "南京 禄口国际机场","loc" : { "type": "Point", "coordinates": [118.783799,31.979234]}})
WriteResult({ "nInserted" : 1 })
db.mapinfo.insert({"address" : "南京 浦口公园","loc" : { "type": "Point", "coordinates": [118.639523,32.070078]}})
WriteResult({ "nInserted" : 1 })
db.mapinfo.insert({"address" : "南京 火车站","loc" : { "type": "Point", "coordinates": [118.803032,32.09248]}})
WriteResult({ "nInserted" : 1 })
db.mapinfo.insert({"address" : "南京 新街口","loc" : { "type": "Point", "coordinates": [118.790611,32.047616]}})
WriteResult({ "nInserted" : 1 })
db.mapinfo.insert({"address" : "南京 张府园","loc" : { "type": "Point", "coordinates": [118.790427,32.03722]}})
WriteResult({ "nInserted" : 1 })
db.mapinfo.insert({"address" : "南京 三山街","loc" : { "type": "Point", "coordinates": [118.788135,32.029064]}})
WriteResult({ "nInserted" : 1 })
```
我们插入示例数据
db.mapinfo.ensureIndex( { loc : "2dsphere" } )
#### 建立2dsphere的索引
db.mapinfo.find({    "loc" : {     "$near" : {        "$geometry" : {            "type" : "Point", "coordinates" : [118.783799, 31.979234]             }, "$maxDistance" : 500000         }      }  }).limit(10);
这种查询方式无法获得距离，只能获取查询出的点
db.runCommand( {     geoNear: "mapinfo" ,     near: { type: "Point" , coordinates: [120.783799, 31.979234] } ,     spherical: true,     limit:10  })
可以获取查询到的点到该点的距离