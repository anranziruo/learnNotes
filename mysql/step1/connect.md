## 查看数据库连接数
```
show processlis
如果是root帐号，你能看到所有用户的当前连接。如果是其它普通帐号，只能看到自己占用的连接。
show processlist;只列出前100条，如果想全列出请使用show full processlist;
```