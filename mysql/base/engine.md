### InnoDB和MyISAM
```
1.MyISAM不支持事务，而InnoDB支持。InnoDB的AUTOCOMMIT默认是打开的，即每条SQL语句会默认被封装成一个事务，自动提交，这样会影响速度，所以最好是把多条SQL语句显示放在begin和commit之间，组成一个事务去提交
2.InnoDB支持数据行锁定，MyISAM不支持行锁定，只支持锁定整个表。即 MyISAM同一个表上的读锁和写锁是互斥的，MyISAM并发读写时如果等待队列中既有读请求又有写请求，默认写请求的优先级高，即使读请求先到，所以 MyISAM不适合于有大量查询和修改并存的情况，那样查询进程会长时间阻塞。因为MyISAM是锁表，所以某项读操作比较耗时会使其他写进程饿死。
3.InnoDB支持外键，MyISAM不支持。
4.InnoDB的主键范围更大，最大是MyISAM的2倍
```
