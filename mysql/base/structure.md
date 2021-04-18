## 内容
```
DDL（Data Definition Languages）语句：即数据库定义语句，用来创建数据库中的表、索引、视图、存储过程、触发器等，常用的语句关键字有：CREATE,ALTER,DROP,TRUNCATE,COMMENT,RENAME。增删改表的结构(不可以回滚)
DML（Data Manipulation Language）语句：即数据操纵语句，用来查询、添加、更新、删除等，常用的语句关键字有：SELECT,INSERT,UPDATE,DELETE,MERGE,CALL,EXPLAIN PLAN,LOCK TABLE,包括通用性的增删改查。增删改表的数据(可以回滚)
DCL（Data Control Language）语句：即数据控制语句，用于授权/撤销数据库及其字段的权限（DCL is short name of Data Control Language which includes commands such as GRANT and mostly concerned with rights, permissions and other controls of the database system.）。常用的语句关键字有：GRANT,REVOKE。
TCL（Transaction Control Language）语句：事务控制语句，用于控制事务，常用的语句关键字有：COMMIT,ROLLBACK,SAVEPOINT,SET TRANSACTION。
```
### Mysql之drop、delete、truncate的区别
```
delete 是 DML 语句，操作完以后如果没有不想提交事务还可以回滚，
truncate 和 drop 是 DDL 语句，操作完马上生效，不能回滚
执行速度:一般，drop> truncate > delete
1.想删除部分数据用delete，注意带上where子句，回滚段要足够
2.删除表用drop
3.删除所有的数据，但是保留表用truncate
```

