### 事务
```
事务应该具有4个属性：原子性、一致性、隔离性、持久性。这四个属性通常称为ACID特性
事务的隔离级别:
未提交读（READ UNCOMMITED）脏读
已提交读 （READ COMMITED）不可重复读
可重复读（REPEATABLE READ）(innodb默认采用的是可重复读的级别)
可串行化（SERIALIZABLE）
```
####
#### 脏读
```
当一个事务被允许读取另外一个事务修改但未提交的数据时就会发生脏读(事务隔离级别为READ-UNCOMMITTED),示例如下:
t_user表事先插入数据
id age
1  26
session1:
set tx_isolation='READ-UNCOMMITTED';//设置当前session为未提交读
begin;//开启事务
select age from t_user where id =1;//查询数据
接下来:
session2:
set tx_isolation='READ-UNCOMMITTED';
begin;//开启事务
update t_user set age=27 where id =1;//更新这条数据
session1:
select age from t_user where id =1;//这个时候的结果是27
session2:
rollback;//事务回滚，然后session1的事务中id=1的结果就是错的，这个时候就产生幻读
```

                
#### 不可重复读
```
当事务内相同的记录被检索两次，且两次得到的结果不同时，此现象称为不可重复读(事务隔离级别为READ-COMMITTED)
```
#### 幻读
```
在事务执行过程中，另一个事务将新记录添加到正在读取的事务中时，会发生幻读(事务隔离级别为REPEATABLE-READ),
当执行SELECT ...WHERE语句时未对范围锁定，则可能会发生这种情况。幻读是不可重复读的一种特殊情况
在MySQL中增加了间隙锁防止幻读发生，所以在MySQL中事务隔离级别为REPEATABLE-READ，不会发生以上异常现象.
```

#### 比较
```
隔离级别比较：可串行化>可重复读>读已提交>读未提交
隔离级别对性能的影响比较：可串行化>可重复读>读已提交>读未提交
```
