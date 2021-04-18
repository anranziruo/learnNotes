#### ON DUPLICATE KEY UPDATE的作用
```
防止插入重复记录,使用的时候保证有一个字段是唯一不能重复的
如果你插入的记录导致一个UNIQUE索引或者primary key(主键)出现重复，那么就会认为该条记录存在，则执行update语句而不是insert语句，反之，则执行insert语句而不是更新语句。所以 ON DUPLICATE KEY UPDATE是不能写where条件的
```
