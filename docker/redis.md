##### redis 数据容器

##### docker-composer文件
```
db_redis:
      image: "redis:5.0.10"
      ports:
      - "6379:6379"
      restart: always
      command: redis-server --requirepass 123456 --appendonly yes //设置启动有密码验证
      volumes:
        - /home/cr7/install/redis:/data //rdb文件的目录映射
```