### mvcc
```
MVCC最重要的核心便是解决了读写直接不阻塞的问题，提高了事务之间的并发性,
根据每一行记录上的事务ID进行比较来 判断记录是否可见的
```
#### session1
```
select age from t_user where id = 3;return age=29;
beigin;
update t_user set age=30 where id =3;
commit;
```
#### session2
```
begin;
select age from t_user where id = 3;//在session1更新数据，但是未提交
commit;
select age from t_user where id = 3;//在session2更新数据，提交事务
```
#### 总结(不同隔离级别的输出)
```
READ-UNCOMMITTED:session1提交前后,session2的输出结果都是30
READ-CPMMOTTED:session1提交前是session2输出是29，提交后输出是30
REPEATABLE-READ,SERIALIZABLE:session1提交前后，session1的输出结果都是29
```

#### innodb的数据行实现
```
DB_ROW_ID:如果表中没有显式定义主键或者没有唯一索引，则MySQL会自动创建一个6字节的rowid存在记录中。
DB_TRX_ID:事务ID。
DB_ROLL_PTR:回滚段指针,则表示指向该行回滚段的指针，该行上所有旧的版本，在undo中都通过链表的形式组织，
而该值，正式指向undo中该行的历史记录链表
```

#### readview的实现
```
min_trx_id:read view 生成时，活跃事务id列表中最小的id
max_trx_id:read view 生成时，数据库即将分配的事务id，就是当前已创建的最大事务id+1

对于READ-COMMITTED隔离级别，事务内的每一条查询语句都会重新创建Read View，这样就会产生不可重复读现象
对于REPEATABLE-READ隔离级别，事务内第一条语句执行时会创建Read View，在事务结束这段时间内每一次查询都不会重新创建Read View，从而实现了可重复读
```
判断标准如下:
```
trx_id<min_trx_id:说明该版本的对于当前事务来说，是对于已提交事务生成,那么对于当前事务可见
trx_id>=max_trx_id:对于当前事务来说,是不可见的，属于即将发生的事务
min<trx_id<max_trx_id:则判断TRX_ID值是否在Read View中。
如果在Read View中，则根据行记录上的回滚段指针找到回滚段中的对应记录且取出TRX_ID值(说明此行记录在事务开始时处于活跃状态);如果不是，则返回记录
```
