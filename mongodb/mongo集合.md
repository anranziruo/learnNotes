### 集合

#### 
* 1.固定集合
db.createCollection("userFeed",{"capped":true,"size":1000,"max":10})
创建size类型是1000，集合文档数量是10
![示例图1](https://github.com/zhangchao1/learnNotes/blob/master/assets/mongodb/fix_collection.png)
当集合文档是10，插入第11个文档的时候，会删除最老的文档
* 2.可以将固定集合转成非固定集合
db.runCommand({"convertToCapped":"userAdmire","size":10,"max":2})
* 3.固定集合的自然排序
![示例图1](https://github.com/zhangchao1/learnNotes/blob/master/assets/mongodb/fix_collection.png)
-1是新到旧，1是从旧到新
* 4.循环游标
循环游标只能用于固定集合
* 5.集合缺少_id索引的集合
创建的时候加入autoIndexId:true
#### MongoDB GridFS
这种格式适用于存储文件
* GridFS 也是文件存储的一种方式，但是它是存储在MonoDB的集合中。
* GridFS 可以更好的存储大于16M的文件。
* GridFS 会将大文件对象分割成多个小的chunk(文件片段),一般为256k/个,每个chunk将作为MongoDB的一个文档(document)被存储在chunks集合中。
* ridFS 用两个集合来存储一个文件：fs.files与fs.chunks。
* 每个文件的实际内容被存在chunks(二进制数据)中,和文件有关的meta数据(filename,content_type,还有用户自定义的属性)将会被存在files集合中
* GridFS is useful not only for storing files that exceed 16 MB but also for storing any files for which you want access without having to load the entire file into memory. See also When to Use GridFS. 
* GridFS是可以存储任何你想要存储的文件，不仅仅是那些超过16M文件，意思是什么文件都能存
* GridFS使用上的每个块索引和文件集合的效率。驱动程序符合GridFS规范自动创建这些索引方便。您还可以创建任何额外索引所需的适合您的应用程序的需求。